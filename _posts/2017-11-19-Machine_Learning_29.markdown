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

# TensorFlow+

https://mp.weixin.qq.com/s/OVWbxBNc4i0_5jgy06xS1A

基于Tensorflow Estimators的文本分类

https://mp.weixin.qq.com/s/c_2_9gvOynHaVW6pi4qQjQ

用TensorFlow让机器人唱首歌给你听

https://mp.weixin.qq.com/s/hn-LqyREkusxP2TOWfTJ6g

使用TensorFlow官方Java API调用TensorFlow模型

https://mp.weixin.qq.com/s/kS92vYyeHLc38RGc_4CZbg

如何应用TFGAN快速实践生成对抗网络？

https://mp.weixin.qq.com/s/hquOoKeeHQXqWcHM6Bkvbw

如何训练一个简单的音频识别网络

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
