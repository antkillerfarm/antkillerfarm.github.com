---
layout: post
title:  孩子的编程语言, Go, Julia, Rust, 地理
category: language 
---

# 孩子的编程语言

## 概述

在孩子中普及编程教育已经是一个趋势了。

参见：

http://www.kidscode.cn/

中国少儿编程网

## Scratch

Scratch是一款由麻省理工学院(MIT) 设计开发的一款面向少年的简易编程工具。适合8～12岁的儿童。

官网：

https://scratch.mit.edu/

安装：

`sudo apt install scratch`

参考：

https://mp.weixin.qq.com/s/HzbegP_NIowUwRoeDne5Vw

适合小朋友的Scratch动手项目！AI在生活中的19个应用

## Squeak

Squeak是一个是一个现代的，开源的，功能齐全的Smalltalk程序设计语言和执行环境。适合9岁以上的儿童。

官网：

http://squeak.org/

## Logo语言的安装

Logo语言是我学习计算机时的启蒙语言。那是初一、初二的时候，当时的电脑还是运行Dos的386系统，我们在原始的命令行下，津津有味的编着代码。那时候电脑可是稀罕物，为了节约上机时间，程序都是事先写到小本本上的，我至今还保留着当时的作业本。

话归正传，说说Linux下Logo语言的安装。

1）到Berkeley的官网下载Logo语言的源代码

http://www.cs.berkeley.edu/~bh/logo.html

2）按照源代码包里面readme的提示，编译之。提示缺少libbsd和libtermcap。

3）`sudo apt-get install libbsd-dev libncurses5-dev`

重新编译即可。

再一次看到熟悉的命令行界面和海龟了:)

## NetLogo

NetLogo是继承了Logo语言的一款编程开发平台,但它又改进了Logo语言只能控制单一个体的不足,它可以在建模中控制成千上万的个体,因此,NetLogo建模能很好地模拟微观个体的行为和宏观模式的涌现及其两者之间的联系。

官网：

http://ccl.northwestern.edu/netlogo/

## Kodu

Kodu是微软推出的面向儿童的可视化编程语言，可以让孩子自己创造PC或XBox游戏。

官网：

https://www.kodugamelab.com/

## turtle

turtle是python对于LOGO语言图形库的实现。大概是出于照顾初学者，尤其是孩子的考虑，turtle库无须安装，已经内置到python的标准库中了。

文档：

https://docs.python.org/3/library/turtle.html

参考：

https://mp.weixin.qq.com/s/HaxaJHKZnctyGJZnvloCKA

What?!Python一行代码，能玩这么多童年的游戏？

## 其他

https://mp.weixin.qq.com/s/WJcLbYwH3-cTRHkMYNjJZA

Steam高赞游戏入门机器学习！不写代码，人人可玩，又能吸猫，汉化版已推出

https://mp.weixin.qq.com/s/gRumlJI8iUj_0HART2WD8Q

不用写代码，谷歌教你如何用2个小时做出只属于你的游戏（Area 120 Game Builder）

https://mp.weixin.qq.com/s/7SfFeKUwEixcmChe3lC-lg

有了这15款编程游戏，谁都可以学编程！

https://www.codewar.cn/

CodeCombat是一个让学生通过玩游戏学习计算机科学的平台

https://mp.weixin.qq.com/s/TLnUMECwumg-xA48b1bOIg

这三款超好评编程游戏，好玩到停不下来

# Go

官网：

https://golang.org

教程：

https://www.runoob.com/go/go-tutorial.html

安装：

`sudo apt install golang`

运行：

`go run xxx.go`

- Web 框架：Gin、Beego、Echo等。
- 微服务框架：go-kit、go-micro等。
- 数据库连接库：go-sql-driver/mysql、go-redis/redis、mongo-go-driver、go-elasticsearch，以及gorm、xorm等。
- 中间件软件：etcd、Consul、NSQ、Caddy等。
- 数据库软件：TiDB、Cockroach、InfluxDB、Cayley等。
- 数据爬取软件：Pholcus、Colly等。

参考：

https://mp.weixin.qq.com/s/ka5woeuvNxX3Y0Y4UMlruw

Go上下文取消操作

https://mp.weixin.qq.com/s/5MNvW9czxaRWso8jbBRyBw

Go调度程序：Ms，Ps&Gs

https://mp.weixin.qq.com/s/mUhPHycvLSYYkBQSfIgIdA

Channels In Go

https://mp.weixin.qq.com/s/neBQ4Etx3RLhMQdM6GksVg

Go语言HTTP/2探险之旅

https://mp.weixin.qq.com/s/_d_-qg-mY3QLSxfACsNBJw

Golang之不可重入函数实现

https://mp.weixin.qq.com/s/3jmMexGYY4ww_urSZ36vVQ

图解Go内存分配器

https://mp.weixin.qq.com/s/qaqN4Eqndjg95TPBOC4d_g

HTTP/2 in GO

https://mp.weixin.qq.com/s/XbtSamp7I6HwvRO_OweqJg

Go实现ORM及构建查询

https://mp.weixin.qq.com/s/DxE3YOE1GDq6ZXRhpzfC0w

使用Go语言徒手撸一个简单的负载均衡器

https://mp.weixin.qq.com/s/SWfPV6tUC5olZgIdVabd3A

从入门到掉坑：Go内存池/对象池技术介绍

## GTK

熟悉我的朋友都知道，新语言的GTK demo是一定要有的。毕竟helloworld太简单了，容易让人产生从三到万的错觉。

go的GTK绑定主要有两个项目：

For GTK3:

https://github.com/gotk3/gotk3

For GTK2:

https://github.com/mattn/go-gtk

需要注意的是，第一个项目由于没有使用GObject Introspection，所以只支持了主要功能。项目作者也介绍说，当初发起项目的时候，并不知道GObject Introspection这样的神器，也许以后的某个时间会迁移过去。至于GTK2就不用说了，GI是只有GTK3才有的福利。

参考：

https://www.jianshu.com/p/be197980d4bb

2019，Go GUI项目爆发的一年？

# Julia

Julia是新晋发布1.0版本的科学计算类语言，号称兼具C++、python、matlab的优点。

官网：

https://julialang.org/

代码：

https://github.com/JuliaLang/julia

## 安装

Julia目前暂时没有apt安装的办法，需要源代码编译，后者安装预编译的版本。这里讲讲源码编译的过程。

1.安装依赖

`sudo apt install build-essential libatomic1 python gfortran perl wget m4 cmake pkg-config`

2.源码编译

`make -j 4`

3.安装

`echo "alias julia='/path/to/install/folder/bin/julia'" >> ~/.bashrc && source ~/.bashrc`

## GTK

1.安装

Julia使用pkg模式进行安装。在Julia命令行下，输入`]`即可进入pkg模式。

`(v1.0) pkg> add Gtk`

输入Ctrl+C可退出pkg模式。

## 参考

https://mp.weixin.qq.com/s/dvVQ9H14eyVjyD4yhadRnQ

MIT正式发布编程语言Julia 1.0：Python、R、C++三合一

https://mp.weixin.qq.com/s/X_MDcEmmKDN_RHYTx3kjhw

Julia 1.0正式发布，这是新出炉的一份简单中文教程

https://mp.weixin.qq.com/s/zZbK5VPlr43CleExwDoBxw

如何在Julia编程中实现GPU加速

https://mp.weixin.qq.com/s/G0u_mP7xBJ7mx4eerdiY_g

为什么Julia比Python快？

# Rust

官网：

https://www.rust-lang.org/

教程：

http://wiki.jikexueyuan.com/project/rust-primer/

Rust的包管理器叫做cargo。它的官网：

https://crates.io

## 安装

`curl https://sh.rustup.rs -sSf | sh`

或者

`sudo apt install rustc cargo`

Rust的更新比较快，3个月就会发布一个新版本，因此推荐使用前者。

更新：

`rustup update`

参考：

https://blog.csdn.net/xiangxianghehe/article/details/53471936

如何利用科大源提速Cargo和Rust

## Cargo

Rust的编译工具叫做`rustc`，然而正如我们很少使用`gcc`，而更多使用`make`一样，我们更多使用Cargo来编译工程。

文档：

https://doc.rust-lang.org/cargo/guide/

编译：

`cargo build`

运行：

`cargo run`

Cargo可以像maven一样自动下载依赖，也可以手动安装：`cargo install`。

## GTK示例

Rust的gtk支持：

https://gtk-rs.org/

代码：

https://github.com/gtk-rs/gtk

示例：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/helloworld/rust/hello-rust

## 编写OS

教程：

https://os.phil-opp.com/

Writing an OS in Rust

https://web.stanford.edu/class/cs140e/

CS140E: Operating Systems Design and Implementation

项目：

https://github.com/rcore-os/rCore

参考：

https://zhuanlan.zhihu.com/c_1078248076300521472

一个Rust OS的专栏

## 参考

https://mp.weixin.qq.com/s/JBlzMIhMa7TB5tHGSRhVkQ

​半小时入门Rust，这是一篇Rust代码风暴

https://mp.weixin.qq.com/s/xGBAGBGsxBDuKkSxXOZRjQ

在Rust代码中编写Python是种怎样的体验？
