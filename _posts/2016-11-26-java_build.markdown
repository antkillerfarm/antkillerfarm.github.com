---
layout: post
title:  Java构建工具, Jam
category: toolchain 
---

* toc
{:toc}

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

```xml
<mirrors>
  <mirror>
    <id>UK</id>
    <name>UK Central</name>
    <url>http://uk.maven.org/maven2</url>
    <mirrorOf>central</mirrorOf>
  </mirror>
</mirrors>
```

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

```xml
  <dependencies>
    <dependency>
      <groupId>org.richard</groupId>
      <artifactId>my-jar</artifactId>
      <version>1.0</version>
      <scope>system</scope>
      <systemPath>${project.basedir}/lib/my-jar.jar</systemPath>
    </dependency>
  </dependencies>
```

这种方法下，jar可以和代码放在同一个文件夹下，以便分发。然而这种做法不是官方推荐的做法，在执行时，需要自己处理classpath的问题。

这里吐槽一下Java。Java的classpath选项中，如果指定的jar不止一个，那么两个jar之间需用符号分隔。这个分隔符在Windows上是`;`，而在Linux上是`:`。居然连这一点都没统一，实在无语。

### uber-jar

uber在德语中，表示above或over。uber-jar实际上就是将程序的依赖也打包，而生成的jar。例如maven中，可用：

`mvn uberjar`

生成uber-jar。

此外，maven shade plugin和dependency-reduced-pom.xml实际上也与uber-jar有关。

### 搭建Maven私服

#### Artifactory

Artifactory可用于搭建团队内部的Maven私服。其企业版还支持Pypi、npm等多种仓库。

官网：

https://www.jfrog.com/open-source/

正常运行项目的maven命令，向artifactory索求jar。如果artifactory已经下载了就会直接返回给你，还没有的就会去那几个repo官方站下载。

参考：

http://blog.csdn.net/calvinxiu/article/details/1713323

真正的maven私服搭建器--Artifactory

#### Nexus Repository Manager

这是另一个Maven私服，也支持多种仓库。

官网：

https://www.sonatype.com

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

安装：

`sdk install gradle 5.6.3`

教程：

https://spring.io/guides/gs/gradle/

参考：

https://blog.csdn.net/lj402159806/article/details/78422953

gradle配置国内镜像

### Gradle Wrapper

Gradle Wrapper能够让你的工程在没有安装Gradle的机器上编译。

1.使用`gradle wrapper`命令生成gradlew命令。

2.`./gradlew run`

类似的，maven也有一个Wrapper，有些工程里的mvnw或mvnw.cmd，就是这个Wrapper的文件。

# Jam

Jam是Perforce推出的构建工具。

官网：

https://www.perforce.com/documentation/jam-documentation

由于Perforce已经不再维护Jam项目，所以目前主要使用由FreeType维护的FT Jam。

官网：

https://www.freetype.org/jam/

此外，还有Boost.Build（也就是原来的Boost.Jam）。

官网：

https://boostorg.github.io/build/

其命令行工具之前叫bjam，现在叫b2。

原始的Jam对C++不太友好，所以如果是C++的项目，推荐使用b2。

b2的使用示例：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/cpp/jam

- 每个项目的根目录必须有一个Jamroot.jam文件。

- 其他源代码目录下有一个Jamfile.jam文件。

参考：

https://blog.csdn.net/jadedrip/article/details/1722318

bjam初接触

# Build tools+

## blade

blade是腾讯出品的构建工具。

官网：

https://github.com/chen3feng/blade-build

## MSbuild

MSbuild当然是微软的构建工具了。

官网：

https://msdn.microsoft.com/en-us/library/dd393574.aspx

参考：

http://www.cnblogs.com/linianhui/archive/2012/08/30/2662648.html

MSBuild入门

## OkBuck

OkBuck是Uber推出的构建工具。它的底层使用Buck构建系统。

官网：

https://github.com/uber/okbuck

## Buck2

Buck2是Buck的升级版，两者都是Meta开发的。Buck2目前支持C++, Python, Rust, Erlang, OCaml等语言的项目构建。

官网：

https://buck2.build

## WAF

WAF是一个python写的C/C++构建工具。

官网：

https://waf.io

## vcpkg

这是MS提供的一个C/C++包管理工具，一般配合CMake使用。支持平台包括Windows/Linux/MacOS。

官网：

https://github.com/Microsoft/vcpkg

## pyproject.toml

pyproject.toml是python官方提出的构建方案。

一般使用build工具来构建基于pyproject.toml的项目：

https://build.pypa.io/en/stable/

参考：

https://zhuanlan.zhihu.com/p/666166082

Python新规范pyproject.toml完全解析
