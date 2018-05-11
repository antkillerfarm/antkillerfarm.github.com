---
layout: post
title:  深度学习（一）——MP神经元模型, BP算法
category: DL 
---

# 前言

神经网络本质上不是什么新东西。十年前，我还在上学的时候，就接触过皮毛。然而那时这玩意更多的还是学术界的屠龙之术，工业界几乎没有涉及。

及至近日重新拾起，方才发现，这十年正是神经网络蓬勃发展，逐渐进入应用阶段的十年。各种概念层出不穷，远非昔日模样。

Deep Learning虽然在学术界的大牛看来，属于旧概念的炒作。然而由于神经网络本身的非线性和连接的复杂性，其中的概念的确比一般的浅层算法复杂的多，从这个角度来说，称其为Deep，也算有些道理。

这里最主要的参考文献包括：

《机器学习》，周志华著。

>注：周志华，1973年生，南京大学博士（2000年）。南京大学教授。   
>从派系而言，周属于统计学派，对深度学习并不感冒。然而《机器学习》一书（又称西瓜书），已经成为ML入门的必读经典，因此这里也把它列为主要的参考文献之一。   
>在深度学习统治学界的时代，周依然能用gcForest、mcODM之类的算法，撑起统计学派的大旗，堪称国内一流的学者。

http://www.useit.com.cn/thread-13132-1-1.html

《Deep Learning Tutorial》，李宏毅著。

http://speech.ee.ntu.edu.tw/~tlkagk/courses.html

台湾大学李宏毅副教授的深度学习课程

>李宏毅，国立台湾大学硕士（2010年）+博士（2012年）。国立台湾大学计算机系助理教授。

https://www.csie.ntu.edu.tw/~yvchen/f106-adl/syllabus.html

李宏毅的搭档，美女陈蕴侬的深度学习课程

![](/images/article/yvchen.jpg)

>陈蕴侬，国立台湾大学本硕（2011年）+CMU博士（2015年）。国立台湾大学计算机系助理教授。

《Deep Learning》，Ian Goodfellow、Yoshua Bengio、Aaron Courville著。

原版：

http://www.deeplearningbook.org/

中文版：

https://github.com/exacity/deeplearningbook-chinese

>注：这本书基于markdown文件，使用tex编译而成，可作为编写大型书的代码参考项目。   
>安装方法：   
>`sudo apt install texlive-xetex texlive-lang-chinese texlive-science xindy`   
>`make`

《Deep Learning With Python》，Keras的作者Francois Chollet著。偏重于DL实战。

此外，Stanford、CMU、UCB等名校也有大量[相关课程](/resource/2018/01/24/course.html)。

其他参考文献将在各相关部分列出。

Deep Learning圈子的主要人物：

![](/images/article/DL_NB.png)

>注：Yann LeCun，1960年生，法国科学家。Pierre and Marie Curie University博士。Geoffrey Hinton是他博士后时代的导师。CNN的发明人。纽约大学教授，Facebook AI研究所主任。由于他的姓名发音非常东方化，因此被网友起了很多中文名如燕乐存、杨乐康等。2017.3，Yann访华期间正式公布中文名：杨立昆。

>Léon Bottou，法国科学家，随机梯度下降算法的发明人。

>Yoshua Bengio，1964年生，法国出生的加拿大科学家。深度学习的另一个宗师。

>这三个法国佬，都是好基友。只不过Yann LeCun和Yoshua Bengio研究神经网络，而Léon Bottou研究SVM，学术上分属不同派系。

>Geoffrey Everest Hinton，1947年生，英国出生的加拿大科学家，爱丁堡大学博士，多伦多大学教授。连接主义的代表人物，多层神经网络的宗师。英国皇家学会会员。

>一般将Geoffrey Hinton、Yann LeCun和Yoshua Bengio并称为深度学习的三大宗师。

![](/images/article/author_network.png)

上图是一个更大的DL牛人关系图。

https://mp.weixin.qq.com/s/w_LKb-xOdyBsQBRGmHo6zw

深度学习综述：Hinton、Yann LeCun和Bengio经典重读

# MP神经元模型

MP神经元模型是1943年，由Warren McCulloch和Walter Pitts提出的。

>注：Warren Sturgis McCulloch，1898~1969，美国神经生理学和控制论科学家。哥伦比亚大学博士，先后执教于MIT、Yale、芝加哥大学。

>Walter Harry Pitts, Jr.，1923~1969，美国计算神经学科学家。   
>这个人的经历，实在是非典型。家里贫穷，大约是读不起大学，15岁的时候，到芝加哥大学旁听Bertrand Russell的讲座。Russell很看重这个年轻人，但由于他只是访问学者，于是在回国之前，将Pitts介绍给Rudolf Carnap，后者为Pitts安排了一份在学校打杂的工作。这一打杂就是五六年时间，最后凭借论文，获得芝加哥大学的准学士学位（因为他始终都不是正式学籍的学生），这也是他一生唯一的学位。   
>但是如果看看Pitts的合作者的阵容，就知道Pitts水平之高了。他们是：Warren McCulloch、Jerome Lettvin、Norbert Wiener。

MP神经元模型如下图所示：

![](/images/article/MP_model.jpg)

即：

$$y_j=\sum _{i=1}^nw_{ij}x_i-\theta_j$$

上式其实就是《机器学习（一）》中提到的逻辑回归。

生物神经元和MP神经元模型的对应关系如下表：

| 生物神经元 | MP神经元模型 |
|:--:|:--:|
| 神经元 | $$j$$ |
| 输入信号 | $$x_i$$ |
| 权值 | $$w_{ij}$$ |
| 输出信号 | $$y_j$$ |
| 总和 | $$\sum$$ |
| 膜电位 | $$\sum _{i=1}^nw_{ij}x_i$$ |
| 阈值 | $$\theta_j$$ |

从上图亦可看出，如果将阈值看作输入为-1.0的哑节点的连接权重，则权重和阈值可统一为权重。神经网络训练的过程，实际上就是根据样本调整权重和阈值的过程。

参考：

http://blog.csdn.net/u013007900/article/details/50066315

神经网络学习之M-P模型

# 单层感知器 vs. 多层感知器

神经网络的层数越多，其表达力越丰富，如下表所示：

![](/images/article/single_layer_vs_multi_layer.png)

# ANN简史

![](/images/article/ANN_history.png)

参考：

https://mp.weixin.qq.com/s/0OqqdbUWlWIbfBrRdMs4PA

洪小文：以科学的方式赤裸裸地剖析人工智能：混沌初开

https://mp.weixin.qq.com/s/_G08-3g4QPau2_ZLcsm6-Q

洪小文：以科学的方式赤裸裸地剖析AI（二）：从寒冬到复兴

https://mp.weixin.qq.com/s/yWBcK5mEK0AxusVnPt0VNA

洪小文：以科学的方式赤裸裸地剖析AI（三）：人的智慧在哪里？

https://mp.weixin.qq.com/s/DkAFMDOnJKkdpV7bnkSZqQ

洪小文：以科学的方式赤裸裸地剖析AI（四）：未来是人工智能+人类智能

# BP算法

单层神经网络的学习算法最早由Donald Olding Hebb提出，因此又被叫做Hebb算法。但是这种算法无法扩展到多层神经网络，这最终导致了AI的第一个冬天，直到BP算法的出现。

>Donald Olding Hebb，1904~1985,加拿大心理学家，哈佛博士（1936），McGill University教授。英国皇家学会会员。神经心理学和神经网络之父。

误差逆传播（error BackPropagation）算法最早由Paul J. Werbos于1974年提出，然而此时正值ANN的低谷，未得到人们的重视。因此到了1986年时，由David Everett Rumelhart重新发明了该算法。

>注：Paul J. Werbos，1947年生，哈佛大学博士。

>David Everett Rumelhart，1942~2011，美国心理学家。斯坦福大学博士，先后执教于UCSD和斯坦福。美国科学院院士。

![](/images/article/BP_network.png)

BP算法的核心思路：

1.利用前向传导公式，计算第n层输出值。

2.计算输出值和实际值的残差。

3.将残差梯度传递回第$$n-1,n-2,\dots,2$$层，并修正各层参数。（即所谓的误差逆传播）

BP算法的推导过程教材已经写的很好了，这里只对要点做一个摘录。

## 链式法则

Chain Rules本来是微积分中，用于求一个复合函数导数的常用法则。这里用来进行残差梯度的逆传播。

由《机器学习（一）》的公式3可得：

$$\Delta w_{hj}=-\eta\frac{\partial E_k}{\partial w_{hj}}$$

$$w_{hj}$$先影响$$\beta_j$$，再影响$$\hat y_j^k$$，然后影响误差$$E_k$$，因此有：

$$\frac{\partial E_k}{\partial w_{hj}}=\frac{\partial E_k}{\partial \hat y_j^k}\cdot \frac{\partial \hat y_j^k}{\partial \beta_j}\cdot \frac{\partial \beta_j}{\partial w_{hj}}\tag{1}$$

![](/images/article/chain_rule.png)

## 随机初始化

神经网络的参数的**随机初始化**的目的是使对称失效。否则的话，所有对称结点的权重都一致，也就无法区分并学习了。

随机初始化的方法有如下几种：

1.Gaussian。用给定均值和方差的Gaussian分布设定随机值。这也是最常用的方法。

2.Xavier。该方法基于Gaussian分布或均匀分布产生随机数。其中分布W的均值为零，方差公式如下：

$$\text{Var}(W)=\frac{1}{n_{in}}$$

其中，$$n_{in}$$表示需要输入层的神经元的个数。也有如下变种：

$$\text{Var}(W)=\frac{2}{n_{in}+n_{out}}$$

其中，$$n_{out}$$表示需要输出层的神经元的个数。

3.MSRA。该方法基于零均值的Gaussian分布产生随机数。Gaussian分布的标准差为：

$$\sqrt{\frac{2}{n_l}}$$

其中，$$n_l=k_l^2d_{l-1}$$，$$k_l$$表示l层的filter的大小，$$d_{l-1}$$表示l-1层的filter的数量。

参见：

http://blog.csdn.net/xizero00/article/details/51013088

Different Methods for Weight Initialization in Deep Learning

https://mp.weixin.qq.com/s/xqWli1xnsGkqYDUjgvOnkQ

反向传播神经网络极简入门

https://mp.weixin.qq.com/s/PhxkfWH5bEbykMKGEtDScA

为什么神经网络参数不能够全部初始化为全0？

https://mp.weixin.qq.com/s/s-v7T0k2gy7ZRnrFCUpTYg

通过方差分析详解最流行的Xavier权重初始化方法

https://mp.weixin.qq.com/s/r1OJoLa_t8QwNcL4kfx5uQ

神经网络参数随机初始化已经过时了

https://mp.weixin.qq.com/s/mFA2PeO70o3HR6AQ74Lp3g

深度学习最佳实践之权重初始化

https://mp.weixin.qq.com/s/M1TswiDh-LkH9G7jCQ_UqA

神经网络编程-前向传播和后向传播

https://mp.weixin.qq.com/s/Dygdn0Xzpx40-zUQadiiHg

通过梯度检验帮助实现反向传播

# BP算法的缺点

虽然传统的BP算法，理论上可以支持任意深度的神经网络。然而实际使用中，却很少能支持3层以上的神经网络。

![](/images/article/sigmoid.png)

如上图所示，sigmoid函数不是线性的，一个小的输出值的改变，对应了比较大的输入值改变。换句话说，就是输出值的梯度较大，而输入值的梯度较小。而梯度在基于梯度下降的优化问题中，是至关重要的。

随着层数的增多，反向传递的残差梯度会越来越小，这样的现象，被称作**梯度消失**（Vanishing Gradient）。它导致的结果是，虽然靠近输出端的神经网络已经训练好了，但输入端的神经网络仍处于随机状态。也就是说，靠近输入端的神经网络，有和没有都是一样的效果，完全体现不了深度神经网络的优越性。

和梯度消失相反的概念是**梯度爆炸**（Vanishing Explode），也就是神经网络无法收敛。

参考：

https://mp.weixin.qq.com/s/w7EbDI9MQBZF67XM-cV1eQ

一文了解神经网络中的梯度爆炸

