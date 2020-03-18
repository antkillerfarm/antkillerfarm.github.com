---
layout: post
title:  深度学习（一）——前言, MP神经元模型, BP算法
category: DL 
---

# 前言

神经网络本质上不是什么新东西。十几年前，我还在上学的时候，就接触过皮毛。然而那时这玩意更多的还是学术界的屠龙之术，工业界几乎没有涉及。

及至近日重新拾起，方才发现，这十年正是神经网络蓬勃发展，逐渐进入应用阶段的十年。各种概念层出不穷，远非昔日模样。

Deep Learning虽然在学术界的大牛看来，属于旧概念的炒作。然而由于神经网络本身的非线性和连接的复杂性，其中的概念的确比一般的浅层算法复杂的多，从这个角度来说，称其为Deep，也算有些道理。

这里最主要的参考文献包括：

《机器学习》，周志华著。

>周志华，1973年生，南京大学博士（2000年）。南京大学教授。   
>从派系而言，周属于统计学派，对深度学习并不感冒。然而《机器学习》一书（又称西瓜书），已经成为ML入门的必读经典，因此这里也把它列为主要的参考文献之一。   
>在深度学习统治学界的时代，周依然能用gcForest、mcODM之类的算法，撑起统计学派的大旗，堪称国内一流的学者。

http://www.useit.com.cn/thread-13132-1-1.html

《Deep Learning Tutorial》，李宏毅著。

http://speech.ee.ntu.edu.tw/~tlkagk/courses.html

台湾大学李宏毅副教授的深度学习课程

>李宏毅，国立台湾大学硕士（2010年）+博士（2012年）。国立台湾大学计算机系助理教授。

李宏毅的课程难度适中，内容全面，且更新很快，每隔半年逛一下，必有新收获。

https://www.csie.ntu.edu.tw/~yvchen/f106-adl/syllabus.html

李宏毅的搭档，美女陈蕴侬的深度学习课程

![](/images/article/yvchen.jpg)

>陈蕴侬，国立台湾大学本硕（2011年）+CMU博士（2015年）。国立台湾大学计算机系助理教授。

http://introtodeeplearning.com/

MIT 6.S191 Introduction to Deep Learning。涵盖深度学习导论、序列建模、深度视觉、生成模型、强化学习、图神经网络、对抗学习、贝叶斯模型、神经渲染、机器学习嗅觉等方面，是DL领域的综合课程。

《Deep Learning》，Ian Goodfellow、Yoshua Bengio、Aaron Courville著。

原版：

http://www.deeplearningbook.org/

中文版：

https://github.com/exacity/deeplearningbook-chinese

>这本书基于markdown文件，使用tex编译而成，可作为编写大型书的代码参考项目。   
>安装方法：   
>`sudo apt install texlive-xetex texlive-lang-chinese texlive-science xindy`   
>`make`

《Deep Learning With Python》，Keras的作者Francois Chollet著。偏重于DL实战。

此外，Stanford、CMU、UCB等名校也有大量[相关课程](/resource/2018/01/24/course.html)。

其他参考文献将在各相关部分列出。

Deep Learning圈子的主要人物：

![](/images/article/DL_NB.png)

>Yann LeCun，1960年生，法国科学家。Pierre and Marie Curie University博士。Geoffrey Hinton是他博士后时代的导师。CNN的发明人。纽约大学教授，Facebook AI研究所主任。由于他的姓名发音非常东方化，因此被网友起了很多中文名如燕乐存、杨乐康等。2017.3，Yann访华期间正式公布中文名：杨立昆。

>Léon Bottou，法国科学家，随机梯度下降算法的发明人。

>Yoshua Bengio，1964年生，法国出生的加拿大科学家。深度学习的另一个宗师。

>这三个法国佬，都是好基友。只不过Yann LeCun和Yoshua Bengio研究神经网络，而Léon Bottou研究SVM，学术上分属不同派系。

>Geoffrey Everest Hinton，1947年生，英国出生的加拿大科学家，爱丁堡大学博士，多伦多大学教授。连接主义的代表人物，多层神经网络的宗师。英国皇家学会会员。

>一般将Geoffrey Hinton、Yann LeCun和Yoshua Bengio并称为深度学习的三大宗师。

>2019.3.27 三巨头被共同授予2018年度图灵奖。

![](/images/article/author_network.png)

上图是一个更大的DL牛人关系图。

https://mp.weixin.qq.com/s/w_LKb-xOdyBsQBRGmHo6zw

深度学习综述：Hinton、Yann LeCun和Bengio经典重读

![](/images/img2/AI.jpg)

上图是李开复提出了AI替代人工的象限图。非创意和非关怀类的职业会逐渐被AI所取代。

# MP神经元模型

MP神经元模型是1943年，由Warren McCulloch和Walter Pitts提出的。

>Warren Sturgis McCulloch，1898~1969，美国神经生理学和控制论科学家。哥伦比亚大学博士，先后执教于MIT、Yale、芝加哥大学。

>Walter Harry Pitts, Jr.，1923~1969，美国计算神经学科学家。   
>这个人的经历，实在是非典型。家里贫穷，大约是读不起大学，15岁的时候，到芝加哥大学旁听Bertrand Russell的讲座。Russell很看重这个年轻人，但由于他只是访问学者，于是在回国之前，将Pitts介绍给Rudolf Carnap，后者为Pitts安排了一份在学校打杂的工作。这一打杂就是五六年时间，最后凭借论文，获得芝加哥大学的准学士学位（因为他始终都不是正式学籍的学生），这也是他一生唯一的学位。   
>但是如果看看Pitts的合作者的阵容，就知道Pitts水平之高了。他们是：Warren McCulloch、Jerome Lettvin、Norbert Wiener。

MP神经元模型如下图所示：

![](/images/article/MP_model.jpg)

即：

$$y_j=f\left(\sum _{i=1}^nw_{ij}x_i-\theta_j\right)$$

上式其实就是《机器学习（一）》中提到的逻辑回归。f被称为称为激活函数(Activation Function)或转移函数(Transfer Function)，用以提供**非线性表达能力**。

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

宽度也有类似的现象：

![](/images/img3/infinite_width.gif)

参考：

https://mp.weixin.qq.com/s/W0mVk_KtL2Tr_Uo-1el7Aw

5行代码打造无限宽神经网络模型

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

https://www.visualcapitalist.com/ai-revolution-infographic/

Visualizing the AI Revolution in One Infographic

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

值得注意的是残差梯度实际上包括两部分：$$\Delta x$$和$$\Delta w$$。如下图所示：

![](/images/img2/BP.png)

其中，$$\Delta x$$和$$\Delta w$$分别是$$\Delta$$在x和w的偏导数方向上的分量。$$\Delta x$$用于向上层传递梯度，而$$\Delta w$$用于更新权值w。

通常来说，我们只需要更新权值w，但少数情况下，w和x可能都需要更新，这时只要分别计算w和x的偏导，并更新即可。

除了基于梯度下降的BP算法之外，还有基于GA（genetic algorithm）的BP算法，但基本只有学术界还在尝试。

## 随机初始化

神经网络的参数的**随机初始化**的目的是使对称失效。否则的话，所有对称结点的权重都一致，也就无法区分并学习了。

随机初始化的方法有如下几种：

1.Gaussian。用给定均值和方差的Gaussian分布设定随机值。这也是最常用的方法。

2.Xavier。该方法基于Gaussian分布或均匀分布产生随机数。其中分布W的均值为零，方差公式如下：

$$\text{Var}(W)=\frac{1}{n_{in}}\tag{1}$$

其中，$$n_{in}$$表示需要输入层的神经元的个数。也有如下变种：

$$\text{Var}(W)=\frac{2}{n_{in}+n_{out}}\tag{2}$$

其中，$$n_{out}$$表示需要输出层的神经元的个数。

公式1也被称作LeCun initializer，公式2也被称作Glorot initializer。
