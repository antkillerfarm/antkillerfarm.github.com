---
layout: post
title:  AI Compiler（二）, Git（二）
category: toolchain 
---

* toc
{:toc}

# AI Compiler

## 概述（续）

https://mp.weixin.qq.com/s/jjT0x99ht8xtfWmzL-0R1A

深度学习的IR“之争”

https://mp.weixin.qq.com/s/G36IllLOTXXbc4LagbNH9Q

编译器与IR的思考: LLVM IR，SPIR-V到MLIR

https://www.zhihu.com/question/391811802

如何评价TensorFlow开源的新运行时TFRT？

https://zhuanlan.zhihu.com/p/347599203

TFRT的开源代码分析

https://zhuanlan.zhihu.com/p/600622073

AI编译器之后端优化

## OpenAI Triton

一个类似于TVMscript的可以通过python语法去写高性能GPU程序的库。注意不要和NVIDIA Triton搞混了。后者是一个AI推理框架。

NVIDIA Triton Inference Server（此前称为TensorRT Inference Server）能够帮助开发人员和IT/DevOps轻松地在云端、本地数据中心或边缘部署高性能推理服务器。

官网：

https://github.com/openai/triton

参考：

https://zhuanlan.zhihu.com/p/394377526

OpenAI开源GPU编程语言Triton，将同时支持N卡和A卡

https://zhuanlan.zhihu.com/p/613244988

谈谈对OpenAI Triton的一些理解

## NVFuser

NVFuser是NV专门为Pytorch设计的自动化的GPU代码生成器。

## AKG

AKG是面向华为昇腾AI处理器的张量编译器。

官网：

https://gitee.com/mindspore/akg/

![](/images/img5/akg-design.png)

# Git+

## Monorepo

上面提到的多项目管理，一般称为MultiRepo，即每个项目对应一个单独的仓库。与之相对的则是Monorepo，即把多个项目放在一个仓库里面。

![](/images/img4/Monorepo.png)

参考：

https://juejin.cn/post/6944877410827370504

现代前端工程为什么越来越离不开Monorepo?

https://zhuanlan.zhihu.com/p/77577415

Monorepo是什么，为什么大家都在用？

## Git多项目管理

项目越来越大，一些通用的模块我们希望将他抽离出来作为单独的项目，以便其他项目也可以使用，或者使用一些第三方库，可能我们并不想将代码直接拷贝进我们的项目里面，而仅仅只是单纯的引用。

基于Git有多种方式来解决这个问题：

- Git Submodule。

- Git Subtree。这两个已经集成到git中。

- GitSlave。一个git插件，需要额外安装。

- Google Repo。基于git的python脚本。Android项目的官方工具。

`git submodule update --init --remote --rebase`

单独更新子module AAA：

`git submodule update AAA`

参考：

https://www.jianshu.com/p/284ded3d191b

Git多项目管理

## OpenGrok

OpenGrok是一个阅读源码的Web系统。

官网：

http://oracle.github.io/opengrok/

代码：

https://github.com/oracle/opengrok

参考：

http://mazhuang.org/2016/12/14/rtfsc-with-opengrok/

搭建大型源码阅读环境——使用OpenGrok

## 参考

https://mp.weixin.qq.com/s/z_zFveiiLu9vLvWuBcsaIg

Git从入门到进阶

https://mp.weixin.qq.com/s/0Cv968LpSSYKJpAbW1MlMA

手把手教你git全操作

https://mp.weixin.qq.com/s/DbvRbaH7BJKeTCT4LVXUoA

Git的4个阶段的撤销更改

https://mp.weixin.qq.com/s/nUqvSnnPjYqk2O8u0tQEtQ

Git内部原理揭秘

https://mp.weixin.qq.com/s/nmi1HYkKD8QX0phbvOko2Q

Git居然还有这么高级用法，你一定需要

https://mp.weixin.qq.com/s/PUUL913fig6cFfqy4OKcGA

工作流一目了然，看小姐姐用动图展示10大Git命令

https://blog.csdn.net/mocoe/article/details/84344411

修改git commit的author信息

https://mp.weixin.qq.com/s/9Ey04P5Xv4W95N2FEioZ1g

如何选择Git分支模式？

https://mp.weixin.qq.com/s/GZkGfwrfMTzrMfki8LKbIw

腾讯广告3000+万行大代码库主干开发实战

https://mp.weixin.qq.com/s/KEu6qCl-En6HiKGo2pgEQg

TensorBay：一款易用的像Git一样数据版本管理工具！
