---
layout: post
title:  深度加速（五）——模型压缩与加速（2）
category: DL acceleration 
---

# 模型压缩与加速

## 权值稀疏化实战（续）

一般采用稀疏化率来描述权值的稀疏化程度。每层的稀疏化率可以相同，也可以不同。前者被称作Magnitude Pruner，而后者被称作Sensitivity Pruner。

权值稀疏化的设置也和网络结构有关。比如分类网络，由于输入图片是高维数据，而分类结果是低维数据，因此在稀疏化处理的时候，**越靠近输出结果的Layer，其稀疏化程度就可以越高。**而最初的几层，即使只加少量稀疏化，也会导致精度的大幅下降，这时往往就不做或者少做稀疏化处理了。

上述方法的问题在于，分类网络的计算量主要集中在最初几层，所以这种triangle prune mode对于压缩计算量的效果一般。

除了训练后的权值稀疏化之外，权值稀疏化训练也是一种方法。

论文：

《FLOPs as a Direct Optimization Objective for Learning Sparse Neural Networks》

这篇论文，将计算量也就是FLOPs作为Loss function设计的一部分，由于稀疏化的权值没有运算量，因此，采用这种Loss训练出的网络，天生就是稀疏化的。

## AutoML

由于模型压缩，本质上是一个精益求精的优化问题，因此采用AutoML技术对于各个超参数进行优化，就成为了一件很有必要的事情。

这里主要的问题在于超参数数量众多，导致状态空间过大。

论文：

《AMC: AutoML for Model Compression and Acceleration on Mobile Devices》

这是韩松组的何宜晖的作品。该论文采用深度强化学习的DDPG网络来优化目标网络，从而大大减少了需要搜索的状态空间。

## EfficientNet

https://mp.weixin.qq.com/s/on1YdDexq5ICZL70mvikyw

谷歌大脑提出EfficientNet平衡模型扩展三个维度，取得精度-效率的最大化！

https://mp.weixin.qq.com/s/tCdG9gvpav1SvEzyAyBZXA

谷歌EfficientNet缩放模型，PyTorch实现出炉，登上GitHub热榜

https://mp.weixin.qq.com/s/NPM4E2gGOf3awQw7-_s6Uw

令人拍案叫绝的EfficientNet和EfficientDet

https://mp.weixin.qq.com/s/_eJ27nKULYzUNzDEf62x2w

何恺明团队最新力作RegNet：超越EfficientNet，GPU上提速5倍

## 参考

https://github.com/memoiry/Awesome-model-compression-and-acceleration

模型压缩与加速相关资源汇总

https://mp.weixin.qq.com/s/bndECrtEcNCkCF5EG0wO-A

移动端机器学习资源合集

https://blog.csdn.net/hw5226349/article/details/84888416

Deep Compression/Acceleration：模型压缩加速论文汇总

https://zhuanlan.zhihu.com/p/58805980

深度学习的模型加速与模型裁剪方法

https://mp.weixin.qq.com/s/pAEoVs8xu0SY9tfBqOJHHA

Google DeepMind最新报告—深度神经网络压缩进展

http://blog.csdn.net/shuzfan/article/details/51383809

神经网络压缩：Deep Compression

https://mp.weixin.qq.com/s/2NOFyu_twx1EciDeDPBLKw

深度神经网络加速与压缩

https://mp.weixin.qq.com/s/0KlnQ8UUxpyhBRdeo0EOAA

用于网络压缩的滤波器级别剪枝算法ThiNet

https://mp.weixin.qq.com/s/lO2UM04PfSM5VJYh6vINhw

为模型减减肥：谈谈移动／嵌入式端的深度学习

https://mp.weixin.qq.com/s/cIGuJvYr4lZW01TdINBJnA

深度压缩网络：较大程度减少了网络参数存储问题

https://mp.weixin.qq.com/s/1JwLP0FmV1AGJ65iDgLWQw

神经网络模型压缩技术

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

https://mp.weixin.qq.com/s/rTFLiZ7DCo6vzD5O64UnMQ

阿里提出新神经网络算法，压缩掉最后一个比特

https://mp.weixin.qq.com/s/f1SCK0J5oTWNJvtld3UAHQ

神经网络修剪最新研究进展

https://mp.weixin.qq.com/s/3oL0Bso3mwbsfaG8X5-xoA

英特尔提出新型压缩技术DeepThin，适合移动端设备深度神经网络

https://mp.weixin.qq.com/s/JnW7RnOQKG-dPOOAQeOmSA

当前深度神经网络模型压缩和加速都有哪些方法？

https://mp.weixin.qq.com/s/YUg2dZZhDSsRpSftdNfiIQ

极致的优化：智能手机是如何处理大型神经网络的

https://mp.weixin.qq.com/s/Ck_GDv1Xo-YMZcu-00gTOA

中星微夺冠国际人工智能算法竞赛，目标检测一步法精度速度双赢

https://mp.weixin.qq.com/s/qWJarPrjOrwxSX77xQ9rCw

面向卷积神经网络的卷积核冗余消除策略

https://mp.weixin.qq.com/s/QSGgvhkMUj3cXVlQwlzTFQ

深度神经网络加速和压缩新进展年度报告

https://zhuanlan.zhihu.com/p/37074222

CVPR 2018 高效小网络探密（上）

https://zhuanlan.zhihu.com/p/37919669

CVPR 2018 高效小网络探密（下）

https://zhuanlan.zhihu.com/p/38046989

从ISCA论文看AI硬件加速的新技巧

https://mp.weixin.qq.com/s/jqRBrs9Y_-3qvemL0RTflA

支付宝如何优化移动端深度学习引擎？

https://mp.weixin.qq.com/s/-V6hlZAKp1vuARSibZDBQQ

深度学习高效计算与处理器设计

https://mp.weixin.qq.com/s/NJzGR-tY_WWeccbdshHckA

基于交错组卷积的高效深度神经网络

https://mp.weixin.qq.com/s/ccFccLb2UTyFyMwFPjsDaA

让CNN跑得更快，腾讯优图提出全局和动态过滤器剪枝

https://mp.weixin.qq.com/s/vswtn3D1-VZZlyKLJmHc7A

纪荣嵘：深度神经网络压缩及应用

https://mp.weixin.qq.com/s/cSYCT1I1asaSCIc5Hgu0Jw

计算成本降低35倍！谷歌发布手机端自动设计神经网络MnasNet

https://zhuanlan.zhihu.com/p/42474017

MnasNet：终端轻量化模型新思路

https://mp.weixin.qq.com/s/p_qdKcQwQ8y_JUw3gQUEnA

谷歌大脑用强化学习为移动设备量身定做最好最快的CNN模型

https://mp.weixin.qq.com/s/OyEIcS5o6kWUu2UzuWZi3g

这么Deep且又轻量的Network，实时目标检测

https://mp.weixin.qq.com/s/mWfZ4jfuby4myGfi6TW3wQ

从超参数到架构，一文简述模型优化策略

https://mp.weixin.qq.com/s/8NDOf_8qxMMpcuXIZGJCGg

Google又发大招：高效实时实现视频目标检测

https://mp.weixin.qq.com/s/IxVMMu_7UL5zFsDCcYfzYA

AutoML自动模型压缩再升级，MIT韩松团队利用强化学习全面超越手工调参

https://mp.weixin.qq.com/s/fU-AeaPz-lHlg0CBgqnpZQ

轻量化神经网络综述

https://mp.weixin.qq.com/s/BMsvhXytSy2nWIsGOSOSBQ

自动生成高效DNN，适用于边缘设备的生成合成工具FermiNets

https://mp.weixin.qq.com/s/nEMvoiqImd0RxrskIH7c9A

仅17KB、一万个权重的微型风格迁移网络！

https://mp.weixin.qq.com/s/pc8fJx5StxnX9it2AVU5NA

基于手机系统的实时目标检测

https://mp.weixin.qq.com/s/6wzmyhIvUVeAN4Xjfhb1Yw

论文解读：Channel pruning for Accelerating Very Deep Neural Networks

https://mp.weixin.qq.com/s/-X7NYTzOzljzOaQL7_jOkw

惊呆了！速度高达15000fps的人脸检测算法！

https://mp.weixin.qq.com/s/6eyEMW9dVBR5cZrHxn8iqA

腾讯AI Lab详解3大热点：模型压缩、自动机器学习及最优化算法

https://xmfbit.github.io/2018/02/24/paper-ssl-dnn/

论文-Learning Structured Sparsity in Deep Neural Networks

https://mp.weixin.qq.com/s/d6HFVbbHwkxPGdnbyVuMyQ

密歇根州立大学提出NestDNN：动态分配多任务资源的移动端深度学习框架

https://mp.weixin.qq.com/s/lUTusig94Htf7_4Z3X1fTQ

清华&伯克利ICLR论文：重新思考6大剪枝方法

https://mp.weixin.qq.com/s/g3y9mRhkFtzSuSMAornnDQ

韩松博士论文：面向深度学习的高效方法与硬件

https://mp.weixin.qq.com/s/aH1zQ7we8OE59-O9n4IXhw

应对未来物联网大潮：如何在内存有限的情况下部署深度学习？

https://mp.weixin.qq.com/s/IfvXrsUq8-cBDC4_3O5v_w

Facebook新研究优化硬件浮点运算，强化AI模型运行速率

https://mp.weixin.qq.com/s/Jsxiha_BFtWVLvO4HMwJ3Q

工业界第一手实战经验：深度学习高效网络结构设计

https://mp.weixin.qq.com/s/uXbLb5ITHOU0dZRSWNobVg

算力限制场景下的目标检测实战浅谈

https://mp.weixin.qq.com/s/DoeoPGnS88HQmxagKJWLlg

小米开源FALSR算法：快速精确轻量级的超分辨率模型

https://mp.weixin.qq.com/s/wT39oUWfrQK-dg7hGXRynQ

实时单人姿态估计，在自己手机上就能实现

https://mp.weixin.qq.com/s/GJ7JMtWiKBku7dVJWOfLOA

CNN能同时兼顾速度与准确度吗？CMU提出AdaScale

https://mp.weixin.qq.com/s/pmel2k2J159zQi87ib3q8A

如何让CNN高效地在移动端运行

https://mp.weixin.qq.com/s/m-wQRm3VpfQkEOoUAxEdoA

论文解读: Quantized Convolutional Neural Networks for Mobile Devices

https://mp.weixin.qq.com/s/w7O2JxDH2ECqPn50sLfxpg

不用重新训练，直接将现有模型转换为MobileNet

https://mp.weixin.qq.com/s/EW6jvf98ifBucVz74SfSIA

文档扫描：深度神经网络在移动端的实践

https://mp.weixin.qq.com/s/FvR6loJ8KUxm7qwclestcQ

专门为卷积神经网络设计的训练方法：RePr

https://mp.weixin.qq.com/s/67GSnZnJySFrCESvmwhO9A

论文解读Channel pruning for Accelerating Very Deep Neural Networks

https://mp.weixin.qq.com/s/Lkxc_9sbRY157sMWaD5c7g

视频分割在移动端的算法进展综述

https://mp.weixin.qq.com/s/F0ykoKv027ycinsAZZjbWQ

ThunderNet：国防科大、旷视提出首个在ARM上实时运行的通用目标检测算法

https://mp.weixin.qq.com/s/J3ftOKDPBY5YYD4jkS5-aQ

ThunderNet：Two-stage形式的目标检测也可很快而且精度很高

https://mp.weixin.qq.com/s/ie2O5BPT-QxTRhK3S0Oa0Q

剪枝需有的放矢，快手&罗切斯特大学提出基于能耗建模的模型压缩

https://mp.weixin.qq.com/s/NsvjADgQZrYkUGNN6fzXVg

驭势科技推出“东风网络”：如何找到速度-精度的最优解？

https://mp.weixin.qq.com/s/HzgRHtVwdmW6_m7OJwK-ew

SysML 2019论文解读：Accurate and Efficient 2-Bit Quantized Neural Netowrks
