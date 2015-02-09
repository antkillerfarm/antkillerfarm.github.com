---
layout: post
title:  Android研究
category: technology 
---

## 1.JNI

(1)Java call Native C

JNI的基本概念可以参考以下文献：

[http://blog.csdn.net/believefym/archive/2007/06/08/1644635.aspx](http://blog.csdn.net/believefym/archive/2007/06/08/1644635.aspx)

 这里需要注意的是javah命令处理的是.class文件，而不是.java文件。你需要指定package的路径和package名。javap命令也有类似的要求。

(2)Native C call Java

基本方法参考下列文献：

[http://icepeer.itpub.net/post/3982/19158](http://icepeer.itpub.net/post/3982/19158)

注意事项：

使用构造函数时，JDK1.1的写法是：

`jmethodID mid = (*env)->GetMethodID(env,cls,"","(Ljava/lang/String;)V");`

而JDK1.2的写法是：

`jmethodID mid = (*env)->GetMethodID(env,cls,"","(Ljava/lang/String;)V");`

(3)Java call Native C & Native C callback Java

Android由于提供的是Java接口，所以这里最常见的情况是

1)Java启动程序。

2)Java调用Native C。

3)Native C回调Java。

有上面的编程经验，这个也不是太难的事。和(2)中的区别在于：(2)中由于程序是用C启动的，所以在调用Java之前，需要启动JVM，并实例化相关的Java对象，而(3)中例子由于程序是用Java启动的，在C回调Java时，JVM已经启动，相关的Java对象实例也已存在，所以做法要简单的多。

使用javah生成的.h文件中，函数的声明在Java中声明的参数之前，还有两个参数JNIEnv和jobject。env是调用Java方法的接口，它的功能非常多。jobject传入的是调用Native C的那个Java对象实例的引用。jobject指针不能在多个线程中共享. 就是说,不能直接在保存一个线程中的jobject指针到全局变量中,然后在另外一个线程中使用它.幸运的是,可以用`gs_object=env->NewGlobalRef(obj);`来将传入的obj保存到gs_object中，从而其他线程可以使用这个gs_object来操纵那个java对象了。

## 2.在Android上编译C程序

(1)风临左岸在这方面颇有建树，以下是他的blog：

[http://blog.sina.com.cn/flza](http://blog.sina.com.cn/flza)

最简单的hello world程序可以参看下面这篇文章的做法。

Android原生(Native)C开发之一：环境搭建篇

[http://blog.sina.com.cn/s/blog_4a0a39c30100auh9.html](http://blog.sina.com.cn/s/blog_4a0a39c30100auh9.html)

这里有个概念要弄清楚，之前的例子都是针对x86-win32平台的，而这里我们要在arm-linux下编程。我这里有个同事，曾将x86-win32下的dll放到Android模拟器中，然后抱怨无法使用JNI，这就是这类错误的一个典型的例子。

还有一点需要注意的是，风临左岸使用的交叉编译工具，所编出的程序虽然可以在模拟器中运行，但却是无法直接用于JNI的，需要使用一定的技巧，可参见以下网页：

Shared library "Hello World!" for Android

[http://honeypod.blogspot.com/2007/12/shared-library-hello-world-for-android.html](http://honeypod.blogspot.com/2007/12/shared-library-hello-world-for-android.html)

从这篇文章可以看出，风临左岸使用的交叉编译工具的动态库的默认格式，和Android平台的动态库的格式是不同的，这也是之前有人说Android无法使用JNI的原因。

更好的办法是直接使用google提供的交叉编译工具。

(2)获得Android的源代码

获得源代码的官方方法如下：

[http://source.android.com/source/download.html](http://source.android.com/source/download.html)

虽然源代码在http://source.android.com下，但用浏览器是无法获得代码的，只能用上文提到的方法才行。

当然诸如www.androidin.com之类的网站也提供了非官方的源代码下载。

源代码的大小有1G左右，解压源代码，在源代码的prebuilt/linux-x86/toolchain/arm-eabi-4.2.1/bin文件夹下可以找到google提供的交叉编译工具。

使用prebuilt工具编译C程序的方法可以参考以下文章：

[http://www.j2medev.com/android/native/200901/20090119222445.html](http://www.j2medev.com/android/native/200901/20090119222445.html)

## 3.Android JNI

正统的做法可以参照以下网页：

[http://www.j2medev.com/android/native/200901/20090119222617.html](http://www.j2medev.com/android/native/200901/20090119222617.html)

当然简单的做法也是有的，可以只解压源代码中的bionic、dalvik、prebuilt这三个文件夹。

需要注意的是，和第三方的交叉编译工具不同，prebuilt下只有相关的头文件而没有相应的.o或.a文件，所以用一般的makefile的话，没有办法编译静态可执行文件，但是动态库文件是可以编的。

这里还有一个奇怪的地方，如果直接使用cc生成.so的话，会出错。而先cc生成.o，再ld生成.so就没问题。不知道这是什么原因，尚需高手指点。

这里需要指出一点，普通的JNI调用使用System.loadLibrary加载动态库，而Android平台要使用System.load加载动态库。这就是很多人套用PC上的语法，但却在Android上失败的原因。

## 4.关于一个语法现象
{% highlight java %}
    AlertDialog dialog = new AlertDialog.Builder(SmartGuider.this)
           .setTitle("设置错误")
           .setMessage("GPS功能未开启，请先开启GPS后再使用本程序！\n\n点击Ok进入系统设置页面，点击Cancel退出程序。")
           .create();
{% endhighlight %}

在apidemo中，有很多类似于这样的语句。刚阅读到的时候，先是觉得是不是语法有误，怎么能在一条语句中，调用两个成员函数。接着一想，这是人家出的demo，怎么可能犯这种低级错误，一处也就罢了，总不可能到处都出这种错吧。

于是就考虑是不是Java的新语法，毕竟Java这种语言从1.0到1.7，已经有8个版本了，像@Override和模板之类的特性都是后来添加进去的。但查了半天也没查到出处。

于是去Android SDK中查看相关类的方法，才发现了奥妙。

{% highlight java %}
AlertDialog create()
AlertDialog.Builder setTitle(CharSequence title)
AlertDialog.Builder setMessage(CharSequence message)
{% endhighlight %}

由于setTitle和setMessage返回的都是AlertDialog.Builder类型的引用（多半就是this），从而使得这样连写的语法得以实现。如果把create放到setTitle或setMessage之前，显然就会出错了。因此这根本就不是什么新语法，而只是一种技巧而已。

在C语言中，其实也有类似的情况，例如

`char *strcpy(char *strDestination,const char *strSource);`

为什么不写成

`void strcpy(char *strDestination,const char *strSource);`

或者

`char *strcpy(const char *strSource);`

呢？

不采用第一种，是因为这样做可以使用诸如`strlen(strcpy(...))`之类的便捷写法。

不采用第二种，当然是和内存分配有关了，这里就不多说了。

## 5.Activity生命周期初探

Android SDK上关于Activity生命周期的图，无疑是权威的，但相关文档中并没有Android手机功能键对Activity生命周期所造成的改变的内容。

我这里通过重载Activity派生类的相关函数，并输出日志的方法，探讨该问题。

先说一下日志，可以使用android.util.Log类来输出日志，eclipse在Debug状态下可以看到输出的日志。输出日志的方法有很多种，他们并不是各自独立的，而是一种信息内容包含的关系，从最简略到最详细依次为ERROR, WARN, INFO, DEBUG, VERBOSE，也就是说ERROR中的日志一定在VERBOSE中也能看到。还有Android运行时生成的日志很多，可以自定义过滤器过滤无关的日志。

这里仅对onCreate、onDestroy、onPause、onRestart、onResume、onStart、onStop进行重载，并输出日志。

1)启动程序

onCreate->onStart->onResume

2)重启动程序(按下左侧的主页键和左侧的拨号按钮，不会关闭当前的Activity)

onRestart->onStart->onResume

3)按下左侧的主页键和左侧的拨号按钮

onPause->onStop

4)按下右侧的返回键

onPause->onStop->onDestroy

5)右侧的挂机按钮

onPause

## 6.添加新的View

利用向导生成的代码中只有一个View，相应的UI布局文件在res/layout/main.xml，找了半天也没找到如何快捷的添加View，于是只好自己手动添加相关的xml文件，好在eclipse可以定制添加的xml文件的模板，于是把main.xml复制一下，做成模板，以后添加就方便了。

## 7.编译Froyo（部分转贴）

****
说点题外话，这个Blog本来打算完全写些自己的原创内容，对于转贴只给链接，但链接这东西，有利有弊。常常过上几个月，原来的链接就不好使了。所以只好有选择的粘贴一些了。
****

 Froyo出来有一阵子了，一时兴起，从官网上git了代码，打算编译。不料根据出错信息得知，Froyo及其以后的版本需要64-bit的OS才能编译。所以只好重新安装64-bit的Ubuntu。

按照官网上的步骤一步一步的做，然后卡在apt-get install sun-java5-jdk上了。出错信息告诉我，找不到这个包。Google了一下，找到以下解决方法：

9.10/10.04 add ubuntu 9.04 line to you /etc/apt/sources.list

{% highlight bash %}
deb http://us.archive.ubuntu.com/ubuntu/ jaunty multiverse
deb http://us.archive.ubuntu.com/ubuntu/ jaunty-updates multiverse
sudo apt-get update
sudo apt-get install sun-java5-jdk
{% endhighlight %}

（注意安装会一直停留在阅读sun的同意书上，使劲按确定都没反应的（确定是文本不是按钮），后来按键盘Tab解决。）

更改预设jdk的方法如下：同理，更改 默认的javac,方法为

{% highlight bash %}
update-alternatives --config java
update-alternatives --config javac
{% endhighlight %}

显示如下，然后键入java-1.5.0-sun的 编号：

有 2 个选项可用于替换项 java (提供 /usr/bin/java)。

  选择       路径                                    优先级  状态
****
* 0            /usr/lib/jvm/java-6-openjdk/jre/bin/java   1061      自动模式

1            /usr/lib/jvm/java-1.5.0-sun/jre/bin/java   53        手动模式

2            /usr/lib/jvm/java-6-openjdk/jre/bin/java   1061      手动模式

查看当前的java版本：

java -version

java version "1.5.0_22"

Java(TM) 2 Runtime Environment, Standard Edition (build 1.5.0_22-b03)

Java HotSpot(TM) Client VM (build 1.5.0_22-b03, mixed mode, sharing)

最郁闷的是，在安装好sun-java5-jdk之后，make的时候，出错信息告诉我，Froyo已经改用Java 1.6了。 

make一切顺利，之后在out/host/linux-x86/bin下找到emulator，但却无法执行，提示缺少AVD device，使用`export ANDROID_PRODUCT_OUT=~/android/out/target/product/generic`解决之。

## 8.JDK 1.6 + eclipse 3.6.2 + ADT 10.0.1

今天捡起许久不曾弄过的Android应用开发，重新来过。

1) JDK 1.6和eclipse 3.6.2顺利安装。

2) ADT 10.0.1的安装碰到麻烦。在Eclipse的菜单栏Help->Install New Software中添加https://dl-ssl.google.com/android/eclipse/下载ADT时，发现还需要安装其他的依赖模块。

按照网上的文章说需要安装如下依赖模块：

Eclipse GEF      - http://download.eclipse.org/tools/gef/updates/releases

Eclipse EMF      - http://download.eclipse.org/modeling/emf/updates/releases

Eclipse GMF      - http://download.eclipse.org/modeling/gmf/updates/releases

Eclipse Webtools - http://download.eclipse.org/webtools/updates

Google eclipse Plugin - http://dl.google.com/eclipse/plugin/3.6

但这些更新都很麻烦，而且有的和eclipse 3.6.2有冲突。一直都试不成功。

试了半天之后，终于发现其实不需要这么麻烦，可以按以下步骤操作：

1)在eclipse的官网下载Eclipse Modeling Tools (includes Incubating components)，这个包已经包含了Eclipse Classic的内容，所以如果下载了它的话，就没必要下载Eclipse Classic了。

2)下载WTP包，WTP就够了，不需要WTP-SDK。

3)下载ADT 10.0.1。

# Android Studio使用心得

## 1.Android Studio配置

又是两年不曾摸过Android的开发了，一切又需要从头开始。这次卷土重来，windows已经不是我首选的平台了，因此以下的内容都是在Ubuntu 14.04下完成的。PS：除了网银、游戏、公司的工作，我现在已经基本不在windows下玩了。

应该说每次重来，google的开发工具都给我不一样的感觉。

2008年的时候，除了下载Eclipse、ADT之外，还需下载若干插件，找起来颇为费事，即使看着攻略也需要大半天的时间。

2011年的时候，插件需要下载的已经不多了，而且多数也不需要特别的设置就能用。

现在(2014年)，所有的插件都被放在了一起，用户只需要下载一个文件，就可以傻瓜式的开始开发了。（当然联网还是必要的，有很多默认安装缺少的插件需要下载，只是这个过程已经自动化，完全不需手工操作。）

由于编译脚本由Ant换成了Gradle,因此需要下载Gradle。但是用Android Studio自动下载，速度很慢，还不如直接到官网下载。

然后把下好的gradle-1.6-bin.zip放置到gradle的下载临时目录才可以了。对应不同系统，此目录为：

1)windows：C:\Documents and Settings\Kiki.J.Hu\.gradle\wrapper\dists\gradle-1.6-bin\72srdo3a5eb3bic159kar72vok\

2)linux：~/.gradle/wrapper/dists/gradle-1.6-bin/72srdo3a5eb3bic159kar72vok

3)mac：~/.gradle/wrapper/dists/gradle-1.6-bin/72srdo3a5eb3bic159kar72vok

## 关于自动更新

Android Studio有增量自动更新功能，虽然不是每次出新版本都要升级，但是其升级系统只支持若干个小版本之间的跳变升级。一但超出这个范围，就需要手动下载离线包来升级了。

离线升级的方法很多地方都有写，这里不再赘述。唯一指出的一点是如果PC是Linux平台的话，patch包名称的OS字段是unix。

以0.8.9到1.1 Beta4为例，通过阅读https://dl.google.com/android/studio/patches/updates.xml的内容可知，0.8.9最多只能升级到0.9.9。要想升级到1.1 Beta4，就必须以0.9.9为跳板一步步的向上升。

最终升级路径如下：

0.8.9->0.9.9->1.0.2->1.1 Beta4

## 2.Eclipse导出至Android Studio

将之前开发的旧工程导入Android Studio，出现 INSTALL_PARSE_FAILED_MANIFEST_MALFORMED的问题，这个是由于AndroidManifest.xml文件内容有问题导致的。最常见的原因是activity所在的包的名字里有大写字母，但奇怪的是activity本身有大写字母却是无所谓的。

## 3.调用计算器
如果是调用系统自带的计算器，在网上搜了一下，可以使用如下代码：
{% highlight java %}
Intent mIntent = new Intent();
mIntent.setClassName("com.android.calculator2",
     "com.android.calculator2.Calculator");
startActivity(mIntent);
{% endhighlight %}

从代码的内容来看，主要是使用Intent启动系统计算器的Activity。但是这个代码在我目前的环境下，却在运行时出现了以下错误：

android.content.ActivityNotFoundException: Unable to find explicit activity class {com.android.calculator2/com.android.calculator2.Calculator};

很显然系统计算器的Activity的名字不对，那么正确的名字是什么呢？

手动打开系统计算器，这时可以在系统log中看到“com.android.calculator2.CalculatorActivity”的字样频繁出现，显然这个才是正确的Activity的名字。

在三星的手机上，计算器的Activity是“com.sec.android.app.popupcalculator.Calculator”。

同样的方法，我们可以找到浏览器的Activity是“com.android.browser.BrowserActivity”。

## 4.手势问题

Android作为一个手机OS，从API Level 1开始就提供了手势相关的类android.view.GestureDetector，以及两个接口GestureDetector.OnDoubleTapListener和GestureDetector.OnGestureListener。相关的用法不赘述。这里仅对onFling()事件一直就没有被捕捉到说一下。

原因有二：

1）我们需要在onCreate中tv.setOnTouchListener(this);之后添加如下一句代码。

`tv.setLongClickable(true);`

只有这样，view才能够处理不同于Tap（轻触）的hold（即ACTION_MOVE，或者多个ACTION_DOWN），我们同样可以通过layout定义中的android:longClickable来做到这一点。

2）如果1不行的话，把onScroll() Return True。这个多半都是由于ScrollView对消息的处理导致的。

## 5.使用Sqlite数据库

### 1--基本的使用方式主要有两种:

1)纯SQL语句。这种方式只需要掌握execSQL和query两个函数即可。前者用于执行所有的非SELECT语句，而后者用于执行SELECT语句。这种方式适合于熟悉关系型数据库操作，但对程序设计稍差一些的程序员。

2)SQLiteOpenHelper。这种方式提供了更高级的对象型数据库的接口（而不是关系型数据库接口）。适合于那些对关系型数据库操作不太了解，但对程序设计的结构很清晰的程序员。

### 2--getReadableDatabase函数。

刚看到这个函数还以为是打开一个只读的数据库对象，其实不然，需要注意一下。
   

