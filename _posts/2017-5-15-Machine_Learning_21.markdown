---
layout: post
title:  机器学习（二十一）——Loss function详解
category: ML 
---

* toc
{:toc}

# Loss function详解（续）

## Weighted softmax loss

假如有一个二分类问题，两类的样本数目差距非常之大。比如图像任务中的边缘检测问题，它可以看作是一个逐像素的分类问题。此时两类的样本数目差距非常之大，明显边缘像素的重要性是比非边缘像素大的，此时可以针对性的对样本进行加权。

$$l(y,z)=-\sum_{k=0}^C w_ky_k\log (f(z_k))$$

## Triplet Loss

Triplet loss通常是在个体级别的细粒度识别上使用，传统的分类是花鸟狗的大类别的识别，但是有些需求是要精确到个体级别，比如精确到哪个人的人脸识别，所以triplet loss的最主要应用也就是face identification，person re-identification，vehicle re-identification的各种identification识别问题上。

当然你可以把每个人当做一个类别来进行分类训练，但是往往最后会造成softmax的维数远大于feature的维数。

论文：

《Deep feature learning with relative distance comparison for person re-identification》

![](/images/img2/Triplet_Loss.png)

如上图所示，triplet是一个三元组，这个三元组是这样构成的：从训练数据集中随机选一个样本，该样本称为Anchor，然后再随机选取一个和Anchor(记为$$x^a$$)属于同一类的样本和不同类的样本,这两个样本对应的称为Positive(记为$$x^p$$)和Negative(记为$$x^n$$)，由此构成一个（Anchor，Positive，Negative）三元组。

针对每个样本$$x_i$$，训练一个参数共享或者不共享的网络，得到三个元素的特征表达，分别记为：$$f(x_i^a), f(x_i^p), f(x_i^n)$$。

**triplet loss的目的就是通过学习（即上图中的Learning），让$$x^a$$和$$x^p$$特征表达之间的距离尽可能小，而$$x^a$$和$$x^n$$的特征表达之间的距离尽可能大。**

它的公式化的表示就是：

$$\|f(x_i^a)-f(x_i^p)\|_2^2 + \alpha < \|f(x_i^a)-f(x_i^n)\|_2^2$$

其中，$$\alpha$$表示两个距离之间的间隔。因此，对应的目标函数也就很清楚了：

$$\sum_i^N\left[\|f(x_i^a)-f(x_i^p)\|_2^2 - \|f(x_i^a)-f(x_i^n)\|_2^2 + \alpha \right]_+$$

这里距离用欧式距离度量，+表示[]内的值大于零的时候，取该值为损失，小于零的时候，损失为零。

>需要注意的是Triplet Loss以及后面介绍的各种改进版softmax，其收敛速度不如softmax，因此，先用softmax训练几轮，再改用这些loss，也是常用的调参技巧。

参考：

https://blog.csdn.net/u010167269/article/details/52027378

Triplet Loss、Coupled Cluster Loss探究

https://blog.csdn.net/tangwei2014/article/details/46788025

triplet loss原理以及梯度推导

https://www.zhihu.com/question/62486208

triplet loss在深度学习中主要应用在什么地方？有什么明显的优势？

https://mp.weixin.qq.com/s/XB9VsW3NRwHua6AdRL3n8w

Lossless Triplet Loss:一种高效的Siamese网络损失函数

## 参考

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

https://mp.weixin.qq.com/s/Ywzbn2_QqYd1W8dv8cxupw

Seesaw Loss：一种面向长尾目标检测的平衡损失函数

https://mp.weixin.qq.com/s/qJIlbLuM7--wj3fDLUecYw

Pytorch中的四种经典Loss源码解析

https://mp.weixin.qq.com/s/7Jg-YvS3nvcPJ-zYhK96EA

分享神经网络中设计loss function的一些技巧

https://mp.weixin.qq.com/s/cYcztl8N9JF-XXp9xLJIxg

一文道尽softmax loss及其变种

https://mp.weixin.qq.com/s/MTeuRYutMiCmthEAObyAIg

从最优化的角度看待Softmax损失函数

https://zhuanlan.zhihu.com/p/23340343

Center Loss及其在人脸识别中的应用

https://zhuanlan.zhihu.com/p/34404607

人脸识别的LOSS（上）

https://zhuanlan.zhihu.com/p/34436551

人脸识别的LOSS（下）

https://mp.weixin.qq.com/s/kI22wSoyNT3QXXI8pVwbjA

腾讯AI Lab提出新型损失函数LMCL：可显著增强人脸识别模型的判别能力

https://mp.weixin.qq.com/s/8KM7wUg_lnFBd0fIoczTHQ

用收缩损失(Shrinkage Loss)进行深度回归跟踪

https://mp.weixin.qq.com/s/piYyhPbA6kAXuSE5yHfQ1g

人脸识别损失函数综述

https://zhuanlan.zhihu.com/p/60747096

人脸识别损失函数简介与Pytorch实现：ArcFace、SphereFace、CosFace

https://mp.weixin.qq.com/s/_cVNNZBBJljdWBPU9d38CA

常见的损失函数超全总结

https://mp.weixin.qq.com/s/P6xLYrNP4pNKtHcxAWAOKg

数据竞赛中的各种loss function

https://mp.weixin.qq.com/s/Q4ryiTOSJQaJ2e5clmXjtg

一文看尽深度学习中的15种损失函数

# 俄乌战争=

整个俄乌战争给我的感觉就是俄罗斯简直晚明翻版。俄军就是辽东军翻版，整个军队山头林立，各有私心，军官吃空饷，后勤贪腐亏空，前线士兵无饷可发，补给又少，再加上一个多疑+刚愎的领袖，身边的所谓智囊也活脱脱的东林党翻版，简直齐活了！

---

这俄友捡了顾客信用卡，直接刷了五百块钱，在WOT氪金被条子抓了，交不起罚款，参加俄军结果被乌友活捉。

---

我发现保加利亚是真的坏，表面上跟鹅保持暧昧背地给乌萝送弹药就算了，居然还从鹅买低价油然后送给乌萝，在最多的时候乌萝百分之四十的柴油都是保加利亚提供。

---

大弟太看不起乌贼了，才动员30万够干啥啊，4000W人口的国家光男兵都可以至少1000W，况且女兵也征了不少，这波动员兵过去估计还是送。

---

联想到上次泽连老婆“疯狂购物”，还是去年12月“巴黎疯狂购物4万欧元”，可能穷俄宣自己事后也觉得，区区4万欧元对于他们宣称的“外国银行存款13亿美元”的泽连两口子，也太“皇帝金粪叉”了，所以这次直接涨到110万美元了。

https://www.zhihu.com/question/624768993

如何评价乌克兰总统泽连斯基第一夫人埃琳娜在纽约豪掷百万美元购买首饰？

---

2021年的时候在政权动荡，内战，经济停滞等积弊冲击下，除了极端民族主义者，乌克兰国民已经没什么人认同乌克兰这玩意儿了，一般乌友心态也是毁灭吧，这破国谁上台都是这吊样。如果京子哥有战略定力，再忍2年，乌克兰的权力中枢就自我崩溃了，到时候京子哥打款整一个亲俄派上台并不难，反对派平台——为了生活就在21年时候，在地方选举赢了不少城市。

戏子本来大概率会是一个历史不会记起的庸才，凭着不知道是愚蠢还是信念的神操作，硬接俄罗斯第一猛男——大弟的全力一击，还他妈的站住了，直接一举封神。

---

波兰总统说了个很意思的话，要那些试图让乌克兰割让土地的国家，把自己领土交给大毛，用来换克里米亚。皆大欢喜的建议啊！

---

将希望寄托在匈牙利和土鸡是不靠谱的，他们只想把京子卖个好价钱。
