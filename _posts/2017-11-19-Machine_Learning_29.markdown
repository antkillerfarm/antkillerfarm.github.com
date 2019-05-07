---
layout: post
title:  机器学习（二十九）——在线学习
category: ML 
---

# 在线学习

我们之前讨论的算法，都是给定一个训练集S，经训练之后，得到预测函数h，然后再在新的样本集上进行预测。这种方法被称为批量学习（batch learning）。

与之相对的，还有一种边学习边预测的在线学习（online learning）算法。其步骤如下：

>1.$$i:=0$$。   
>2.输入$$x^{(i)}$$，算法预测$$y^{(i)}$$。   
>3.根据$$y^{(i)}$$的真实值，修正算法模型。这一步也被称作更新过程（update procedure）   
>4.令$$i:=i+1$$，以处理下一个数据样本。

在线学习的优点：
1.算法在学习过程中，即可预测。
2.随着数据样本的增多，预测会更加准确，即具有自我完善的的能力。

下面以感知器（perceptron）算法为例，讨论一下在线学习的误差问题。

首先回忆一下感知器算法：

$$h_\theta(x)=g(\theta^Tx),y\in\{1,-1\}$$

$$g(z)=\begin{cases}
1, & z\ge 0 \\
-1, & z<0 \\
\end{cases}$$

对于训练样本$$(x,y)$$，其更新过程为：

$$\theta:=\begin{cases}
\theta, & h_\theta(x)=y \\
\theta+yx, & otherwise \\
\end{cases}\tag{3}$$

这里给出一个和感知器算法有关的定理：

对于给定的m个样本**序列**（注意“序列”二字，在线算法对于样本的顺序是敏感的）$$x^{(i)}$$，如果所有的$$\|x^{(i)}\|\le D$$，并且存在单位向量u，使得所有样本的$$y^{(i)}(u^Tx^{(i)})\ge \gamma$$，则感知器算法在该序列上的预测错误个数最多为$$(D/\gamma)^2$$。

这个定理是Henry David Block和Albert B. J. Novikoff于1962年提出的。

>注：Henry David Block，1920～1978，美国数学家。   
>这个人的经历有点非典型。20岁本科毕业，由于专业是文学和心理学，结果找不到工作。只好回炉，又读了个土木工程的本科。   
>二战期间在Goodyear的飞机工厂（没错，就是那个卖轮胎的Goodyear）担任测试工程师。在那里碰到一个当医生的英国妹子，搞定之。   
>1946年，他老婆在Iowa State University获得了一个职位，于是他也跟着搬了过去。估计是无所事事，他经常到大学里蹭课，然后就发现自己对数学很感兴趣。   
>于是继续读书，1949年拿到博士学位。经过几年助教生涯之后，最终成为康奈尔大学应用数学教授。   
>话说，根据缩写找人真是太痛苦了，很多资料都不是你要找的人，Block又是个大路货。我最后是在他的一位同事的论文中找到他的全名的。那篇论文发表于1985年，距离他去世已经7年，但他仍是作者之一，可见人缘不错。

>Albert B. J. Novikoff，全名不详，只知道是纽约大学教授。从岁数来看应该已经退休了。

下面给出这个定理的证明过程：

由公式3可知，$$\theta$$的值只有在发生预测错误的时候才会改变，因此我们可以使用$$\theta^{(k)}$$表示第k个错误。同时令$$\theta^{{1}}=0$$。

根据更新公式可得：

$$(\theta^{(k+1)})^Tu=(\theta^{(k)})^Tu+y^{(i)}(x^{(i)})^Tu\ge (\theta^{(k)})^Tu+\gamma$$

所以：

$$(\theta^{(2)})^Tu\ge (\theta^{(1)})^Tu+\gamma=\gamma$$

$$(\theta^{(3)})^Tu\ge (\theta^{(2)})^Tu+\gamma\ge 2\gamma$$

由数学归纳法可得：

$$(\theta^{(k+1)})^Tu\ge k\gamma\tag{4}$$

另外，

$$\|\theta^{(k+1)}\|^2=\|\theta^{(k)}+y^{(i)}x^{(i)}\|^2=\|\theta^{(k)}\|^2+\|y^{(i)}x^{(i)}\|^2+2y^{(i)}(x^{(i)})^T\theta^{(k)}$$

由感知器算法的定义可得:

$$y^{(i)}(x^{(i)})^T\theta^{(k)}\le 0$$

所以：

$$\|\theta^{(k+1)}\|^2\le \|\theta^{(k)}\|^2+\|x^{(i)}\|^2\le \|\theta^{(k)}\|^2+D^2$$

同样，由数学归纳法可得：

$$\|\theta^{(k+1)}\|^2\le kD^2\tag{5}$$

因为：

$$(\theta^{(k+1)})^Tu=\|\theta^{(k+1)}\|\cdot\|u\|\cdot \cos\phi\le \|\theta^{(k+1)}\|\cdot\|u\|=\|\theta^{(k+1)}\|\tag{6}$$

由公式4、5、6可得：

$$\sqrt{k}D\ge \|\theta^{(k+1)}\|\ge (\theta^{(k+1)})^Tu\ge k\gamma$$

所以：

$$k\le (D/\gamma)^2$$

# 贝叶斯学习

1787年5月，美国各州（当时为13个）代表在费城召开制宪会议；1787年9月，美国的宪法草案被分发到各州进行讨论。一批反对派以“反联邦主义者”为笔名，发表了大量文章对该草案提出批评。宪法起草人之一亚历山大·汉密尔顿着急了，他找到曾任外交国务秘书（即后来的国务卿）的约翰·杰伊，以及纽约市国会议员麦迪逊，一同以普布利乌斯（Publius）的笔名发表文章，向公众解释为什么美国需要一部宪法。他们走笔如飞，通常在一周之内就会发表3-4篇新的评论。1788年，他们所写的85篇文章结集出版，这就是美国历史上著名的《联邦党人文集》。

《联邦党人文集》出版的时候，汉密尔顿坚持匿名发表，于是，这些文章到底出自谁人之手，成了一桩公案。1810年，汉密尔顿接受了一个政敌的决斗挑战，但出于基督徒的宗教信仰，他决意不向对方开枪。在决斗之前数日，汉密尔顿自知时日不多，他列出了一份《联邦党人文集》的作者名单。1818年，麦迪逊又提出了另一份作者名单。这两份名单并不一致。在85篇文章中，有73篇文章的作者身份较为明确，其余12篇存在争议。

像这样一个问题，在没有机器学习的时代，可以耗费一个考据学家10年20年也不一定能有结果。但是用机器学习一个叫朴素贝叶斯的方法，就可以解开。

https://mp.weixin.qq.com/s/szTmHY-Yvn7N3s_GzTDiEA

解开贝叶斯黑暗魔法：通俗理解贝叶斯线性回归

https://mp.weixin.qq.com/s/1JSxjkKEUlWOzXCQPTve3A

贝叶斯线性回归简介

https://mp.weixin.qq.com/s/NTK-u4aVrTTmvi-4ZBa8RQ

数十亿用户的Facebook如何进行贝叶斯系统调优？

https://mp.weixin.qq.com/s/g24mcZjQ25sQJx9mqE_XSA

怎样判断漂亮女孩是不是单身的？美国海军在汪洋大海里搜索丢失的氢弹、失踪的核潜艇都用过这种方法。

https://mp.weixin.qq.com/s/bjyO4AS1Sjo09qNMpqf6JA

最新36页《贝叶斯非参学习综述》，机器学习内功修炼手册

https://mp.weixin.qq.com/s/x3AREJcDKvjo7vl6TS79OA

贝叶斯推理实用入门

https://mp.weixin.qq.com/s/Tk2t3R_SUU6S9yvdjnwzDQ

量化交易中的贝叶斯优化问题

https://mp.weixin.qq.com/s/1JH3hDy0FXUjGB4QgTd31g

不看任何数学公式来讲解贝叶斯算法

https://zhuanlan.zhihu.com/p/59196946

如何用贝叶斯高斯张量分解修复缺失数据？

https://zhuanlan.zhihu.com/p/63351454

如何用贝叶斯概率矩阵分解修复缺失数据？

# TensorFlow

https://mp.weixin.qq.com/s/EytvywrsgydXAJQhuUqKvg

简易浣熊识别器是如何实现的

http://www.jianshu.com/p/d443aab9bcb1

在TensorFlow上使用LSTM进行情感分析

https://mp.weixin.qq.com/s/gW_KX6eF9XEsSUO1UzJ3WQ

基于LSTM的情感分析

https://mp.weixin.qq.com/s/KZhL477ApHgQfmM2xFrYJw

Tensorlang：基于TensorFlow的可微编程语言

https://mp.weixin.qq.com/s/_9NJ6QLQArUAD1DKb0KRfA

如何使用TensorFlow mobile部署模型到移动设备

https://mp.weixin.qq.com/s/e_TzQxFLAonLMyYAhte6Cg

face-api.js：在浏览器中进行人脸识别的JavaScript接口

https://mp.weixin.qq.com/s/23FoaaA3Z_3kf03BmepFPg

如何将模型部署到安卓移动端，这里有一份简单教程

https://mp.weixin.qq.com/s/MT1YaMm4KBWsIZeHehahgw

TensorFlow重大升级：自动将Python代码转为TF Graph，大幅简化动态图处理！

https://mp.weixin.qq.com/s/zeZs48XbYJGhvOoIysZ8QA

Docker Compose+GPU+TensorFlow所产生的奇妙火花

https://mp.weixin.qq.com/s/sOggiB57D-ekWOsbL6TY_A

TensorFlow中那些鲜为人知却又极其实用的知识

https://mp.weixin.qq.com/s/XHKrkNf2bWLF7r7gzdU24w

Quick, Draw!涂鸦分类递归神经网络

https://mp.weixin.qq.com/s/QQednlKYl6t3aO0xCzgGmA

带你轻松使用TensorFlow创建大型线性模型

https://mp.weixin.qq.com/s/BfwTOtLnwnMsS3-PQQBHSg

使用TensorFlow训练WDL模型性能问题定位与调优

https://mp.weixin.qq.com/s/kw3BYTXdyhnIyVZrnQuPew

基于深度学习和迁移学习的识花实践

https://mp.weixin.qq.com/s/F965Zu_PgA-1ZUGIQ0nIEQ

TensorFlow协同过滤推荐实战

https://mp.weixin.qq.com/s/KohwsQQetwjfTj-PXvLjwA

使用MNIST数据集，在TensorFlow上实现基础LSTM网络

http://mp.weixin.qq.com/s/ioaS7RQ6bsJs4_X0G4ZHyQ

如何优雅地用TensorFlow预测时间序列：TFTS库详细教程

https://mp.weixin.qq.com/s/pIESRzjsmqoO46P4x5Iqhw

Tensorflow卷积神经网络

https://mp.weixin.qq.com/s/Cge_GY19aZ1AcMkhW93C1A

TensorFlow中的那些高级API

https://mp.weixin.qq.com/s/kYOwUWlTP4T0IYKDWDbCsg

tensorflow object detection API训练公开数据集Oxford-IIIT Pets Dataset

https://mp.weixin.qq.com/s/8uDsaZjsiKXGea6M-w-RvA

tensorflow object detection API使用之GPU训练实现宠物识别

https://mp.weixin.qq.com/s/knw7yuUxHe-qeCLfj20onw

Bayesian GAN的TensorFlow实现

https://mp.weixin.qq.com/s/Sxui9CvdGocIxVG2FM4JtQ

基于tensorflow使用CNN-RNN进行中文文本分类！

https://mp.weixin.qq.com/s/X7ISLRMy5cddpmO5mev4MA

自创数据集，使用TensorFlow预测股票入门

https://mp.weixin.qq.com/s/kJxXIN6D5TEEFSFhGJNIyw

开源神经网络图片上色技术解析

https://mp.weixin.qq.com/s/qXMRHxDDRa-_rJZMhXWB4w

详解TensorFlow的新seq2seq模块及其用法

https://mp.weixin.qq.com/s/YdcIDXadEnDsyfc6Iu1gGw

手把手教你用TensorFlow训练模型

https://mp.weixin.qq.com/s/Off0pgaRNyik2nvjHaQQkw

在TensorFlow中对比两大生成模型：VAE与GAN

https://mp.weixin.qq.com/s/rMYjsIgFNvv47F4YZjY8SA

如何在K8S上玩转TensorFlow？

https://zhuanlan.zhihu.com/p/30751039

TensorFlow全新的数据读取方式：Dataset API入门教程

https://mp.weixin.qq.com/s/SDVQSn1aVDXk_RPGuVcQgQ

TensorFlow深度自动编码器入门实践

https://mp.weixin.qq.com/s/mZ79KAuSIJLtBEXvKwUi-w

如何利用TensorFlow.js部署简单的AI版“你画我猜”图像识别应用

https://mp.weixin.qq.com/s/atQHJ5U2pJpSG6PguN7J4Q

如何保持运动小车上的旗杆屹立不倒？TensorFlow利用A3C算法训练智能体玩CartPole游戏

https://mp.weixin.qq.com/s/yi-PNmMNMbwSi56aXo6ZSQ

tensorflow对象检测框架训练VOC数据集常见的两个问题

https://mp.weixin.qq.com/s/bcLCCvWrJLbMxwDl9GutjQ

TensorFlow动态图5行代码实现迁移学习-识别转变风格的MNIST

https://mp.weixin.qq.com/s/iQIRj7ifsOEnupYZuQsVwQ

是时候放弃TensorFlow集群，拥抱Horovod了
