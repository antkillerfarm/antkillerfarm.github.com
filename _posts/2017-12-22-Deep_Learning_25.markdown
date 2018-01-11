---
layout: post
title:  深度学习（二十五）——深度推荐系统, 模型压缩, 手势识别
category: DL 
---

# 深度推荐系统

推荐系统一直是AI能够落地且商业前景很好的一个研究方向。自2016年以来，该方向也逐渐被DL所侵蚀，尽管目前从招聘来说，这方面的职位仍以普通ML为主。

2017年5月，我曾面试了一家电商企业。当时给我的感觉，虽然里面的工程师较早接触ML，然而知识老化现象比较严重，对最基本的神经网络知识缺乏必要的了解。这显然给了后来者一个弯道超车的好机会。

深度推荐系统的算法包括：

https://mp.weixin.qq.com/s/qwDIvXlpP5UIBTwtpqhYsg

Auto-Encoder

https://mp.weixin.qq.com/s/AqgxnfR4h1FBRmmEe6uPqQ

CDL

https://mp.weixin.qq.com/s/WlgUVf1EjpO9UGqjTJN5ww

UWRL

https://mp.weixin.qq.com/s/KII9oNg7kqfco2MngUOGAw

AutoRec

https://mp.weixin.qq.com/s/mnGuPGtdw9d1BzeNpoYYqw

DeepCoNN

https://mp.weixin.qq.com/s/lJDiP7oeiFQSEyxWt_9uBA

NFM

https://mp.weixin.qq.com/s/G4bDj4a05K0kB4IZ6IosiQ

Wide & Deep

https://mp.weixin.qq.com/s/JNGKz4-fWG4ygl7f6UkxcQ

DeepFM

## 工具

https://www.librec.net/

这是一个Java写的推荐系统。

## 参考

https://zhuanlan.zhihu.com/p/26237106

深度学习在推荐算法上的应用进展

http://i.dataguru.cn/mportal.php?mod=view&aid=11463

深度学习在推荐领域的应用

https://mp.weixin.qq.com/s/hGvQvddD3i858XSK4z08Ug

主要推荐系统算法总结及Youtube深度学习推荐算法实例概括

https://mp.weixin.qq.com/s/yHtqWJUpCIvTStKW5TINaA

Youtube短视频推荐系统变迁：从机器学习到深度学习

https://mp.weixin.qq.com/s/N1oLs-saWN_ifkWEaWw_Vg

YouTube 2016年公布的基于深度学习的推荐算法

https://mp.weixin.qq.com/s/WzSO_XobY6kesDm4sF-hBg

深度学习之推荐篇

https://mp.weixin.qq.com/s/LKjVfhyhL4GVx6l5WC6-CQ

如何用深度学习实现用户行为预测与推荐

https://mp.weixin.qq.com/s/UrMsMHAkqNHJEl5lhAvLtA

腾讯提出并行贝叶斯在线深度学习框架PBODL：预测广告系统的点击率

http://mp.weixin.qq.com/s/Jiis7j3W3D5GG_ZdxplY7Q

淘宝搜索/推荐系统背后深度强化学习与自适应在线学习的实践之路

https://mp.weixin.qq.com/s/847h4ITQMtUlZcurJ9Vlvg

深度学习在美团点评推荐平台排序中的运用

https://mp.weixin.qq.com/s/AICgNDyWASx_B8NzWcFTqA

一文综述所有用于推荐系统的深度学习方法

https://mp.weixin.qq.com/s/zSBpqhoyROh74UZEItBanA

基于概率隐层模型的购物搭配推送：阿里巴巴提出新型用户偏好预测模型

http://mp.weixin.qq.com/s/nmLNKscP1qxyv_aoSrwEEw

基于大规模图计算的本地算法对展示广告的行为预测

https://mp.weixin.qq.com/s/8hNkntUauCSeVqc2v0QUqA

人工智能如何帮你找到好歌：探秘Spotify神奇的每周歌单

https://zhuanlan.zhihu.com/p/30720579

推荐中的序列化建模：Session-based neural recommendation

https://mp.weixin.qq.com/s/vpxLTcwenvlIvj5D-8uolg

一天造出10亿个淘宝首页，阿里工程师如何实现？

https://mp.weixin.qq.com/s/lZ4FOOVIxsdKvfW45CYCnA

你看到哪版电影海报，由算法决定：揭秘Netflix个性化推荐系统

http://www.cnblogs.com/qcloud1001/p/7483362.html

深度学习在CTR中应用

http://www.cnblogs.com/qcloud1001/p/7513982.html

常见计算广告点击率预估算法总结

https://mp.weixin.qq.com/s/Q8Mt9B1rzbeWXqIInrzSYQ

使用深度学习构建先进推荐系统：近期33篇重要研究概述

https://mp.weixin.qq.com/s/PlsFxKz_Igorh94Ni-78Hg

融合MF和RNN的电影推荐系统

https://mp.weixin.qq.com/s/JKMOhpLWWlrDzymDDEldXw

深度学习大行其道，个性化推荐如何与时俱进？

https://mp.weixin.qq.com/s/FBzd0x4_A9z-r0f3ZKFGuw

携程个性化推荐算法实践

# 模型压缩

对于AI应用端而言，由于设备普遍没有模型训练端的性能那么给力，因此如何压缩模型，节省计算的时间和空间就成为一个重要的课题。

此外，对于一些较大的模型（如VGG），即使机器再给力，单位时间内能处理的图像数量，往往也无法达到实际应用的要求。这点在自动驾驶和视频处理领域显得尤为突出。

这里首先提到的是韩松的两篇论文：

《Deep Compression: Compressing Deep Neural Networks with Pruning, Trained Quantization and Huffman Coding》

《Learning both Weights and Connections for Efficient Neural Networks》

>韩松，清华本科（2012）+Stanford博士（2017）。MIT AP（from 2018）。   
>个人主页：   
>https://stanford.edu/~songhan/

韩松也是SqueezeNet的二作。

![](/images/article/nn_compression.png)

韩松论文的中心思想如上图所示。简单来说，就是去掉原有模型的一些不重要的参数、结点和层。

参数的选择，相对比较简单。参数的绝对值越接近零，它对结果的贡献就越小。这一点和稀疏矩阵有些类似。

结点和层的选择，相对麻烦一些，需要通过算法得到不重要的层。

比如可以逐个将每一层50%的参数置零，查看模型性能。对性能影响不大的层就是不重要的。

虽然这些参数、结点和层相对不重要，但是去掉之后，仍然会对准确度有所影响。这时可以对精简之后的模型，用训练样本进行re-train，通过残差对模型进行一定程度的修正，以提高准确度。

其次还可以看看图森科技的论文：

https://www.zhihu.com/question/62068158

如何评价图森科技连发的三篇关于深度模型压缩的文章？

图森的思路比较有意思。其中的方法之一，是利用L1规则化会导致结果的稀疏化的特性，制造出一批接近0的参数。从而达到去除不重要的参数的目的。

除此之外，矩阵量化、Kronecker内积、霍夫曼编码、模型剪枝等也是常见的模型压缩方法。

当然最系统的做法还属Geoffrey Hinton的论文：

《Distilling the Knowledge in a Neural Network》

图森科技的后两篇论文也是在Hinton论文的基础上改进的。

论文：

《Articulatory and Spectrum Features Integration using Generalized Distillation Framework》

参考：

https://zhuanlan.zhihu.com/p/24337627

深度压缩之蒸馏模型

http://blog.csdn.net/shuzfan/article/details/51383809

神经网络压缩：Deep Compression

https://zhuanlan.zhihu.com/p/24894102

《Distilling the Knowledge in a Neural Network》阅读笔记

https://luofanghao.github.io/2016/07/20/%E8%AE%BA%E6%96%87%E7%AC%94%E8%AE%B0%20%E3%80%8ADistilling%20the%20Knowledge%20in%20a%20Neural%20Network%E3%80%8B/

论文笔记 《Distilling the Knowledge in a Neural Network》

http://blog.csdn.net/zhongshaoyy/article/details/53582048

蒸馏神经网络

https://www.zhihu.com/question/50519680

如何理解soft target这一做法？

https://mp.weixin.qq.com/s/0KlnQ8UUxpyhBRdeo0EOAA

用于网络压缩的滤波器级别剪枝算法ThiNet

https://mp.weixin.qq.com/s/lO2UM04PfSM5VJYh6vINhw

为模型减减肥：谈谈移动／嵌入式端的深度学习

https://mp.weixin.qq.com/s/cIGuJvYr4lZW01TdINBJnA

深度压缩网络：较大程度减少了网络参数存储问题

https://mp.weixin.qq.com/s/1JwLP0FmV1AGJ65iDgLWQw

神经网络模型压缩技术

https://mp.weixin.qq.com/s/Xqc4UgcfCUWYOeGhjNpidA

CNN模型压缩与加速算法综述

https://mp.weixin.qq.com/s/rzv8VCAxBQi0HsUcnLqqUA

处理移动端传感器时序数据的深度学习框架：DeepSense

https://mp.weixin.qq.com/s/UYk3YQmFW7-44RUojUqfGg

上交大ICCV：精度保证下的新型深度网络压缩框架

https://mp.weixin.qq.com/s/ZuEi32ZBSjruvtyUimBgxQ

揭秘支付宝中的深度学习引擎：xNN

http://mp.weixin.qq.com/s/iapih9Mme-VKCfsFCmO7hQ

简单聊聊压缩网络

https://mp.weixin.qq.com/s/3qstz-KoRuxwpmfE4XDI-Q

面向卷积神经网络的卷积核冗余消除策略

https://mp.weixin.qq.com/s/dEdWz4bovmk65fwLknHBhg

韩松毕业论文：面向深度学习的高效方法与硬件

https://mp.weixin.qq.com/s/GFE2XYHZXPP0doQ5nd0JNQ

当前深度神经网络模型压缩和加速方法速览

https://mp.weixin.qq.com/s/Faej1LKqurtwEIreUVJ0cw

普林斯顿新算法自动生成高性能神经网络，同时超高效压缩

https://mp.weixin.qq.com/s/uK-HasmiavM3jv6hNRY11A

深度梯度压缩：降低分布式训练的通信带宽

https://mp.weixin.qq.com/s/_MDbbGzDOGHk5TBgbu_-oA

中大商汤等提出深度网络加速新方法，具有强大兼容能力

https://mp.weixin.qq.com/s/gbOmpP7XO1Hz_ld4iSEsrw

三星提出移动端神经网络模型加速框架DeepRebirth

# 手势识别

https://zhuanlan.zhihu.com/p/26630215

浅谈手势识别在直播中的运用

https://zhuanlan.zhihu.com/p/30561160

2017-最全手势识别/跟踪相关资源大列表分享

http://www.sohu.com/a/203306961_465975

浙江大学CSPS最佳论文：使用卷积神经网络的多普勒雷达手势识别

https://www.zhihu.com/question/20131478

我打算只根据手的形状来识别手势。用哪种机器学习算法比较好？

https://www.leiphone.com/news/201502/QM7LdSN874dWXFLo.html

带你了解世界最先进的手势识别技术

# NN的INT8计算

## 概述

NN的INT8计算是近来NN计算优化的方向之一。相比于传统的浮点计算，整数计算无疑速度更快，而NN由于自身特性，对单点计算的精确度要求不高，且损失的精度还可以通过retrain的方式恢复大部分，因此通常的科学计算的硬件（没错就是指的GPU）并不太适合NN运算，尤其是NN Inference。

>传统的GPU并不适合NN运算，因此Nvidia也好，还是其他GPU厂商也好，通常都在GPU中又集成了NN加速的硬件，因此虽然商品名还是叫做GPU，但是工作原理已经有别于传统的GPU了。

这方面的文章以Xilinx的白皮书较为经典：

https://china.xilinx.com/support/documentation/white_papers/c_wp486-deep-learning-int8.pdf

利用Xilinx器件的INT8优化开展深度学习

论文：

《On the efficient representation and execution of deep acoustic models》

![](/images/img2/INT8.png)

《Quantization and Training of Neural Networks for Efficient Integer-Arithmetic-Only Inference》

![](/images/img2/INT8_2.png)

参考：

https://www.chiphell.com/thread-1620755-1-1.html

新Titan X的INT8计算到底是什么鬼

https://mp.weixin.qq.com/s/S9VcoS_59nbZWe_P3ye2Tw

减少模型半数内存用量：百度&英伟达提出混合精度训练法

## NN硬件的指标术语

MACC：multiply-accumulate，乘法累加。

FLOPS：Floating-point Operations Per Second，每秒所执行的浮点运算次数。

显然NN的INT8计算主要以MACC为单位。

