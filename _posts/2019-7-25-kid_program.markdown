---
layout: post
title:  孩子的编程语言, Go
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

## 其他

https://mp.weixin.qq.com/s/WJcLbYwH3-cTRHkMYNjJZA

Steam高赞游戏入门机器学习！不写代码，人人可玩，又能吸猫，汉化版已推出

https://mp.weixin.qq.com/s/gRumlJI8iUj_0HART2WD8Q

不用写代码，谷歌教你如何用2个小时做出只属于你的游戏（Area 120 Game Builder）

https://mp.weixin.qq.com/s/7SfFeKUwEixcmChe3lC-lg

有了这15款编程游戏，谁都可以学编程！

# Go

官网：

https://golang.org

教程：

https://www.runoob.com/go/go-tutorial.html

安装：

`sudo apt install golang`

运行：

`go run xxx.go`

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
