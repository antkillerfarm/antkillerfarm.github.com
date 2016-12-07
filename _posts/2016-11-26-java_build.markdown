---
layout: post
title:  Java构建工具, Storm, WebService
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

# Storm

Storm是一个大数据领域的实时计算框架。

官网：

http://storm.apache.org/

教程：

http://storm.apache.org/releases/current/Tutorial.html

http://blog.csdn.net/rzhzhz/article/details/8788137

http://ifeve.com/getting-started-with-stom-index/

http://blog.csdn.net/NB_vol_1/article/details/46287077

http://www.open-open.com/lib/view/open1374979211233.html

## 示例

这里使用源代码中自带的示例。

1.下载源代码。

2.`mvn clean install -DskipTests=true`

3.进入examples/storm-starter目录：

`mvn compile exec:java -Dstorm.topology=org.apache.storm.starter.ExclamationTopology`

## Trident

Trident是在storm基础上，一个以realtime计算为目标的高度抽象。它在提供处理大吞吐量数据能力的同时，也提供了低延时分布式查询和有状态流式处理的能力。

教程：

http://storm.apache.org/releases/current/Trident-tutorial.html

http://blog.csdn.net/derekjiang/article/details/9126185

## Storm vs Spark

http://blog.csdn.net/iefreer/article/details/32715153

## 问题汇总

http://blog.sina.com.cn/s/blog_8c243ea30101k0k1.html

## 集群部署

Storm的本地模式，无须hadoop生态圈软件的支持，自己就能运行。但它的集群部署依赖Zookeeper和YARN。

1.配置YARN和Zookeeper，并启动相关服务进程。

2.配置Storm

https://storm.apache.org/releases/current/Setting-up-a-Storm-cluster.html

3.启动Nimbus和Supervisor

Nimbus和Supervisor都是Storm服务进程，前者运行在Master上，而后者运行在Node上。这里的Master和Node，可以和Zookeeper或YARN设置的不同。

Nimbus的作用是将运行的jar分发到各Node去执行。

虽然`bin/storm nimbus`可以启动nimbus，然而这种方法启动的是前台进程，一旦退出终端，进程就会被杀死。可用如下方法解决之：

`start-stop-daemon --start --background --exec /root/apache-storm-1.0.2/bin/storm nimbus`

# WebService

WebService经过近二十年的发展，已经有非常多的框架了。知名的有：Axis1、Axis2、Xfire、CXF、JWS等。

其中的JWS在JDK 1.6之后被集成到JDK中，成为了我学习的首选。

JWS包含了JAX-WS、JAX-RS、JAXB、JAXR、SAAJ、StAX等组件。这里主要涉及的是JAX-WS。

相关的demo参见：

https://github.com/him-bhar/jax-ws

以下仅对demo的代码和遇到的问题做一下说明。

## 代码详解

这个demo包括

## 编译

`mvn package`

由于我使用的JDK是JDK 1.8，因此会遇到如下问题：

`由于 accessExternalSchema属性设置的限制而不允许 'file' 访问, 因此无法读取方案文档'xjc.xsd'。`

解决方法：

{% highlight xml %}
<plugin>
    <groupId>org.jvnet.jax-ws-commons</groupId>
    <artifactId>jaxws-maven-plugin</artifactId>
    <version>2.3</version>
    <configuration>
        <!-- Needed with JAXP 1.5 -->
        <vmArgs>
            <vmArg>-Djavax.xml.accessExternalSchema=all</vmArg>
        </vmArgs>
    </configuration>
</plugin>
{% endhighlight %}

这个问题是由于JDK 1.8以后相关权限变得更加严格所导致的。

参见：

https://my.oschina.net/fuckmylife0/blog/325432

## 参考

http://www.blogjava.net/zjhiphop/archive/2009/04/29/webservice.html

http://blog.csdn.net/lifetragedy/article/details/7205832

