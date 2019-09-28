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

IntelliJ IDEA安装更新包的方法与Android Studio一致，但需要添加log4j.jar到classpath。

# 完整安装包

https://dl.google.com/dl/android/studio/ide-zips/2.1.2.0/android-studio-ide-143.2915827-linux.zip

# Eclipse导出至Android Studio

将之前开发的旧工程导入Android Studio，出现 INSTALL_PARSE_FAILED_MANIFEST_MALFORMED的问题，这个是由于AndroidManifest.xml文件内容有问题导致的。最常见的原因是activity所在的包的名字里有大写字母，但奇怪的是activity本身有大写字母却是无所谓的。

# 关于一个语法现象

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

# Android上的PC模拟器

Exagear、Winulator等，可以用来玩PC老游戏。

# 有用的类

android.media.MediaPlayer 播放多媒体文件

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

# Linaro

Linaro虽然名义上是一家非营利性质的开放源代码软件工程公司。然而，它所提供的ARM工具链，基本上已经是御用级别的了。

官网：

https://www.linaro.org/

# Yocto

Yocto是一个开源协作项目，它提供了一些模板、工具和方法来支持面向嵌入式产品的自定义Linux系统。

官网：

https://www.yoctoproject.org/

它也提供了一套工具链，依附于旗下的子项目：Poky Linux。似乎NXP用的较多。

参考：

https://www.ibm.com/developerworks/cn/linux/l-yocto-linux/index.html

使用Yocto Project构建自定义嵌入式Linux发行版

# Retrofit

Retrofit是一套RESTful架构的Android(Java)客户端实现，基于注解，提供JSON to POJO(Plain Ordinary Java Object,简单Java对象)，POJO to JSON，网络请求(POST，GET,PUT，DELETE等)封装。

官网：

http://square.github.io/retrofit/

参考：

www.cnblogs.com/xinye/p/4375936.html

Retrofit

# OkHttp

OkHttp和Retrofit一样，也是square的作品，它提供了一个比HttpURLConnection更友好强大的HTTP/HTTP2 API。

官网：

http://square.github.io/okhttp/

参考：

http://www.cnblogs.com/ct2011/p/4001708.html

OkHttp使用介绍

# ButterKnife

这个开源库可以让我们从大量的findViewById()和setOnClickListener()中解放出来。这里用到了Java的annotation语法。

参考：

http://www.cnblogs.com/flyme/p/4517560.html

推荐一个Android开发懒人库--ButterKnife

# 各种特效的代码

http://www.23code.com/

这个网站是各种特效的搜集站。

https://github.com/maarek/android-wheel.git

电表或水表那样的表盘式选择器控件。

https://github.com/jgilfelt/android-viewbadger.git

按钮或者View的角标控件。

https://blog.csdn.net/u011387817/article/details/94607919

Android自定义Drawable第十四式之百步穿杨
