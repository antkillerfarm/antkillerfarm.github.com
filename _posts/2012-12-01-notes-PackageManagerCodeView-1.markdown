---
layout: post
title: PackageManager Code View Part 1
category: notes
---

# PackageManager Code View Part 1#

## Content Summary ##

* PackageManager的靜態結構.
* 應用程序包詳細信息的獲取.

## Overview ##

PackageManager是用來獲取安裝在設備上的應用程序包的各種信息的模塊。在應用程式開發過程中很少用到，但是非常重要。在系統啟動的時候，PackageManager會讀取檢測手機上已有的所有應用程序包，將相關信息裝進內存，給應用程序提供信息，亦會將相關信息寫入配置文件。下次啟動的時候會直接讀取配置文件，如應用程序包有變化，再更改相關的配置文件。

## How to use PackageManager in Application ##

和其他服務一樣，我們可以從Context類來獲取PackageManager對象：

{% highlight java %}
	PackageManager pm = context.getPackageManager();
{% endhighlight %}

通過pm來調用PackageManager里的接口獲取用戶所需要的信息。這種方法和通常的獲取系統服務的方法是一樣的，唯一的區別的是，獲取PackageManager服務的代碼被封裝在了ActivityThread的getPackageManager方法里：

{% highlight java %}
	public static IPackageManager getPackageManager() {
        if (sPackageManager != null) {
            return sPackageManager;
        }
        IBinder b = ServiceManager.getService("package");
        sPackageManager = IPackageManager.Stub.asInterface(b);
        return sPackageManager;
    }
{% endhighlight %}

## The Architecture of PackageManager ##

先看看有關PackageManager的結構圖：

![alt text](/images/notes/pm-architecture.png "pm-architecture.png")

從上圖可知，PackageManager是個abstract interface類，裏面定義了所有PackageManager提供給用戶的接口，但是需要其他的類繼承才能實現抽象類裡的接口，直接繼承他的是ApplicationPackageManager。其實ApplicationPackageManager也僅僅是個wrapper,這個類中有一個IPackageManager變量，他也是一個接口類，但是她是通過IPackageManager.aidl來定義的。AIDL是Android設計的用來做進程間通信的接口。通過編譯，java編譯器會自動將IPackageManager.aidl轉換成IPackageManager.java,在IPackageManager類裏面內嵌了stub類，而stub 繼承自Binder，以及實現了AIDL interface里定義的方法，因此依靠binder來實進程間服務調用。PackageManagerService通過繼承IPackageManager.stub來實現具體PackageManager功能,並且實現了進程間的調用。我們之後重點研究PackageManagerService。

## The details of PackageManagerService ##

### How to launch the PackageManager Service ###

Framework中會運行一個ServerThread,用來啟動android所需要的所有的Service，調用關係如圖：

![alt text](/images/notes/pm-service-launch.png "pm-service-launch.png")

如圖所示，在PackageManagerService中有靜態方法main來通過調用PackageManagerService的構造函數來初始化此service：

{% highlight java %}
	public static final IPackageManager main(Context context, boolean factoryTest, boolean onlyCore) {
    	PackageManagerService m = new PackageManagerService(context, factoryTest, onlyCore);
    	ServiceManager.addService("package", m);
    	return m;
	}
{% endhighlight %}

### PackageManagerService構造函數 ###

在構造函數PackageManagerService中，會檢測所有的安裝在設備中的應用程序，將相關信息存入相關的數據結構中。我們先來看一下PackageManagerService的構造函數的運行流程概覽：

![alt text](/images/notes/PackageManagerConstructor.png "PackageManagerConstructor.png")

我們逐步分析各個子過程：

* Installer對象的創建
* 建立PackageHandler消息循环
* readPermissions流程
* Settings對象的創建與讀取
* java binary的DexOpt
* 用AppDirObserver來監視app目錄
* scanDirLI流程
* Other Operations

### 1. Installer對象的創建 ###

Installer是一個單獨的類，他主要通過socket與C語言編寫的installd程序交互，主要實現apk安裝卸載，dex的優化。Installer負責通過socket往installd發送消息，而具體操作都是在installd中完成。命令行程序installd執行的命令如下：

{% highlight java %}
	struct cmdinfo cmds[] = {
    	{ "ping",             0, do_ping },
        { "install",          3, do_install },
        { "dexopt",           3, do_dexopt },
        { "movedex",          2, do_move_dex },
        { "rmdex",            1, do_rm_dex },
        { "remove",           1, do_remove },
        { "rename",           2, do_rename },
        { "freecache",        1, do_free_cache },
        { "rmcache",          1, do_rm_cache },
        { "protect",          2, do_protect },
        { "getsize",          3, do_get_size },
        { "rmuserdata",       1, do_rm_user_data },
        { "movefiles",        0, do_movefiles },
     };
{% endhighlight %}

PackageManagerService既直接使用Installer對象，也將通過UserManager間接使用，在UserManager調用構造函數的時候將Installer對象傳進去：

{% highlight java %}
	mInstaller = new Installer();
	......
    mUserManager = new UserManager(mInstaller, mUserAppDataDir);
{% endhighlight %}

在PackageManagerService的其他部份將會通過mUserManager來調用Installer相關的功能。

### 2. 建立PackageHandler消息循环 ###

代碼如下：

{% highlight java %}
	mHandlerThread.start();
    mHandler = new PackageHandler(mHandlerThread.getLooper());
{% endhighlight %}

mHandlerThread是HandlerThread的實例化，HandlerThread繼承自Thread，根據文檔介紹，HandlerThread對於run函數有自己特殊的實現，通過mHandlerThread.getLooper()獲得的Looper傳遞給Handler的構造函數，這樣的話，mHandler的消息處理實際就在HandlerThread里運行了。另外PackageManagerService的Handler所處理的消息如下：

{% highlight java %}
	static final int SEND_PENDING_BROADCAST = 1;
    static final int MCS_BOUND = 3;
    static final int END_COPY = 4;
    static final int INIT_COPY = 5;
    static final int MCS_UNBIND = 6;
    static final int START_CLEANING_PACKAGE = 7;
    static final int FIND_INSTALL_LOC = 8;
    static final int POST_INSTALL = 9;
    static final int MCS_RECONNECT = 10;
    static final int MCS_GIVE_UP = 11;
    static final int UPDATED_MEDIA_STATUS = 12;
    static final int WRITE_SETTINGS = 13;
    static final int WRITE_STOPPED_PACKAGES = 14;
    static final int PACKAGE_VERIFIED = 15;
    static final int CHECK_PENDING_VERIFICATION = 16;
{% endhighlight %}

### 3. readPermissions流程 ###

readPermissions將會讀取/etc/permissions下所有的xml文件，他們是：
    
    android.hardware.location.gps.xml
    android.hardware.touchscreen.multitouch.distinct.xml
    android.hardware.touchscreen.multitouch.xml
    android.hardware.wifi.xml
    com.android.location.provider.xml
    com.augusta.fmradio.xml
    com.google.android.maps.xml
    handheld_core_hardware.xml
    platform.xml

這些文件都囊括在permission與"/permission之間，他們的作用是在低層系統的user IDs，group IDs和高層的被platform管理的權限之間的映射。readPermissions中將對每個每個xml文件調用readPermissionsFromXml，readPermissionsFromXml里主要處理如下tags：

* Permission

這個tag是用來將低層的group IDs和permissions name粘合起來，也可以說做一個映射，比如：

    <permission name="android.permission.BLUETOOTH_ADMIN" >
        <group gid="net_bt_admin" />
    </permission>

對於這樣一個映射，你可以說任何一個被授予這個權限的應用程序(apk)運行起來時，相應的group ID也會附著在這個應用程序的進程上，所以應用程序也具備了這個group所允許的各種操作權限。

在readPermissionsFromXml中，當讀到group字段時，將其後面的gid解析出來，存入PackageManagerService中的變量int[] mGlobalGids中，當然如果有重複將不會存入，然後將讀到permission, 將其後面的name讀出，作為String參數傳入readPermission函數，其定義為：

{% highlight java %}
    void readPermission(XmlPullParser parser, String name)
{% endhighlight %}

在readPermission中，將permission name與相關group ID存入Settings類中的：

{% highlight java %}
    final HashMap<String, BasePermission> mPermissions =
            new HashMap<String, BasePermission>();
{% endhighlight %}

因為permission name與group id有一對n的關係，即每一個permission name會有多個group ID與之附著，所以有一個專門的類BasePermission來管理，裏面會將所有group IDs存入變量：

{% highlight java %}
    int[] gids;
{% endhighlight %}

中，當然也會消重。

* assign-permission

這個tag是用來將高層的權限附著給相應的user IDs, 這樣可以使linux系統用戶能夠擁有framework定義的高級權限。例如，我們給user ID為shell的賦了些權限，那麼所有在shell下運行的，比如adb shell等都有和shell一樣的高層permission, 比如：

     <assign-permission name="android.permission.WRITE_EXTERNAL_STORAGE" uid="shell" />

在readPermissionsFromXml中，當讀到assign-permission時，將assign-permission name與uid分別讀出，存入變量：

{% highlight java %}
    final SparseArray<HashSet<String>> mSystemPermissions =
            new SparseArray<HashSet<String>>();
{% endhighlight %}

mSystemPermissions是以int uid為key，元素為HashSet<String>的數組，每一個uid都可能附著多個assign-permission，所以每一個uid里的HashSet<String>可能會存入多個assign-permission name。

* library

library tag用來標識應用程序apk需要鏈接的一些java庫，他通常有兩種屬性: name和file。顧名思義，name是代碼中鏈接這個庫是用的名字，而file則是這個庫文件的絕對路徑。例如：

    <library name="com.augusta.fmradio"
            file="/system/framework/com.augusta.fmradio.jar"/>

readPermissionsFromXml將解析結果存入類中變量mSharedLibraries中，他的定義如下：

{% highlight java %}
    final HashMap<String, String> mSharedLibraries = new HashMap<String, String>()
{% endhighlight %}

正如前文所述，HashMap中兩個String分別是name和file。

* feature

當系統中增加硬件時，需要這個tag來表示。例如：

    <feature name="android.hardware.location.gps" />

我們將其解析出存入變量:

{% highlight java %}
    final HashMap<String, FeatureInfo> mAvailableFeatures =
            new HashMap<String, FeatureInfo>();
{% endhighlight %}

### 4. Settings對象的創建與讀取 ###

在Settings的構造函數中會建立以下幾個File對象：

{% highlight java %}
    mSettingsFilename = new File(systemDir, "packages.xml");
    mBackupSettingsFilename = new File(systemDir, "packages-backup.xml");
    mPackageListFilename = new File(systemDir, "packages.list");
    mStoppedPackagesFilename = new File(systemDir, "packages-stopped.xml");
    mBackupStoppedPackagesFilename = new File(systemDir, "packages-stopped-backup.xml");
{% endhighlight %}

而systemDir則是"/data/system/"。

然而最關鍵的讀取流程在readLPw函數中，先看流程圖：

![alt text](/images/notes/pm-msettings-readLP.PNG "pm-msettings-readLP.PNG")

如果系統第一次啟動，以上5個xml文件是不存在的，通過掃描所有app目錄下的apk將會生成相關文件。

### 5. java binary的DexOpt ###

DexOpt是Dalvic Virtual machine 提供的命令行工具，用來對java的apk或者jar來進行優化。在PackageManagerService中，通過mInstaller來運行DexOpt。

* 首先遍歷java.boot.class.path的所有目錄，將其下的java binary全部運行DexOpt。
* 然後將mSharedLibraries所保存的所有java jar文件運行DexOpt。
* 最後將/system/framework/下所有的apk,jar進行DexOpt

### 6. 用AppDirObserver來監視app目錄 ###

AppDirObserver繼承自FileObserver，這個FileObserver能夠監控一個文件或者一個目錄下所有文件系統的變化。FileObserver爲什麽擁有如此神力？那是因為身為java類的FileObserver，通過jni對linux inotify系列函數進行了封裝。[Inotify](http://en.wikipedia.org/wiki/Inotify "inotify")是linux kernel中的子系統，專門用來監視文件系統的變化，主要用到以下幾個函數：

{% highlight c %}
    int inotify_init(void);
    int inotify_add_watch(int fd, const char *pathname, uint32_t mask);
    int inotify_rm_watch(int fd, int wd);
{% endhighlight %}

通過調用inotify_init來返回一個inotify的文件句柄，調用inotify _ add _ watch可以添加一個文件系統的路徑，讓句柄為fd的inotify來監視其變化，inotify _a dd _ watch返回一個文件句柄，將這個文件句柄作為參數傳給inotify _ rm _ watch來結束對這個目錄的監視。那麼如何實現的監視呢？通過對之前調用inotify _ init獲得的文件句柄fd，我們用read來對fd讀取，可以讀到一個struct inotify _ event的內存指針, 他可用來標識當前目錄下文件系統的變化，其結構體定義如下：

{% highlight c %}
    struct inotify_event 
    {
        int wd; 		        /* The watch descriptor */
        uint32_t mask; 	        /* Watch mask */
        uint32_t cookie;	    /* A cookie to tie two events together */
        uint32_t len;		    /* The length of the filename found in the name field */
        char name __flexarr;	/* The name of the file, padding to the end with NULs */	
    }
{% endhighlight %}

wd用來表示發生變化的目錄的句柄，mask用來表示具體發生了什麽變化，cookie用來粘合兩個事件，比如要從一個目錄拷貝一個文件到另外一個目錄。len用來表示有變化的文件的路徑的長度，name則表示具體的路徑。繼承自FileObserver的AppDirObserver通過其構造函數將希望被監控的目錄傳進去，然後通過調用FileObserver的接口startWatching來設置監控的目錄，即通過jni調用了inotify _ add_watch函數，然後FileObserver在其內部類ObserverThread的run中進行檢測目錄的變化的工作，即上文提到的通過read來獲取inotify _ event的內存指針的方法。這個過程是通過jni中的C++代碼實現的，在此過程中，每次將檢測結果通過囘調FileOberver的java方法onEvent將結果返回給java空間，所以想要實現自己對於文件變化的處理，AppDirObserver必須實現自己的onEvent。在AppDirObserver的onEvent中，只對REMOVE _ EVENTS和ADD _ EVENTS兩個事件進行了處理，當有REMOVE _ EVENTS事件發生時，說明有apk包被刪除，所以調用removePackageLI函數，當有ADD _ EVENT事件發生時，說明有新的apk被安裝，則調用scanPackageLI函數，掃描新的新增加的文件。最後調用mSettings.writeLPr函數，將增加或者刪除的變化的相關設置寫進相關文件。

下面這個表格列出了PackageManagerService中相關PATH和AppDirObserver的對象，有的PATH沒有用到AppDirObserver來監控:

<table border="1" cellspacing="0" cellpadding="0">
   <tr style="background:#00B0F0">
      <td>Path Parameter</td>
      <td>Absolute Path</td>
      <td>AppDirObserver Object</td>
   </tr>
   <tr>
      <td>mAppDataDir</td>
      <td>/data/data</td>
      <td></td>
   </tr>
   <tr>
      <td>mAppInstallDir</td>
      <td>/data/app</td>
      <td>mAppInstallObserver</td>
   </tr>
   <tr>
      <td>mUserAppDataDir</td>
      <td>/data/user</td>
      <td></td>
   </tr>
   <tr>
      <td>mDrmAppPrivateInstallDir</td>
      <td>/data/app-private</td>
      <td>mDrmAppInstallObserver</td>
   </tr>
   <tr>
      <td>mFrameworkDir</td>
      <td>/system/framework</td>
      <td>mFrameworkInstallObserver</td>
   </tr>
   <tr>
      <td>mDalvikCacheDir</td>
      <td>/data/dalvik-cache</td>
      <td></td>
   </tr>
   <tr>
      <td>mSystemAppDir</td>
      <td>/system/app</td>
      <td>mSystemInstallObserver</td>
   </tr>
   <tr>
      <td>mVendorAppDir</td>
      <td>/vendor/app</td>
      <td>mVendorInstallObserver</td>
   </tr>
</table>

### 7. scanDirLI流程 ###

先來看一下scanDirLI的總體流程圖：

![alt text](/images/notes/scanDirLi_1_stage.png "scanDirLi_1_stage.png")

* 進入scanDirLi后，通過第一個參數scanFile提供的路徑來掃描這個目錄下的所有apk文件，對每一個合法的apk文件調用scanPackageLI。

* 在PackageManagerService下有兩個scanPackageLI，他們的參數不同，在第一個裏面會調用第二個，在流程圖上看，從第二個方框開始就是開始調用了第一個scanPackageLI，一直到最後都在第一個裏面，而後面那個灰色的大方塊則是第二個scanPackageLI的調用。

* 在第一個scanPackageLI里，首先新建了PackageParser對象，然後對這個路徑調用PackageParser裡的parsePackage方法，即黃色方框的部份，并將解析結果保存到PackageParser.Package變量pkg中。在parsePackage中，也和scanPackageLI類似，用了函數名稱一樣，但是參數不同的parsePackage函數。在第一個parsePackage里，通過使用AssetManager，將apk中的AndroidManifest.xml釋放到內存，交給第二個parsePackage去解析。總之，黃方塊里主要是來解析apk文件里的AndroidManifest.xml文件，包括所有的設置，比如權限，以及Activity, Service, Reciever, Provider等，並將結果以PackageParser.Package對象返回。

* 通過調用collectCertificatesLI來與上次掃描的結果的比較來驗證證書是否合法。

下面進入到龐大的第二個scanPackageLI函數中，可以看一下流程圖：

![alt text](/images/notes/scanPackageLI.png "scanPackageLI.png")

* 首先如果是android的原生應用，做一些特殊處理
* 將pkg中的usesLibraries和usesOptionalLibraries中的庫文件與mSharedLibraries里做比較，一致的話都存到pkg.usesLibraryFiles中。
* 在mSharedUsers中查找name為pkg.mSharedUserId的元素，有則返回相應的SharedUserSetting對象，沒有的話新建一個以pkg.mSharedUserId為name的SharedUserSetting對象添加到SharedUserSetting中。
* 處理origPackage的情況，也是system app才有此殊榮。
* 調用verifySignaturesLP進行簽名驗證。
* 將當前的pkg添加到mPackages變量中，mPackages是HashMap<String, PackageParser.Package>型的，以package name為key，以PackageParser.Package為內容。
* 將pkg中的所有Provider添加到mProviders中，mProviders是HashMap<String, PackageParser.Provider>，以provider name為key，以PackageParser.Provider為內容。
* 將pkg中所有的Service信息添加到mServices中。
* 將所有Activity信息添加到mActivities中。
* 將所有的Reciever添加到mReceivers中。
* 將所有的permissionGroups添加到mPermissionGroups中。
* 將所有的permission添加到mSettings.mPermissions中。
* 將所有的instrumentation添加到mInstrumentation中。
* 將所有的Broadcast添加到mProtectedBroadcasts中。

### 8. other operations ###

* 然後清除一些已經不存在的Package，以及安裝失敗的Package。
* 為 "/data/app/" 與 "/data/app-private" 添加AppDirObserver監控，而且分別調用scandirLI來掃描。

* 調用mSettings.writeLPr()來寫入相關設置：

1. 如果packages.xml文件存在且packages-backup.xml不存在，則將packages.xml重命名為packages-backup.xml，如果packages-backup.xml和packages.xml同事存在，則將packages.xml刪除。
2. 準備開始生成package.xml，里面记录了系统的permissions以及每个package的name ,codePath, pkgFlags, timeStamp, versionCode,uesrid,permission-trees, permissions,等信息，这些信息主要通过apk的AndroidManifest.xml解析获取。
3. 生成packages.list,  我們在每一行寫下如下信息：
                    
    * pkgName    ---- package name
    * userId     ---- application-specific user id
    * debugFlag  ---- 0 or 1 if the package is debuggable.
    * dataPath   ---- path to package's data path
    
        例如：
        
        com.android.gallery3d 10003 0 \data\data\com.android.gallery3d                  
        com.android.exchange 10028 0  \data\data\com.android.exchange                     


4. 最後將因為某種原因沒運行起來的package相關信息存入packages-stopped.xml

## 參考資料 ##

1. [Android PackageManagerService详细分析](http://blog.csdn.net/andy_android/article/details/7245602 "Android PackageManagerService详细分析").
2. [Android Develop Guide](http://developer.android.com/develop/index.html "Android Develop Guide").
3. [Android内核剖析](http://book.douban.com/subject/6811238/ "Android内核剖析").
