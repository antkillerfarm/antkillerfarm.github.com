---
layout: post
title:  Ubuntu使用技巧（二）, Java构建工具
category: technology 
---

# Ubuntu 16.04使用手记

Ubuntu 16.04正式发布（2016.4.21）之后，我第一时间下载了下来。

平心而论，虽然厂商已经很努力，但是Ubuntu的版本升级，仍然存在诸多不兼容的问题。我的电脑最初装的是12.04，后来利用apt升级为14.04。然而，从这次的升级体验来说，不仅升级耗时远比重新安装多，而且有些软件并不能自动升级到新版本，因此，就存在和Ubuntu新版本的兼容问题。而这次16.04的升级更绝，我升级之后，电脑直接不能开机了。因此，必须重新安装Ubuntu。

硬盘安装Ubuntu 16.04的步骤，与之前的版本完全相同，不再赘述。

总的来说，这次的升级没有大的变化，但小的改进还是不少的。

1.内核版本升级到4.4。这个太没存在感了，囧。

2.LibreOffice升级到5.X。外观上更简约了，赞一个。

3.Emacs升级到24.5。提一个细节，以前打开同名文件，文件名的后面按照打开顺序加序号，以示区别，但序号含义很不直观。现在用父文件夹名来区分，好用多了。

下面对使用中遇到的问题，及其解决方法，总结如下：

1.安装Flash插件。

有两个办法——要么安装Chrome，要么将Adobe官方驱动中的libflashplayer.so，安装到~/.mozilla/plugins下。

2.gvfsd-smb-browse进程的CPU占用100%。

这是一个ISP DNS导致的问题。其中一个解决方法：

`sudo apt-get remove gvfs-backends`

# 清理系统

## 清理安装包

`sudo apt-get clean`

## 清理旧内核

1.首先查看旧内核情况

`dpkg --get-selections | grep linux`

2.删除旧内核

`sudo apt-get purge linux-image-4.4.0-21-generic linux-headers-4.4.0-21`

# Graphviz

Graphviz是一个绘“图”软件。这里的“图”不是指一般意义上的图片或者照片，而是指数据结构或者图论中的抽象意义上的数学概念中的“图”。Graphviz就是一个用来将“图”可视化的工具集。

官网：

http://graphviz.org/

源代码：

https://github.com/ellson/graphviz/

安装方法：

`sudo apt-get install graphviz graphviz-dev`

其中，后者是graphviz的开发工具包，便于其他软件集成graphviz的相关功能。

graphviz包括了以下工具：

## 图形生成类

按照布局方式的不同，包括circo、dot、fdp、neato、osage、sfdp、twopi等命令。例如：

`dot -Tpng hello.gv -o hello.png`

该命令将hello.gv按照dot布局方式输出成png文件。

## 图形查看类

1.xdot

`sudo apt-get install xdot`

这个工具功能简单，只能按照dot布局方式查看文件。

2.kgraphviewer

`sudo apt-get install kgraphviewer-dev`

这个工具可以选择查看的布局方式。

## 编辑类

包括leftty、dotty，界面丑陋，并不好用。

还有个叫smyrna的工具，据称使用3D加速，可处理10000个结点的图。但是ubuntu下没有方便的安装方式。

# Android手机MTP连接Ubuntu

`sudo add-apt-repository ppa:webupd8team/unstable`

`sudo apt-get update`

`sudo apt-get install go-mtpfs`

`go-mtpfs /media/MyAndroid`

`fusermount -u /media/MyAndroid`

# 文件校验和

计算文件校验和，一般采用MD5和SHA算法。在Ubuntu中，这些算法的命令包括：md5sum、sha1sum(160-bit) ,sha224sum(224-bit) ,sha256sum(256-bit),sha384sum(384-bit),sha512sum(512-bit)等。

# 产品设计工具

| 类别 | 收费 | 免费 |
|:--:|:--:|:--:|
| Office | MS Office | LibreOffice |
| 流程图 | MS Visio | DIA、Kivio |
| 思维导图 | Mindmanager | FreeMind |
| 快速原型 | Axure RP | pencil |

# 发行版乱战

Linux以发行版众多闻名于世。最近发现了以下网站，或可对各个发行版进行一个简单的比较。

http://distrowatch.com/

下面对几个主要的参数，进行一下点评：

## Office

主要是3个流派：

1.StarOffice->OpenOffice.org->LibreOffice。最初由Sun主导，后来改为Google主导。

2.KOffice->Calligra Office。KDE项目的成果。

3.GOffice。Gnome项目的成果，和前两个相比，GOffice的组件比较独立，没有什么协同能力。

# Firefox插件

http://mozilla.com.cn/addon/76-pagesaver/

这个插件可以将网页保存为图片。

# ASCII表情

╮(╯_╰)╭

(^ω^)

# 远程桌面

Linux下的远程桌面软件主要有RealVNC和rdesktop。前者支持VNC协议，而后者支持MS RDP协议，可连接Windows系统。

## rdesktop

安装方法：

`sudo apt-get install rdesktop`

使用方法：

`rdesktop -u administrator -p ****** -a 16 192.168.1.1`

# sdkman

sdkman(The Software Development Kit Manager), 中文名为:软件开发工具管理器．这个工具的主要用途是用来解决在类unix操作系统(如mac, linux等)中多种版本开发工具的切换, 安装和卸载的工作．对于windows系统的用户可以使用Powershell CLI来体验．

例如: 项目A使用Jdk7中某些特性在后续版本中被移除（尽管这是不好的设计），项目B使用Jdk8,我们在切换开发这两个项目的时候，需要不断的切换系统中的JAVA_PATH,这样很不方便，如果存在很多个类似的版本依赖问题，就会给工作带来很多不必要的麻烦． 
　　 
sdkman这个工具就可以很好的解决这类问题，它的工作原理是自己维护多个版本，当用户需要指定版本时，sdkman会查询自己所管理的多版本软件中对应的版本号，并将它所在的路径设置到系统PATH.

官网：

http://sdkman.io/

教程：

http://blog.csdn.net/heiyouhei123/article/details/51103578

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

http://ebr.springsource.com/repository/app/

本地maven repository通常在~/.m2/repository/下，某些直接下载失败的包，可手动安装到该路径下。

Maven入门教程：

http://www.oracle.com/technetwork/cn/community/java/apache-maven-getting-started-1-406235-zhs.html

Maven的缺点是规则的力量过于强大，对于规则覆盖不了的情况，很难处理。但好在多数项目并没有那么复杂的情况。

针对规则的复杂，Maven提出了模板的概念，用以简化操作。命令是：

`mvn archetype:generate`

可选的模板参见：

https://maven.apache.org/guides/introduction/introduction-to-archetypes.html

代码示例：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/java_build/maven

### 插件和依赖的版本控制

Maven允许同一个软件包的不同版本安装在同一台PC上。比如程序A可以使用spring v3.1，而程序B可以使用spring v3.2。

它的实现方式是：同一个软件包的不同版本，放在不同的路径下。

插件和依赖一般使用version标签定义所使用的版本。版本标签在大多数示例中一般是固定值，这样可以确保部署的一致性。

然而对于一些简单的demo，特别是比较老的demo来说，老旧的版本标签意味着，需要从repo中下载老的库，速度慢且占用空间大。

因此，在开发用的机器上，通常需要将同一类应用的各个插件和依赖配置为同一版本，以节省空间。

插件和依赖的版本号，可用以下网站查询：

https://mvnrepository.com/

此外，还可以用range dependency机制，直接使用最新版本。示例如下：

`<version>[2.40.0,)</version>`

这里的[2.40.0,)表示取2.40.0以上最新版本。注意只有依赖可以使用这种语法，插件是不行的。

### 更换mirror

Maven的官方软件仓库URL为：

http://repo.maven.apache.org/maven2

然而这个网址有的时候会比较慢，这时就需要更换更快的mirror。其方法为：

修改`${user.home}/.m2/settings.xml`。

{% highlight xml %}
<mirrors>
  <mirror>
    <id>UK</id>
    <name>UK Central</name>
    <url>http://uk.maven.org/maven2</url>
    <mirrorOf>central</mirrorOf>
  </mirror>
</mirrors>
{% endhighlight %}

其他mirror有：

http://uk.maven.org/maven2

http://repo.maven.org/maven2/

http://repo1.maven.org/maven2/

http://repo2.maven.org/maven2/

本来国内的OSChina有一个Maven mirror。但现在已经不可用了。

### Eclipse工程转为Maven工程

选择项目，右键点击->Configure->Convert to Maven Project

## uber-jar

uber在德语中，表示above或over。uber-jar实际上就是将程序的依赖也打包，而生成的jar。例如maven中，可用：

`mvn uberjar`

生成uber-jar。

此外，maven shade plugin和dependency-reduced-pom.xml实际上也与uber-jar有关。

## Gradle

XML作为配置文件格式，主要有两个缺点：

1.各种标记占用的空间较大，不简洁。有用的信息淹没在各种标记中，反而降低了配置文件的可读性。

2.XML本身的树状结构，对于条件编译（例如if分支）的情况，没有什么好的语法组织形式。

针对以上问题，Gradle使用Groovy语言作为配置文件格式。它自2012年诞生以来，获得了快速普及，尤其是Google采用Gradle作为Android应用的默认构建工具。

Gradle的设计思想已经脱离了make的范畴。make脚本也好，XML也好，都不是通用程序语言，无论设置再多的规则，也总有满足不了需求的时候。因此，Gradle使用Groovy这种通用程序语言来提供灵活性，同时使用plugin，尤其是官方提供的plugin，来设定规则。就好比是C语言+标准库，这样的组合够强吧。

premake实际上也是类似的设计思路。

代码示例：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/java_build/gradle

官网：

https://gradle.org/

教程：

https://spring.io/guides/gs/gradle/

### Gradle Wrapper

Gradle Wrapper能够让你的工程在没有安装Gradle的机器上编译。

1.使用`gradle wrapper`命令生成gradlew命令。

2.`./gradlew run`

