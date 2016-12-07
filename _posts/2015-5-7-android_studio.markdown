---
layout: post
title:  Android Studio, Java
category: technology 
---

## Android Studio配置

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

离线升级的方法：

1）查看最新的版本号

以0.8.9到1.1 Beta4为例，通过阅读https://dl.google.com/android/studio/patches/updates.xml的内容可知，0.8.9最多只能升级到0.9.9。要想升级到1.1 Beta4，就必须以0.9.9为跳板一步步的向上升。

最终升级路径如下：

0.8.9->0.9.9->1.0.2->1.1 Beta4

2）下载更新包

https://dl.google.com/android/studio/patches/AI-130.737825-132.843336-patch-win.jar

如果PC是Linux平台的话，patch包名称的OS字段是unix。

3）安装更新包

键入如下命令：

`java -Xmx1024m -classpath AI-130.737825-132.843336-patch-win.jar com.intellij.updater.Runner install <Android Studio path>`

一般不要将jar直接放在Android Studio path下，有的时候会出错。

随着软件越来越大，Java VM所需的空间也越来越大，如果出现如下错误：

`java.lang.OutOfMemoryError: Java heap space`

可以通过调整`-Xmx1024m`的值，解决该问题，默认是896m。

查阅相关资料，可知32位OS的Java VM内存有上限，网上说是1.5G，但我这里，1G就是上限了。

## 完整安装包

https://dl.google.com/dl/android/studio/ide-zips/2.1.2.0/android-studio-ide-143.2915827-linux.zip

## Eclipse导出至Android Studio

将之前开发的旧工程导入Android Studio，出现 INSTALL_PARSE_FAILED_MANIFEST_MALFORMED的问题，这个是由于AndroidManifest.xml文件内容有问题导致的。最常见的原因是activity所在的包的名字里有大写字母，但奇怪的是activity本身有大写字母却是无所谓的。

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

## 有用的类

android.media.MediaPlayer 播放多媒体文件

# 手势问题

Android作为一个手机OS，从API Level 1开始就提供了手势相关的类android.view.GestureDetector，以及两个接口GestureDetector.OnDoubleTapListener和GestureDetector.OnGestureListener。相关的用法不赘述。这里仅对onFling()事件一直就没有被捕捉到说一下。

原因有二：

1）我们需要在onCreate中tv.setOnTouchListener(this);之后添加如下一句代码。

`tv.setLongClickable(true);`

只有这样，view才能够处理不同于Tap（轻触）的hold（即ACTION_MOVE，或者多个ACTION_DOWN），我们同样可以通过layout定义中的android:longClickable来做到这一点。

2）如果1不行的话，把onScroll() Return True。这个多半都是由于ScrollView对消息的处理导致的。

# 使用Sqlite数据库

## 1--基本的使用方式主要有两种:

1)纯SQL语句。这种方式只需要掌握execSQL和query两个函数即可。前者用于执行所有的非SELECT语句，而后者用于执行SELECT语句。这种方式适合于熟悉关系型数据库操作，但对程序设计稍差一些的程序员。

2)SQLiteOpenHelper。这种方式提供了更高级的对象型数据库的接口（而不是关系型数据库接口）。适合于那些对关系型数据库操作不太了解，但对程序设计的结构很清晰的程序员。

## 2--getReadableDatabase函数。

刚看到这个函数还以为是打开一个只读的数据库对象，其实不然，需要注意一下。
   
# 各种特效的代码

1.http://www.23code.com/

这个网站是各种特效的搜集站。

2.https://github.com/maarek/android-wheel.git

电表或水表那样的表盘式选择器控件。

3.https://github.com/jgilfelt/android-viewbadger.git

按钮或者View的角标控件。

http://blog.csdn.net/zbunix/article/details/8460422

# 下载AOSP源代码

这里使用清华大学提供的AOSP镜像。

1.下载 repo

`git://aosp.tuna.tsinghua.edu.cn/android/git-repo.git/`

2.修改repo

google的地址

`REPO_URL = 'https://gerrit.googlesource.com/git-repo'`

改为清华大学的地址

`REPO_URL = 'git://aosp.tuna.tsinghua.edu.cn/android/git-repo'`

3.下载 manifest

google 的地址

`$ repo init -u https://android.googlesource.com/platform/manifest`

改为清华大学的地址

`$ repo init -u git://aosp.tuna.tsinghua.edu.cn/android/platform/manifest`

4.同步源码

还是和以前一样

`$ repo sync`

5.替换已有的AOSP源代码的remote

如果你之前已经通过某种途径获得了AOSP的源码，但是你希望以后通过TUNA同步，只需要将.repo/manifest.xml中的

{% highlight xml %}
<remote  name="aosp"
fetch=".."
review="https://android-review.googlesource.com/" />
{% endhighlight %}

改为下面的code即可：

{% highlight xml %}
<remote  name="aosp"
fetch="git://aosp.tuna.tsinghua.edu.cn/android/"
review="https://android-review.googlesource.com/" />
{% endhighlight %}

这个方法也可以用来在同步Cyanogenmod代码的时候从TUNA同步部分代码。

以下是中科大的aosp源，提出的修改办法：

修改 .repo/manifests.git/config，将

`url = https://android.googlesource.com/platform/manifest`

修改成

`url = git://mirrors.ustc.edu.cn/aosp/platform/manifest`

即可。 

这种方法比清华的方法好，因为.repo/manifest.xml是已经被git版本控制的文件，修改它会导致repo库今后无法更新，而中科大的方法没有这个副作用。

6.其他源

aosp的源，除了清华之外，还有以下的源，修改方法参见第5条：

git://mirrors.ustc.edu.cn/aosp/ （中科大的源）

git://git.omapzoom.org/

https://android-git.linaro.org/git/

推荐使用中科大的源，作为清华的替代品。

# Android上的PC模拟器

Exagear、Winulator等，可以用来玩PC老游戏。

# Java

## Java反射机制

Java反射机制容许程序在运行时加载、探知、使用编译期间完全未知的classes。参见：

http://blog.csdn.net/yongjian1092/article/details/7364451

反射机制的一个副产品是可以访问私有变量和私有方法。（笔试题的常客）

http://blog.csdn.net/nisaijie/article/details/5692901

## Ubuntu安装Eclipse、Spring

1.安装Eclipse

`sudo apt-get install eclipse`

2.安装Spring

`sudo apt-get install libspring-web-portlet-java`

注意：ubuntu软件仓库中还有一个叫做spring的游戏引擎，不要弄错了。

https://github.com/spring-guides/gs-rest-service

http://www.mkyong.com/spring/quick-start-maven-spring-example/

## JavaFX

使用Java开发GUI是一个很冷门的技能树，即使是多年的Java程序员，也未必接触过这个领域。我上次接触该技能，还是10年前的AWT时代。

Java的GUI框架，大致有：

1.awt。最古老的框架。

2.swt。Eclipse项目的早期产物，现在Eclipse也不用它了。

2.swing。和swt差不多同时期的东西，Sun的产物。

3.JavaFX。目前最新的GUI框架，其设计思路和实现，均与WPF是同一档次，已经成为官方推荐的标准框架，并集成到JRE中。

## Ubuntu安装Oracle JDK

`sudo apt-get install software-properties-common`

`sudo add-apt-repository ppa:webupd8team/java`

这一步，可能会出现安装密钥失败的情况。不过不要紧，后面安装jdk的时候，手动选择Yes就可以了。

`sudo apt-get update`

`sudo apt-get install oracle-java7-installer`

这样安装之后，还需要设置环境变量JAVA_HOME。（在/etc/profile中）

`export JAVA_HOME=/usr/lib/jvm/default-java/`

`cd /usr/lib/jvm`

`sudo ln -s java-7-oracle default-java`

## Java的注释型配置

Spring等框架中，有很多配置项是以注释的形式出现在代码中，这也成为Java代码中很有特色的一点。

早期的Java代码中，虽然已经开始使用注释，然而这时的注释并不参与自身程序的运行，而主要是为第三方程序提供帮助，比如最著名的Java Doc。

从Java 1.5之后，JDK开始提供相关的注释接口。参见：

http://www.open-open.com/lib/view/open1353144218545.html

还有一个叫做ADP4J用于简化处理注释。参见：

http://www.open-open.com/lib/view/open1369278937654.html

## main函数

和一般的C/C++程序只有一个main函数不同，Java允许每个类都有自己的main函数。只要在程序执行时，指定使用某个类的main函数即可，这个类被称作“主类”。

这种设计方式对调试程序、模块测试很有帮助。我们可以设定某个小模块中的类为主类，来单独测试该模块，而不必将整个系统运行起来。


