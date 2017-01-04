---
layout: post
title:  linux学习心得（二）
category: technology 
---

## 查看内存使用情况

### top命令

top命令可在进程这一级查看内存、运行时间、CPU等的使用情况。并可根据不同属性对结果排序：

P：按%CPU使用率排序

T：按TIME+排序

M：按%MEM排序

注：运行top命令之后，输入相应字符即可切换排序。

### free命令

free命令的内容比较概括，主要包含系统内存的整体使用情况，不深入到进程一级。

## 时间的表示方法

一般遵循ISO 8601标准：

https://www.w3.org/TR/NOTE-datetime

YYYY-MM-DDThh:mm:ss.sTZD (eg 1997-07-16T19:20:30.45+01:00)

其中的TZD表示time zone designator。

## wifi配置

Linux下的wifi配置主要使用iw系列命令，包括iw、iwconfig、iwlist、iwpriv。参见：

http://blog.csdn.net/liangyamin/article/details/7209761

## Inotify

一种高效、实时的Linux文件系统事件监控框架。参考文档：

http://www.infoq.com/cn/articles/inotify-linux-file-system-event-monitoring

## /usr

usr很多人都认为是user缩写，其实不然，这是unix system resource的缩写。

## lsof命令

lsof(list open files)是一个列出当前系统打开文件的工具。在linux环境下，任何事物都以文件的形式存在，通过文件不仅仅可以访问常规数据，还可以访问网络连接和硬件。所以如传输控制协议 (TCP) 和用户数据报协议 (UDP) 套接字等，系统在后台都为该应用程序分配了一个文件描述符，无论这个文件的本质如何，该文件描述符为应用程序与基础操作系统之间的交互提供了通用接口。因为应用程序打开文件的描述符列表提供了大量关于这个应用程序本身的信息，因此通过lsof工具能够查看这个列表对系统监测以及排错将是很有帮助的。

## nc命令

NetCat，在网络工具中有“瑞士军刀”美誉。它短小精悍（1.84版本也不过25k，旧版本或缩减版甚至更小）、功能实用，被设计为一个简单、可靠的网络工具，可通过TCP或UDP协议传输读写数据。同时，它还是一个网络应用Debug分析器，因为它可以根据需要创建各种不同类型的网络连接。

http://blog.csdn.net/wang7dao/article/details/7684998

## 常用命令示例

`find . -name *.txt`

查找文件夹（包括子文件夹）中所有的txt文件。

`strings /lib/tls/libc.so.6 | grep GLIBC`

strings能输出文件中的可打印字符串（可指定字符串的最小长度），通常用来查看非文本文件（如二进制可执行文件）中的可读内容。上例可查看glibc支持的版本。

# 频率统计学派 vs. 贝叶斯学派

对数学史感兴趣的朋友，可以看看陈希孺院士的《数理统计学简史》一书。rickjin文章的内容有相当部分取自该书。

>注：陈希孺，1934～2005，数理统计学家。1956年毕业于武汉大学数学系，1997年当选为中国科学院院士。

该书中关于频率统计学派和贝叶斯学派的争议，引起了我的注意。

频率统计学派是所谓的正统派，由于其简单且便于理解的特点，多数入门级的数理统计学教程，一般都是按照该学派的思路写的。

而贝叶斯学派可谓另辟蹊径，它和频率统计学派的差异，参见《机器学习（七）》。由于该派系的思想比较新颖，我一度以为它和频率统计学派的关系，就犹如相对论之于经典力学。

然而，陈希孺院士告诉我们，两者各有优劣，尚未到一方决出胜负的阶段。比如，贝叶斯学派的先验估计，既是其成功的奥秘，也是其不成功的软肋。比如，对于“无信息先验分布”，目前尚处于“信则灵，不信则无”的境地。

陈院士的观点是：**各取所长，为我所用**。

# 随机变量序列的收敛性

弱收敛：$$F_n(x)\xrightarrow{W}F(x)$$

依分布收敛：$$X_n\xrightarrow{L}X$$

依概率收敛：$$X_n\xrightarrow{P}X$$

r阶收敛：$$X_n\xrightarrow{r}X$$

几乎处处收敛（almost everywhere convergent）：$$X_n\xrightarrow{a.e.}X$$ or $$X_n\xrightarrow{a.s.}X$$

一致收敛（uniform convergence）：$$X_n\xrightarrow{u.c.}X$$

以上概念实际上都是测度论的内容。具体到这里，弱收敛针对分布函数F，而其他收敛针对随机变量X。

收敛严格性：

$$X_n\xrightarrow{P}X>X_n\xrightarrow{L}X$$

$$X_n\xrightarrow{r}X>X_n\xrightarrow{P}X$$

$$X_n\xrightarrow{a.s.}X>X_n\xrightarrow{P}X$$

大数定理：

依概率收敛->弱大数定理

几乎处处收敛->强大数定理

# 统计力学与组合优化

MCMC和Gibbs Sampling最早都是统计力学的概念，后来才被用于机器学习领域。现将统计力学与组合优化的对应关系罗列如下：

| 统计力学 | 组合优化 |
|:--:|:--:|
| 样本 | 问题实例 |
| 状态（构形） | 构形 |
| 能量 | 代价函数 |
| 温度 | 控制参数 |
| 基态能量 | 最小代价 |
| 基态构形 | 最小构形 |

# 概率图模型

probabilistic graphical model（PGM）最早由Judea Pearl发明。

这方面比较重要的文章和书籍有：

http://www.cis.upenn.edu/~mkearns/papers/barbados/jordan-tut.pdf

Michael Irwin Jordan著。

《Probabilistic Graphical Models: Principles and Techniques》，Daphne Koller，Nir Friedman著（2009年）。

>注：Judea Pearl，1936年生，以色列-美国计算机科学家，UCLA教授。2011年获得图灵奖。

>Michael Irwin Jordan，1956年生，美国计算机科学家。UCSD博士，先后执教于MIT和UCB。吴恩达的导师。

>Daphne Koller，女，1968年生，以色列-美国计算机科学家。斯坦福大学博士及教授。和吴恩达共同创立在线教育平台Coursera。

>Nir Friedman，1967年生，以色列计算机科学家。斯坦福大学博士，耶路撒冷希伯来大学教授。

http://www.cs.cmu.edu/~epxing/Class/10708-14/lectures/

CMU的邢波（Eric Xing）所开的概率图模型课程。

# BugBrain

BugBrain是一个神经网络相关的益智游戏。官网：

http://www.biologic.com.au/bugbrain/

# 协方差和相关系数

$$Cov(X,Y)=E\{[X-E(X)][Y-E(Y)]\}$$

$$\rho_{XY}=\frac{Cov(X,Y)}{\sqrt{D(X)}\sqrt{D(Y)}}$$


