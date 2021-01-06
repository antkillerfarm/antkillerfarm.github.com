---
layout: post
title:  机器学习（三十五）——Probabilistic Robotics, Kalman filters
category: ML 
---

* toc
{:toc}

# Probabilistic Robotics

这篇心得主要根据Sebastian Thrun的Probabilistic Robotics课程的ppt来写。

>注：Sebastian Thrun，德国波恩大学博士（1995年）。先后执教于CMU和Stanford。

网址：

http://robots.stanford.edu/probabilistic-robotics/ppt/

## 贝叶斯过滤器

假定我们需要根据测量值z来判断门的开关。显然，这里的$$P(open\mid z)$$是诊断式（**diagnostic**）问题，而$$P(z\mid open)$$是因果式（**causal**）问题。通常来说，后者比较容易获取，而前者可以基于后者使用贝叶斯公式计算得到。

一般将$$P(z\mid x)$$称为**Sensor model**。

针对多相关测量值问题，这里有一个和朴素贝叶斯假设相仿的**Markov assumption**——假设$$z_n$$独立于$$z_1,\dots,z_{n-1}$$（即“现在”不依赖于“过去”），则：

$$
\begin{array}\\
P(x\mid z_1,\dots,z_n)=\frac{P(z_n\mid x)P(x\mid z_1,\dots,z_{n-1})}{P(z_n\mid z_1,\dots,z_{n-1})}(\text{Bayes})\\
=\eta P(z_n\mid x)P(x\mid z_1,\dots,z_{n-1})=\eta_{1,\dots,n}\prod_{i=1}^nP(z_i\mid x)P(x)(\text{Markov})
\end{array}
$$

>注：以下的推导过程注释中，如无特别说明。均以Bayes指代Bayes' theorem，以Markov指代Markov assumption。

上式中的$$\eta$$表示概率的归一化系数。

除了测量值z之外，一般的控制系统中还有动作（Action）的概念。比如打开门就是一个Action。Action会导致系统的状态发生改变（也可不变）。如下图所示：

![](/images/article/state_trans.png)

通常，将$$P(x\mid u,x')$$称作**Action Model**。其中，u表示Action，而x'表示系统的上一个状态。

一般的，**新的测量值会减少系统的不确定度，而新的Action会增加系统的不确定度。**

综上，一个贝叶斯过滤器（Bayes Filters）的框架包括：

输入：

1.观测值z和Action u的序列：$$d_t=\{u_1,z_1,\dots,u_t,z_t\}$$

2.Sensor model：$$P(z\mid x)$$

3.Action model：$$P(x\mid u,x')$$

4.系统状态的先验概率：$$P(x)$$

输出：

1.估计动态系统的状态X。

2.状态的后验概率，也叫**Belief**：

$$\begin{align}
\mathbf{Bel(x_t)}&=P(x_t\mid u_1,z_1,\dots,u_t,z_t)
\\&=\eta P(z_t\mid x_t,u_1,z_1,\dots,u_t)P(x_t\mid u_1,z_1,\dots,u_t)(\text{Bayes})
\\&=\eta P(z_t\mid x_t)P(x_t\mid u_1,z_1,\dots,u_t)(\text{Markov})
\\&=\eta P(z_t\mid x_t)\int P(x_t\mid u_1,z_1,\dots,u_t,x_{t-1})P(x_{t-1}\mid u_1,z_1,\dots,u_t)\mathrm{d}x_{t-1}(\text{Total prob.})
\\&=\eta P(z_t\mid x_t)\int P(x_t\mid u_t,x_{t-1})P(x_{t-1}\mid u_1,z_1,\dots,u_t)\mathrm{d}x_{t-1}(\text{Markov})
\\&=\eta P(z_t\mid x_t)\int P(x_t\mid u_t,x_{t-1})P(x_{t-1}\mid u_1,z_1,\dots,z_{t-1})\mathrm{d}x_{t-1}(\text{Markov})
\\&=\eta P(z_t\mid x_t)\int P(x_t\mid u_t,x_{t-1})\mathbf{Bel(x_{t-1})}\mathrm{d}x_{t-1}
\end{align}$$

上式也可以写作：

**预测**：

$$\overline{\mathbf{Bel(x_t)}}=\int P(x_t\mid u_t,x_{t-1})\mathbf{Bel(x_{t-1})}\mathrm{d}x_{t-1}$$

**修正**：

$$\mathbf{Bel(x_t)}=\eta P(z_t\mid x_t)\overline{\mathbf{Bel(x_t)}}$$

熟悉卡尔曼滤波的同学大概已经看出来了。没错！贝叶斯过滤器是一大类算法的统称。这些算法包括Kalman filters、Particle filters、Hidden Markov models、Dynamic Bayesian networks、Partially Observable Markov Decision Processes (POMDPs)等。

## 递归最小二乘法

Recursive Least Squares

http://www.doc88.com/p-6836102414531.html

RLS自适应算法基本原理

https://blog.csdn.net/HJ199404182515/article/details/52504150

浅谈自适应滤波器

# Kalman filters

>注：Rudolf (Rudi) Emil Kálmán，1930～2016，匈牙利出生的美国科学家。哥伦比亚大学博士（1957），先后执教于斯坦福大学和佛罗里达大学。现代控制理论的里程碑人物，美国科学院院士。   
>卡尔曼滤波从纯数学的角度讲，并没有多大意义。因此，主流数学家们在很长一段时间内，并不承认Kálmán是数学家。只是由于卡尔曼滤波在工程界的巨大影响力，才不得不于2012年，授予其美国数学协会院士。

| 名称 | 使用场景 |
|:--:|:--:|
| Kalman filters | Linear Gaussian |
| Extended Kalman filter | Nonlinear Gaussian |
| Iterated EKF | Nonlinear Gaussian |
| Unscented Kalman filters | Nonlinear Gaussian |
| Particle filter | Nonlinear Non-Gaussian |

unscented transformation

![](/images/img2/UKF.png)

参考：

http://www.cs.unc.edu/~welch/media/pdf/kalman_intro.pdf

Gregory Francis Welch写的卡尔曼滤波科普文。

>注：Gregory Francis Welch，北卡罗莱娜大学博士（1997）。中佛罗里达大学教授。

http://www.cs.unc.edu/~welch/kalman/media/misc/kalman_intro_chinese.zip

上文的中文版。

https://zhuanlan.zhihu.com/p/21294526

知乎诸位大神的科普文。

http://www.docin.com/p-976961701.html

动态相对定位中自适应滤波方法的研究

《自适应动态导航定位》，杨元喜著。

>注：杨元喜，1956年生，大地测量学家。中国科学院院士。

https://zhuanlan.zhihu.com/c_1131936304564453376

专栏：现代控制理论

https://zhuanlan.zhihu.com/ClassicControl

专栏：经典控制理论

https://blog.csdn.net/tiandijun/article/details/72469471

通俗理解卡尔曼滤波及其算法实现

https://zhuanlan.zhihu.com/p/40413223

卡尔曼滤波器实现详解

https://mp.weixin.qq.com/s/kBGhHCxq6idOOSGoLX5Kaw

手把手教你写卡尔曼滤波器

https://mp.weixin.qq.com/s/x0twRIdONCp3-qjhFJuCEQ

手把手教你写扩展卡尔曼滤波器

https://mp.weixin.qq.com/s/Nta9ksUkAVoX8arBGm7qqg

手把手教你实现多传感器融合技术

https://zhuanlan.zhihu.com/p/41767489

概率机器人——扩展卡尔曼滤波、无迹卡尔曼滤波

https://mp.weixin.qq.com/s/J27fVvYMRdoAgQtfXsywxg

OpenCV卡尔曼滤波介绍与代码演示

https://zhuanlan.zhihu.com/p/64007212

卡尔曼滤波家族

https://zhuanlan.zhihu.com/p/66646519

IKF(IEKF)推导

https://blog.csdn.net/baidu_21807307/article/details/51843079

浅谈卡尔曼滤波（Kalman Filter）

https://mp.weixin.qq.com/s/ZlyF1GcmpwZoT-3o4T567A

卡尔曼滤波算法及其应用

https://zhuanlan.zhihu.com/p/77327349

如何理解那个能造导弹军舰还把嫦娥送上天的卡尔曼滤波算法Kalman filter?

https://mp.weixin.qq.com/s/vAm0QDp_i2ec5pZbTmdHNA

傻瓜也能懂的卡尔曼滤波器

https://mp.weixin.qq.com/s/na0vVhECfBppTb7BmhRyzA

从限价订单薄中推导预测因子：卡尔曼滤波来搞定！

https://mp.weixin.qq.com/s/v460ql4RnJGbzbV0iZH4kA

深度解读卡尔曼滤波原理

https://mp.weixin.qq.com/s/Jlux3pZ4keVzkwWzgoPrEQ

深入浅出讲解卡尔曼滤波

https://mp.weixin.qq.com/s/BeIEjASATt3Qpef_Ag-cSw

卡尔曼滤波系列——经典卡尔曼滤波推导

https://mp.weixin.qq.com/s/t4GIPMB-6Vq7i8Q5PN6L-w

追狗，从入门到精通

https://mp.weixin.qq.com/s/EZ4JQM2vynTevUjFpW1h_w

追狗，从入门到精通2.0

https://zhuanlan.zhihu.com/p/128520715

自动驾驶定位技术-马尔科夫定位

https://zhuanlan.zhihu.com/p/138684962

自动驾驶感知融合-卡尔曼及扩展卡尔曼滤波(Lidar&Radar)

https://zhuanlan.zhihu.com/p/141059329

自动驾驶感知融合-无迹卡尔曼滤波(Lidar&Radar)

https://zhuanlan.zhihu.com/p/134595781

卡尔曼滤波(Kalman filter)含详细数学推导

https://zhuanlan.zhihu.com/p/166342719

卡尔曼滤波器详解——从零开始(1)

https://zhuanlan.zhihu.com/p/179480833

卡尔曼滤波器详解——从零开始(2)

https://mp.weixin.qq.com/s/3K9qdH9FXnYABpJoJkFcqw

使用卡尔曼滤波平滑时间序列，提高时序预测的准确率

https://zhuanlan.zhihu.com/p/35978617

线性动态系统与卡尔曼滤波

https://mp.weixin.qq.com/s/2rX6iRTYBk47V29fSTAMQQ

图解卡尔曼滤波(Kalman Filter)

https://mp.weixin.qq.com/s/PuvTkDhwbYv8TK8t-Zhcug

基于卡尔曼滤波的注意力机制—广告点击率预估中的用户行为建模   

https://longaspire.github.io/blog/%E5%8D%A1%E5%B0%94%E6%9B%BC%E6%BB%A4%E6%B3%A2/

卡尔曼滤波器

https://zhuanlan.zhihu.com/p/36745755

卡尔曼滤波：从入门到精通

# Loss function详解+

https://mp.weixin.qq.com/s/CPfhGxig9BMAgimBSOLy3g

用于图像检索的等距离等分布三元组损失函数

https://www.zhihu.com/question/375794498

深度学习的多个loss如何平衡？

https://mp.weixin.qq.com/s/tzY_lG0F9dP5Q-LmwuHLmQ

常见损失函数和评价指标总结

https://mp.weixin.qq.com/s/lw9frtqocqsS-q2KGfzO1Q

深入理解计算机视觉中的损失函数

https://mp.weixin.qq.com/s/_HQ5an_krRCYMVnwEgGJow

深度学习的多个loss如何平衡 & 有哪些“魔改”损失函数，曾经拯救了你的深度学习模型？

https://blog.csdn.net/shanglianlm/article/details/85019768

十九种损失函数

https://mp.weixin.qq.com/s/8oKiVRjtPQIH1D2HltsREQ

图像分割损失函数最全面、最详细总结

https://zhuanlan.zhihu.com/p/158853633

一文理解Ranking Loss/Margin Loss/Triplet Loss

https://zhuanlan.zhihu.com/p/235533342

目标检测：Loss整理

https://zhuanlan.zhihu.com/p/191355122

NLP样本不均衡之常用损失函数对比

https://mp.weixin.qq.com/s/KL_D8pWtcXCJHz7dd70jyw

Face Recognition Loss on Mnist with Pytorch

https://zhuanlan.zhihu.com/p/77686118

机器学习常用损失函数小结

https://zhuanlan.zhihu.com/p/38855840

SphereReID：从人脸到行人，Softmax变种效果显著

https://mp.weixin.qq.com/s/ZoLO6OilivPgle03KdNzCQ

人脸识别中Softmax-based Loss的演化史

https://mp.weixin.qq.com/s/DwtA6GivVCDvL4MXNDBFWg

阿里巴巴提出DR Loss：解决目标检测的样本不平衡问题

https://zhuanlan.zhihu.com/p/145927429

DR Loss

https://mp.weixin.qq.com/s/x0aBo-w669_2FyCGKWJ4iQ

理解计算机视觉中的损失函数

https://mp.weixin.qq.com/s/LOewKsxtWm7dFJS6ioryuw

Siamese网络，Triplet Loss以及Circle Loss的解释

https://zhuanlan.zhihu.com/p/58883095

常见的损失函数(loss function)总结

https://mp.weixin.qq.com/s/UjBCjwNDIxDoAoyQAf8V6A

旷视研究院提出Circle Loss，革新深度特征学习范式

https://mp.weixin.qq.com/s/5RpbXzuHp_tR6C_nBdiXGA

Circle Loss：从统一的相似性对的优化角度进行深度特征学习

https://zhuanlan.zhihu.com/p/304462034

根据标签分布来选择损失函数
