---
layout: post
title:  Java构建工具, ZeroC ICE
category: technology 
---

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

Maven仍旧使用XML作为配置文件格式，但使用内置规则简化脚本的编写。其配置文件一般叫做pom.xml。

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

### 常用maven命令

| 命令 | 含义 |
|:--|:--|
| `mvn compile` | 编译代码 |
| `mvn package` | 打包操作 |
| `mvn exec:java` | 执行程序 |
| `mvn exec:exec` | 执行程序 |
| `mvn clean` | 清理程序 |

使用`mvn exec:java`，还是`mvn exec:exec`，一般根据pom.xml中定义的目标来确定，后者的语法更灵活，功能也更强。

### 插件和依赖模块的版本控制

Maven允许同一个软件包的不同版本安装在同一台PC上。比如程序A可以使用spring v3.1，而程序B可以使用spring v3.2。

它的实现方式是：同一个软件包的不同版本，放在不同的路径下。

插件和依赖模块一般使用version标签定义所使用的版本。版本标签在大多数示例中一般是固定值，这样可以确保部署的一致性。

然而对于一些简单的demo，特别是比较老的demo来说，老旧的版本标签意味着，需要从repo中下载老的库，速度慢且占用空间大。

因此，在开发用的机器上，通常需要将同一类应用的各个插件和依赖模块配置为同一版本，以节省空间。

插件和依赖模块的版本号，可用以下网站查询：

https://mvnrepository.com/

此外，还可以用range dependency机制，直接使用最新版本。示例如下：

`<version>[2.40.0,)</version>`

这里的[2.40.0,)表示取2.40.0以上最新版本。注意只有依赖模块可以使用这种语法，插件是不行的。

### 更换mirror

Maven的官方软件仓库URL为：

http://repo.maven.apache.org/maven2

然而这个网址有的时候会比较慢，这时就需要更换更快的mirror。其方法为：

修改`${user.home}/.m2/settings.xml`。

>注意这个配置文件只对该repo生效，如果想全局有效的话，可修改/etc/maven/settings.xml。

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

http://maven.aliyun.com/nexus/content/groups/public

阿里云的mirror大概是2016年7、8月份推出的，速度非常快。

本来国内的OSChina有一个Maven mirror。但现在已经不可用了。

http://jcenter.bintray.com

jcenter是JFrog的产品，后者专注于maven部署，因此该网站拥有的jar最全且新，速度也不慢。

### Eclipse工程转为Maven工程

选择项目，右键点击->Configure->Convert to Maven Project

### Maven引用本地jar

大多数情况下，Maven并不需要处理jar的引用问题，项目所需的jar以及相应的依赖，会自动从中心软件仓库中下载至本地。

那么对于中心软件仓库中没有的jar，Maven是怎么处理的呢？

这里主要有3种方法：

1.将jar包安装到本地repository中。

`mvn install:install-file -Dfile=my-jar.jar -DgroupId=org.richard -DartifactId=my-jar -Dversion=1.0 -Dpackaging=jar`

这种方法的好处是安装之后的操作，和外部jar相同。但缺点是：部署的时候，每台机器都要安装一次。

2.使用system scope。

{% highlight xml %}
  <dependencies>
    <dependency>
      <groupId>org.richard</groupId>
      <artifactId>my-jar</artifactId>
      <version>1.0</version>
      <scope>system</scope>
      <systemPath>${project.basedir}/lib/my-jar.jar</systemPath>
    </dependency>
  </dependencies>
{% endhighlight %}

这种方法下，jar可以和代码放在同一个文件夹下，以便分发。然而这种做法不是官方推荐的做法，在执行时，需要自己处理classpath的问题。

这里吐槽一下Java。Java的classpath选项中，如果指定的jar不止一个，那么两个jar之间需用符号分隔。这个分隔符在Windows上是`;`，而在Linux上是`:`。居然连这一点都没统一，实在无语。

### uber-jar

uber在德语中，表示above或over。uber-jar实际上就是将程序的依赖也打包，而生成的jar。例如maven中，可用：

`mvn uberjar`

生成uber-jar。

此外，maven shade plugin和dependency-reduced-pom.xml实际上也与uber-jar有关。

### 搭建Maven私服

Artifactory可用于搭建团队内部的Maven私服。官网：

https://www.jfrog.com/open-source/

正常运行项目的maven命令，向artifactory索求jar。如果artifactory已经下载了就会直接返回给你，还没有的就会去那几个repo官方站下载。

参考：

http://blog.csdn.net/calvinxiu/article/details/1713323

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

类似的，maven也有一个Wrapper，有些工程里的mvnw或mvnw.cmd，就是这个Wrapper的文件。

# ZeroC ICE

我在研究生时代，研究过CORBA、EJB、COM这样的中间件技术。然而工作以后，再没有机会使用。平时偶尔关注，也只是晓得CORBA从来没有流行过，EJB在Struts、Hibernate、Spring等框架的围攻下，用者寥寥。直到最近，因为项目需要接触到ZeroC的ICE框架。

ICE框架的官网地址：

https://zeroc.com/downloads/ice

安装：

{% highlight bash %}
wget https://zeroc.com/download/GPG-KEY-zeroc-release
sudo apt-key add GPG-KEY-zeroc-release
sudo apt-add-repository "deb http://zeroc.com/download/apt/ubuntu16.04 stable main"
sudo apt-get update
sudo apt-get install zeroc-ice-all-runtime zeroc-ice-all-dev
sudo apt-get install libssl-dev
pip install zeroc-ice
{% endhighlight %}

多语言demo：

https://github.com/zeroc-ice/ice-demos

注意demo的master分支是开发分支，好多代码都是有问题的，请切换到正在使用版本的分支，例如：

`git checkout 3.6`

# Hprose

Hprose是ZeroC ICE的一个竞争者，由国内某高手打造，支持的语言超过20种，堪称最全。不过貌似没什么大公司用啊。。。

其标榜的无需生成桩代码的优点，相比ZeroC ICE的老版本来说，的确是个进步。但目前的ZeroC ICE 3.6版本，也同样提供了类似的功能。这或者也是Hprose一直火不起来的原因。

官网：

http://hprose.com/

Github：

https://github.com/hprose

# DL参考资源+

https://mp.weixin.qq.com/s/9yfTO2jHz69-k1MsUGIM0Q

双目立体放大！谷歌刚刚开源的这篇论文可能会成为手机双摄的新玩法

https://mp.weixin.qq.com/s/ugK5U6A2tqYkJwWfNWPuKg

计算机视觉深度学习中的几何结构与不确定性

https://mp.weixin.qq.com/s/XESTrLMERQPR9eXcFA56gg

神经符号系统：让机器善解人意

https://mp.weixin.qq.com/s/vTtKUagiFD55mcQIeGDxQw

图像鉴黄做得好，健康上网少烦恼

https://mp.weixin.qq.com/s/l6fNAWeufws4RijA0nZWpA

模型的泛化能力仅和Hessian谱有关吗？

https://mp.weixin.qq.com/s/j_nU7QB_nArHQj36PL3h9w

李宏扬：二阶信息在图像分类中的应用

https://mp.weixin.qq.com/s/hE-2GLo1SNdIMY858_HuaQ

化秋毫为波澜:运动放大算法

https://mp.weixin.qq.com/s/y0SAI0f3w42z-nnoorFoEQ

共享空间学习共有特征的异源图像匹配

https://mp.weixin.qq.com/s/GYe1psxy1KMCbV7f3K8f4Q

神经规则引擎：让符号规则学会变通

https://mp.weixin.qq.com/s/M7w7tMVd23YToIW7DypYjA

用DL实现Bug自动归类：微软研究院提出DBRNN-A

https://mp.weixin.qq.com/s/KX85CCpYrXFOvdTU5Q4Frg

阿里巴巴论文提出针对影视作品的语音情感识别信息融合框架
