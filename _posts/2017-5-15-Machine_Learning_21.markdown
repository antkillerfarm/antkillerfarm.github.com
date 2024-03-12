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

# 俄乌战争-

整个俄乌战争给我的感觉就是俄罗斯简直晚明翻版。俄军就是辽东军翻版，整个军队山头林立，各有私心，军官吃空饷，后勤贪腐亏空，前线士兵无饷可发，补给又少，再加上一个多疑+刚愎的领袖，身边的所谓智囊也活脱脱的东林党翻版，简直齐活了！

---

这俄友捡了顾客信用卡，直接刷了五百块钱，在WOT氪金被条子抓了，交不起罚款，参加俄军结果被乌友活捉。

---

波兰总统说了个很意思的话，要那些试图让乌克兰割让土地的国家，把自己领土交给大毛，用来换克里米亚。皆大欢喜的建议啊！

---

将希望寄托在匈牙利和土鸡是不靠谱的，他们只想把京子卖个好价钱。

---

苏联时代就不玩敌我识别啥的，防止击落友军主要靠分时分区域关闭防空系统，空军要事先交代来回的时间，防空部队发现机型时间对的上就不打，估计阿特木战事紧张飞的勤了，防空部队又害怕上次莫斯科事件重演，神经也紧张。。。

https://zhuanlan.zhihu.com/p/629169472

俄罗斯2架战机2架直升机被友军火力击毁

---

中国有句古话叫做“以己度人”，就是总喜欢用自己的思维替别人思考；其实全世界人都这样。比如美国人提到某地方，就想到“强制劳动”“摘棉花”，因为这个他们经验丰富；俄罗斯天天造谣“波兰瓜分乌克兰”，因为他们自己最喜欢抢人家土地。

---

莫托维利哈是俄罗斯唯一一家生产多种改进型多管火箭系统的系列制造商，生产（MLRS）“Grad”，“Smerch”和“Tornado”等火炮系统、冶金产品以及石油和天然气工业产品。

2022年10月，莫托维利哈工厂还是满负荷的生产状态，为了完成国防部的订单，生产线转为三班倒运转。与2021年相比，该厂2022年Tornado-G和Tornado-S MLRS的火箭炮产量显著增加，并且达到了过去十年中的最高产量。但多批订单交货之后，一直无法获得国防部的货款。莫托维利哈在今年7月资金链断裂申请破产。

俄罗斯银行甚至推出了“专门面向国防企业”的贷款，多么贴心，简直是特别军事行动的功臣！当然，为了犒赏功臣，大家应该不会介意2.5利率（战争开始之前是1.64％）的贷款吧...

驱动技术集团在俄罗斯将近两年的战时中，越来越多的货款因为客户收入下降而无法收回，比如俄罗斯天然气工业公司收入陡降，但必须上缴的税费却大幅度增加，导致拖欠了所有供应商不同比例的货款。驱动技术集团急需取得融资来维持公司的现金流，避免流动性枯竭导致的破产。

面对欧美不断升级的制裁，俄罗斯所有的科技公司都面临获取关键零部件和芯片的困难，在通过土耳其、中亚等地的灰色进口渠道越来越困难时，如何从友好国家获得替代性零部件和芯片，是驱动技术集团之类的俄罗斯企业生存与毁灭的致命问题。但一家民营企业在俄罗斯无法获得外交部、海关等执法机构的鼎立帮助，通过转让股份获得大公主的无限能量，就成了驱动技术集团的最优选项。

https://zhuanlan.zhihu.com/p/596858797

大锤落下——俄罗斯国防部“保护”下的军事工厂去哪了？

https://zhuanlan.zhihu.com/p/656081504

明天去哪里找炮弹？——俄罗斯军工业的世纪难题

https://zhuanlan.zhihu.com/p/654122291

完成了订单却拿不到钱，俄唯一的多管火箭制造商拍卖国防部债务

https://zhuanlan.zhihu.com/p/675582678

俄罗斯大公主为什么要收购俄石油和俄天然气承包商的股份？
