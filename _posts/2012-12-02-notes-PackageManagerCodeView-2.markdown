# PackageManager Code View Part 2#

## Content Summary ##

安裝包的安裝與卸載過程。

## The Installation's Procedure ##

安裝package的接口是：

    public abstract void installPackage(
            Uri packageURI, IPackageInstallObserver observer, int flags,
            String installerPackageName);

packageURI用來指示要安裝的package的地址，可以使路徑，亦可以是intent。observer是設置當安裝過程結束后需要調用的囘調函數，installerPackageName是可選的，表示將要安裝的package名字。且看流程圖：

![alt txt](/images/notes/installPackage.png "installPackage.png").

* 在installPackage中，首先通过：
    
        mContext.enforceCallingOrSelfPermission(android.Manifest.permission.INSTALL_PACKAGES, null);
    
    来判断一下，是否擁有INSTALL_PACKAGES的权限。然后通过：
        
        Message msg = mHandler.obtainMessage(INIT_COPY);
        msg.obj = new InstallParams(packageURI, observer, flags,
                    installerPackageName);
        mHandler.sendMessage(msg);
    
    向handler传递一个INIT_COPY的消息和InstallParams对象。

* handler处理INIT_COPY消息。首先判断是否已经將DefaultContainerConnection获取到。如果获取失败，调用InstallParams的serviceError进行错误处理。如果获取到，就将此次安装添加到：

    ArrayList < HandlerParams >  mPendingInstalls = new ArrayList < HandlerParams > ();
    
    这个安装队列中。添加完成后，如果是第一个添加到队列中的安装，就通过handler发送MCS_BOUND消息来启动安装。

* handler处理MCS_BOUND消息。判断mPendingInstalls是否为空，不为空的话，就取得队列中的第一个安装，并且调用InstallParam的父类HandlerParams的startCopy来进行安装。

* startCopy会調用handleStartCopy。handleStartCopy首先会check flag的各个域。然后会生成一个InstallArgs对象，调用copyApk來對將要安裝的Package進行物理拷貝。接著，startCopy里還會調用handleReturnCode，裏面會調用processPendingInstall，裏面會調用installPackageLI，就像之前所說的scanPackageLI類似，會解析這個Package的各種資源，加到PackageManagerService的內部數據結構當中，在此不贅述。

* handleStartCopy调用结束后，handler发送MCS_UNBIND消息。

* handler处理MCS_UNBIND消息，首先会从mPendingInstalls安装队列中清除第一个item。然后判断mPendingInstalls是否为空，如果为空，则断开DefaultContainerConnection连接，如果不为空，则继续通过handler发送MCS _ BOUND消息来启动下一个安装。

## The Deletion's Procedure ##

不管什麽方式的安裝，必須首先調用PackageService里的deletePackage接口，定義如下：

    public abstract void deletePackage(
            String packageName, IPackageDeleteObserver observer, int flags);

packageName為要卸載的Package名稱，當卸載流程走完，PackageManager在內部會調用observer實現的囘調函數來處理卸載結束后的工作。函數實體最終還是在PackageManagerService.java中，且看圖：

![alt txt](/images/notes/deletePackage.png "deletePackage.png").

* 進入了PackageManagerService的deletePackage的函數中，通過mHandler.post來進行異步調用。主要是deletePackageX，然後調用obserer的囘調。
* deletePackageX中再調用deletePackageLI，然後如果是系統升級，則會廣播一些消息給其他模塊。
* 在deletePackageLI，首先在mPackages查找將要刪除的，如果沒有則通報錯誤。如果有，就將其從mPackages里刪除，然後調用removePackageDataLI。
* 在removePackageDataLI中，調用removePackageLI來從相關列表中刪除屬於這個Package的Activity,Reciever,Provider,Service,Permission,以及Instrumentation.然後，通過mInstaller調用installd來對這個Package進行物理刪除。然後對mSettings進行與這個package相關設置處理處理，最後調用mSettings.writeLPr進行設置文件寫入。

## Reference ##

1. [PackageManager分析(4)](http://blog.csdn.net/ljsbuct/article/details/6650977 "PackageManager分析(4)").
2. Android Source Code.
