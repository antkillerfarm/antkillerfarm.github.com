---
layout: post
title:  深度学习（十七）——模型压缩, 人脸识别
category: theory 
---

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

https://mp.weixin.qq.com/s/b0dRvkMKSkq6ZPm3liiXxg

旷视科技提出新型卷积网络ShuffleNet，专为移动端设计

https://mp.weixin.qq.com/s/UYk3YQmFW7-44RUojUqfGg

上交大ICCV：精度保证下的新型深度网络压缩框架

https://mp.weixin.qq.com/s/f3bmtbCY5BfA4v3movwLVg

向手机端神经网络进发：MobileNet压缩指南

https://mp.weixin.qq.com/s/ZuEi32ZBSjruvtyUimBgxQ

揭秘支付宝中的深度学习引擎：xNN

http://mp.weixin.qq.com/s/iapih9Mme-VKCfsFCmO7hQ

简单聊聊压缩网络

# 人脸识别

## Cascade CNN

《A Convolutional Neural Network Cascade for Face Detection》

http://blog.csdn.net/shuzfan/article/details/50358809

人脸检测——CascadeCNN

## 参考

http://www.cnblogs.com/pandaroll/p/6590339.html

开源人脸识别openface

http://mp.weixin.qq.com/s/KQxGQdLa3XzKVIFYqlrV7g

人脸检测与识别的趋势和分析

https://zhuanlan.zhihu.com/p/25335957

人脸检测与深度学习

http://www.leiphone.com/news/201608/MPXlWtGaJLPYL7NB.html

人脸检测发展：从VJ到深度学习

https://mp.weixin.qq.com/s/ZZmLbFzi843g0k3gTncQmA

拿下人脸识别“世界杯”冠军！松下-NUS和美国东北大学实战分享

https://mp.weixin.qq.com/s/DYCXef_09yFFNR0uHL2Q0Q

基于Python的开源人脸识别库：离线识别率高达99.38%

blog.csdn.net/qq_14845119/article/details/52680940

MTCNN（Multi-task convolutional neural networks）人脸对齐

http://blog.csdn.net/shuzfan/article/details/52668935

人脸检测——MTCNN

https://mp.weixin.qq.com/s/eOxd5XbeyVcRYyAAmPr20Q

利用空间融合卷积神经网络通过面部关键点进行伪装人脸识别

https://mp.weixin.qq.com/s/vAjWHpn5HP_lwSv49g71SA

新型半参数变分自动编码器DeepCoder：可分层级编码人脸动作

https://mp.weixin.qq.com/s/IS5iAPZeUrvvyWs29O8Ukg

通过提取神经元知识实现人脸模型压缩

https://mp.weixin.qq.com/s/DEJ0z2CahZIrhTE3VXNVvg

基于注意力机制学习的人脸幻构

https://mp.weixin.qq.com/s/-G94Mj-8972i2HtEcIZDpA

人脸识别世界杯榜单出炉，微软百万名人识别竞赛冠军分享

https://mp.weixin.qq.com/s/bqWle_188lhYO4hpCfafkQ

用浏览器做人脸检测，竟然这么简单？

https://mp.weixin.qq.com/s/kn9JS55wIW2cfpUv7Jm0eQ

深度学习教你如何“以貌取人”！

https://mp.weixin.qq.com/s/3xEDtMoe0iRQSZiN5A1FGw

IPHONE X“刷脸”技术奥秘大揭底

https://mp.weixin.qq.com/s/s5HL6y2P9_KqpSAQg08URw

世界最大人脸对齐数据集ICCV 2017：距离解决人脸对齐已不远

https://mp.weixin.qq.com/s/Z06oUe7oExgkKZ8g2_PnPw

科普人脸表情识别技术

https://mp.weixin.qq.com/s/tS_9gS2ADoEdSKCLB-cxpA

刘小明教授为你讲解人脸识别

https://mp.weixin.qq.com/s/CLgyaAslE4hYHi62UhzeGw

韩琥：深度学习让机器给人脸“贴标签”

https://mp.weixin.qq.com/s/pUcVO8Fj-n9-pMwAzBvWuQ

山世光：人脸识别领域的“激荡20年”

https://zhuanlan.zhihu.com/p/23340343

Center Loss及其在人脸识别中的应用



