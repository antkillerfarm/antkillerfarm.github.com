---
layout: post
title:  Android Studio
category: technology 
---

# Android Studio配置

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

# 关于自动更新

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

# Java反射机制

Java反射机制容许程序在运行时加载、探知、使用编译期间完全未知的classes。参见：

http://blog.csdn.net/yongjian1092/article/details/7364451

反射机制的一个副产品是可以访问私有变量和私有方法。（笔试题的常客）

http://blog.csdn.net/nisaijie/article/details/5692901

# Java构建工具

构建工具的意义在于，提供一种独立于IDE的软件构建方式。而且通常来说，构建工具更适合特大项目的构建。比如，即使是以功能强大著称的Visual Studio，也提供nmake用以处理特大项目。

常用的Java构建工具主要有：Ant、Maven、Gradle。

## Ant

Ant作为最早的Java构建工具，使用最为广泛，一经使用即取代了之前的IDE工程文件。

PS：那个时代最著名的Java IDE估计要算Borland的JBuilder了。

Ant使用XML语言格式的配置文件，并可结合Ivy用以处理网络式的依赖管理。

Ant入门教程：

http://ant.apache.org/manual/tutorial-HelloWorldWithAnt.html

从设计思想来看，Ant非常类似make。除了建立依赖关系树之外，其他方面完全不限制编写者的发挥。因此，Ant的自由度很高，但缺点就是编写难度和make也类似。

代码示例：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/java_build/ant

## Maven

Maven仍旧使用XML作为配置文件格式，但使用内置规则简化脚本的编写。

Maven最大的优点是，具备从网络上自动下载依赖的能力。比如，最常用的maven repository：

https://repository.apache.org/

Maven入门教程：

http://www.oracle.com/technetwork/cn/community/java/apache-maven-getting-started-1-406235-zhs.html

Maven的缺点是规则的力量过于强大，对于规则覆盖不了的情况，很难处理。但好在多数项目并没有那么复杂的情况。

代码示例：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/java_build/maven

## Gradle

XML作为配置文件格式，主要有两个缺点：

1.各种标记占用的空间较大，不简洁。有用的信息淹没在各种标记中，反而降低了配置文件的可读性。

2.XML本身的树状结构，对于条件编译（例如if分支）的情况，没有什么好的语法组织形式。

针对以上问题，Gradle使用Groovy语言作为配置文件格式。它自2012年诞生以来，获得了快速普及，尤其是Google采用Gradle作为Android应用的默认构建工具。

Gradle的设计思想已经脱离了make的范畴。make脚本也好，XML也好，都不是通用程序语言，无论设置再多的规则，也总有满足不了需求的时候。因此，Gradle使用Groovy这种通用程序语言来提供灵活性，同时使用plugin，尤其是官方提供的plugin，来设定规则。就好比是C语言+标准库，这样的组合够强吧。

premake实际上也是类似的设计思路。

代码示例：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/java_build/gradle

# Ubuntu安装Oracle JDK

`sudo apt-get install software-properties-common`

`sudo add-apt-repository ppa:webupd8team/java`

这一步，可能会出现安装密钥失败的情况。不过不要紧，后面安装jdk的时候，手动选择Yes就可以了。

`sudo apt-get update`

`sudo apt-get install oracle-java7-installer`

这样安装之后，还需要设置环境变量JAVA_HOME。（在/etc/profile中）

`export JAVA_HOME=/usr/lib/jvm/default-java/`

`sudo ln -s /usr/lib/jvm/java-7-oracle /usr/lib/jvm/default-java/`

