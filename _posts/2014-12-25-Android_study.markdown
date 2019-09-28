---
layout: post
title:  Android研究（一）
category: technology 
---

# Android研究

## JNI

(1)Java call Native C

JNI的基本概念可以参考以下文献：

http://blog.csdn.net/believefym/archive/2007/06/08/1644635.aspx

eclipse下jni初试

 这里需要注意的是javah命令处理的是.class文件，而不是.java文件。你需要指定package的路径和package名。javap命令也有类似的要求。

(2)Native C call Java

基本方法参考下列文献：

http://blog.itpub.net/60408/viewspace-790731/

JNI concept(介绍java call c 和C call java的方法)

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

## 在Android上编译C程序

(1)风临左岸在这方面颇有建树，以下是他的blog：

http://blog.sina.com.cn/flza

风子的博客

最简单的hello world程序可以参看下面这篇文章的做法。

http://blog.sina.com.cn/s/blog_4a0a39c30100auh9.html

Android原生(Native)C开发之一：环境搭建篇

这里有个概念要弄清楚，之前的例子都是针对x86-win32平台的，而这里我们要在arm-linux下编程。我这里有个同事，曾将x86-win32下的dll放到Android模拟器中，然后抱怨无法使用JNI，这就是这类错误的一个典型的例子。

还有一点需要注意的是，风临左岸使用的交叉编译工具，所编出的程序虽然可以在模拟器中运行，但却是无法直接用于JNI的，需要使用一定的技巧，可参见以下网页：

Shared library "Hello World!" for Android

http://honeypod.blogspot.com/2007/12/shared-library-hello-world-for-android.html

从这篇文章可以看出，风临左岸使用的交叉编译工具的动态库的默认格式，和Android平台的动态库的格式是不同的，这也是之前有人说Android无法使用JNI的原因。

更好的办法是直接使用google提供的交叉编译工具。

(2)获得Android的源代码

获得源代码的官方方法如下：

http://source.android.com/source/download.html

虽然源代码在http://source.android.com下，但用浏览器是无法获得代码的，只能用上文提到的方法才行。

当然诸如www.androidin.com之类的网站也提供了非官方的源代码下载。

源代码的大小有1G左右，解压源代码，在源代码的prebuilt/linux-x86/toolchain/arm-eabi-4.2.1/bin文件夹下可以找到google提供的交叉编译工具。

使用prebuilt工具编译C程序的方法可以参考以下文章：

http://www.j2medev.com/android/native/200901/20090119222445.html

## Android JNI

正统的做法可以参照以下网页：

http://www.j2medev.com/android/native/200901/20090119222617.html

当然简单的做法也是有的，可以只解压源代码中的bionic、dalvik、prebuilt这三个文件夹。

需要注意的是，和第三方的交叉编译工具不同，prebuilt下只有相关的头文件而没有相应的.o或.a文件，所以用一般的makefile的话，没有办法编译静态可执行文件，但是动态库文件是可以编的。

这里还有一个奇怪的地方，如果直接使用cc生成.so的话，会出错。而先cc生成.o，再ld生成.so就没问题。不知道这是什么原因，尚需高手指点。

这里需要指出一点，普通的JNI调用使用System.loadLibrary加载动态库，而Android平台要使用System.load加载动态库。这就是很多人套用PC上的语法，但却在Android上失败的原因。

## Activity生命周期初探

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

## 添加新的View

利用向导生成的代码中只有一个View，相应的UI布局文件在res/layout/main.xml，找了半天也没找到如何快捷的添加View，于是只好自己手动添加相关的xml文件，好在eclipse可以定制添加的xml文件的模板，于是把main.xml复制一下，做成模板，以后添加就方便了。

## 编译Froyo

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

{% highlight bash %}
选择       路径                                    优先级  状态
* 0   /usr/lib/jvm/java-6-openjdk/jre/bin/java   1061    自动模式
1     /usr/lib/jvm/java-1.5.0-sun/jre/bin/java   53      手动模式
2     /usr/lib/jvm/java-6-openjdk/jre/bin/java   1061    手动模式
{% endhighlight %}

查看当前的java版本：

`java -version`

最郁闷的是，在安装好sun-java5-jdk之后，make的时候，出错信息告诉我，Froyo已经改用Java 1.6了。 

make一切顺利，之后在out/host/linux-x86/bin下找到emulator，但却无法执行，提示缺少AVD device，使用`export ANDROID_PRODUCT_OUT=~/android/out/target/product/generic`解决之。

## JDK 1.6 + eclipse 3.6.2 + ADT 10.0.1

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

## 调用计算器

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

## m & mm & mmm

m：编译所有的模块

mm：编译当前目录下的模块，当前目录下要有Android.mk文件

mmm：编译指定路径下的模块，指定路径下要有Android.mk文件
