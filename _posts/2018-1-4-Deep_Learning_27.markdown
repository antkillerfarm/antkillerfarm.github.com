---
layout: post
title:  深度学习（二十七）——MobileNet, NetVLAD, 李飞飞, RBM & DBN & Deep Autoencoder
category: DL 
---

# MobileNet

论文：

《MobileNets: Efficient Convolutional Neural Networks for Mobile Vision Applications》

代码：

https://github.com/Zehaos/MobileNet

![](/images/article/dwl_pwl.png)

参考：

https://mp.weixin.qq.com/s/f3bmtbCY5BfA4v3movwLVg

向手机端神经网络进发：MobileNet压缩指南

https://mp.weixin.qq.com/s/mcK8M6pnHiZZRAkYVdaYGQ

MobileNet在手机端上的速度评测：iPhone 8 Plus竟不如iPhone 7 Plus

https://mp.weixin.qq.com/s/2XqBeq3N4mvu05S1Jo2UwA

CNN模型之MobileNet

https://mp.weixin.qq.com/s/fdgaDoYm2sfjqO2esv7jyA

Google论文解读：轻量化卷积神经网络MobileNetV2

https://mp.weixin.qq.com/s/7vFxmvRZuM2DqSYN7C88SA

谷歌发布MobileNetV2：可做语义分割的下一代移动端计算机视觉架构

https://mp.weixin.qq.com/s/lu0GHCpWCmogkmHRKnJ8zQ

浅析两代MobileNet

https://mp.weixin.qq.com/s/T6S1_cFXPEuhRAkJo2m8Ig

轻量级CNN网络之MobileNetv2

https://mp.weixin.qq.com/s/RRu3r_dokORhpSq3eyrPDQ

为什么MobileNet及其变体如此之快？

# NetVLAD

NetVLAD算的上是CNN+传统算子的一个范例。

论文：

《NetVLAD: CNN architecture for weakly supervised place recognition》

《GhostVLAD for set-based face recognition》

数据集：

http://places.csail.mit.edu/

## VLAD

Vector of Locally Aggregated Descriptors

https://www.cnblogs.com/minemine/p/7364950.html

场景分类(scene classification)摘录

http://www.cnblogs.com/mafuqiang/p/6909556.html

图像检索——VLAD

## 参考

https://www.oukohou.wang/2018/11/27/NetVLAD/

论文阅读-NetVLAD

https://www.oukohou.wang/2018/12/26/GhostVLAD/

论文阅读-GhostVLAD

https://mp.weixin.qq.com/s/cfUl0Eym0mu7rSJJL7Zt1A

基于深度学习的视觉实例搜索研究进展

https://zhuanlan.zhihu.com/p/25013378

深度纹理编码网络 (Deep TEN: Texture Encoding Network)

https://blog.csdn.net/LiGuang923/article/details/85416407

图像检索与降维（一）：VLAD

https://blog.csdn.net/LiGuang923/article/details/85470289

图像检索与降维（二）：NetVLAD

# 李飞飞

## AI大佬

李飞飞是吴恩达之后的华裔AI新大佬。巧合的是，他们都是斯坦福AP+AI lab的主任，只不过吴是李的前任而已。

**李飞飞（Fei-Fei Li）**，1976年生，成都人，16岁移民美国。普林斯顿大学本科（1995～1999）+加州理工学院博士（2001～2005）。先后执教于UIUC、普林斯顿、斯坦福等学校。

个人主页：

http://vision.stanford.edu/feifeili/

她的老公Silvio Savarese，也是斯坦福的AP。

## 大佬的门徒

比如可爱的妹子**Serena Yeung**。这个妹子是斯坦福的本硕博。出身不详，但从姓名的英文拼法来看，应该是美国土生的华裔。Yeung是杨、阳、羊等姓的传统英文拼法，但显然不是大陆推行的拼音拼法。（可以对比的是Fei-Fei Li和Bruce Lee，对于同一个姓的不同拼法。）

个人主页：

http://ai.stanford.edu/~syyeung/

还有当红的“辣子鸡”：**Andrej Karpathy**，多伦多大学本科（2009）+英属不列颠哥伦比亚大学硕士（2011）+斯坦福博士（2015）。现任特斯拉AI总监。

吐槽一下：英属不列颠哥伦比亚大学其实是加拿大的一所大学。

个人主页：

http://cs.stanford.edu/people/karpathy/

Andrej Karpathy建了一个检索arxiv的网站，主要搜集了近3年来的ML/DL领域的论文。网址：

http://www.arxiv-sanity.com/

**李佳（Jia Li）**，李飞飞的开山大弟子，追随她从UIUC、普林斯顿到斯坦福。目前又追随其到Google。大约是知道自己的名字是个大路货，她的笔名叫做Li-Jia Li。

个人主页：

http://vision.stanford.edu/lijiali/

## 学神

应该说李飞飞和吴恩达都是万里挑一的超卓人物，但是和学神还是有所差距。下面是两个80后的华裔学神，他们都已经是正教授了：

**尹希**，1983年生，哈佛大学物理系教授。

**张锋**，1982年生，MIT教授，生物学家。

这两个人都是有机会挑战诺奖的人，而李和吴暂时还没有这个可能性。

## 网红

这里收录了一些非李飞飞门下的AI网红。

**Zachary Chase Lipton**，1985年生，哥伦比亚大学本科+UCSD博士，CMU的AP。他的另一身份——Jazz歌手，可比他的学术成就知名多了。

个人主页：

http://zacklipton.com/

# RBM & DBN & Deep Autoencoder

## RBM

Restricted Boltzmann Machines由Hinton发明，是一种用于降维、分类、回归、协同过滤、特征学习和主题建模的算法。

![](/images/img2/multiple_inputs_RBM.png)

在重构阶段，第一隐藏层的激活值成为反向传递中的输入。这些输入值与同样的权重相乘，每两个相连的节点之间各有一个权重，就像正向传递中输入x的加权运算一样。这些乘积的和再与每个可见层的偏差相加，所得结果就是重构值，亦即原始输入的近似值。这一过程可以用下图来表示：

![](/images/img2/reconstruction_RBM.png)

由于RBM权重初始值是随机决定的，重构值与原始输入之间的差别通常很大。可以将r值与输入值之差视为重构误差，此误差值随后经由反向传播来修正RBM的权重，如此不断反复，直至误差达到最小。

由此可见，RBM在正向传递中使用输入值来预测节点的激活值，亦即输入为加权的x时输出的概率：$$p(a\mid x; w)$$。

但在反向传递时，激活值成为输入，而输出的是对于原始数据的重构值，或者说猜测值。此时RBM则是在尝试估计激活值为a时输入为x的概率，激活值的加权系数与正向传递中的权重相同。 第二个阶段可以表示为$$p(x\mid a; w)$$。

上述两种预测值相结合，可以得到输入x和激活值a的联合概率分布，即$$p(x, a)$$。

重构与回归、分类运算不同。回归运算根据许多输入值估测一个连续值，分类运算是猜测应当为一个特定的输入样例添加哪种具体的标签。

而重构则是在猜测原始输入的概率分布，亦即同时预测许多不同的点的值。这被称为生成学习，必须和分类器所进行的判别学习区分开来，后者是将输入值映射至标签，用直线将数据点划分为不同的组。

RBM用KL散度来衡量预测的概率分布与输入值的基准分布之间的距离。

最后一点：你会发现**RBM有两个偏差值**。这是RBM与其他自动编码器的区别所在。隐藏的偏差值帮助RBM在正向传递中生成激活值（因为偏差设定了下限，所以无论数据有多稀疏，至少有一部分节点会被激活），而可见层的偏差则帮助RBM通过反向传递学习重构数据。

权重能够近似模拟出数据的特征后，也就为下一步的学习奠定了良好基础，比如可以在随后的有监督学习阶段使用深度置信网络来对图像进行分类。

RBM有许多用途，其中最强的功能之一就是对权重进行合理的初始化，为之后的学习和分类做好准备。从某种意义上来说，RBM的作用与反向传播相似：让权重能够有效地模拟数据。可以认为预训练和反向传播是实现同一个目的的不同方法，二者可以相互替代。

