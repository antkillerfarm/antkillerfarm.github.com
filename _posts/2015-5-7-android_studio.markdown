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

离线升级的方法很多地方都有写，这里不再赘述。唯一指出的一点是如果PC是Linux平台的话，patch包名称的OS字段是unix。

以0.8.9到1.1 Beta4为例，通过阅读https://dl.google.com/android/studio/patches/updates.xml的内容可知，0.8.9最多只能升级到0.9.9。要想升级到1.1 Beta4，就必须以0.9.9为跳板一步步的向上升。

最终升级路径如下：

0.8.9->0.9.9->1.0.2->1.1 Beta4

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

这里使用清华大学提供AOSP镜像。

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