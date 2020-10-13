---
layout: post
title:  深度加速（六）——模型压缩与加速进阶
category: DL acceleration 
---

* toc
{:toc}

# 知识蒸馏（续）

http://blog.csdn.net/zhongshaoyy/article/details/53582048

蒸馏神经网络

https://mp.weixin.qq.com/s/QZ7PGvi27LiDOJaxici7Pw

数据蒸馏Dataset Distillation

https://mp.weixin.qq.com/s/vGTqHif48O2GZhuxWFhOLw

知识蒸馏总结、应用与扩展（2015-2019）

https://zhuanlan.zhihu.com/p/24337627

深度压缩之蒸馏模型

https://mp.weixin.qq.com/s/xcd9CHgE2_vEXrQ4MK019Q

知识蒸馏方法的演进历史综述

https://mp.weixin.qq.com/s/qE1makMUIaFNrWk4nqOxDw

最新《知识蒸馏》2020综述论文，30页pdf，悉尼大学

https://zhuanlan.zhihu.com/p/51563760

知识蒸馏（Knowledge Distillation）最新进展（一）

https://zhuanlan.zhihu.com/p/53864403

知识蒸馏（Knowledge Distillation）最新进展（二）

https://zhuanlan.zhihu.com/p/81467832

知识蒸馏（Knowledge Distillation）简述（一）

https://mp.weixin.qq.com/s/pXoENwz4Z-eok9y3P9rQvg

知识蒸馏（Knowledge Distillation）简述（二）

https://zhuanlan.zhihu.com/p/92166184

知识蒸馏简述（一）

https://zhuanlan.zhihu.com/p/92269636

知识蒸馏简述（二）

http://coderskychen.cn/2019/02/23/distilling/

知识蒸馏三部曲：从模型蒸馏、数据蒸馏到任务蒸馏

https://mp.weixin.qq.com/s/5_qgj33tyVTHivpXkU4LDw

一个知识蒸馏的简单介绍

https://zhuanlan.zhihu.com/p/93287223

从入门到放弃：深度学习中的模型蒸馏技术

https://mp.weixin.qq.com/s/mFuxCl0Mzv5hmDFewWZkrw

FAIR&MIT提出知识蒸馏新方法：数据集蒸馏

https://mp.weixin.qq.com/s/MDgqSwVEClNqNpaWuGTEpg

微软亚研院提出用于语义分割的结构化知识蒸馏

https://blog.csdn.net/xbinworld/article/details/83063726

知识蒸馏（Distilling the Knowledge in a Neural Network），在线蒸馏

https://mp.weixin.qq.com/s/ekKg46bQlWrlg9Hon01M5g

Hinton胶囊网络后最新研究：用“在线蒸馏”训练大规模分布式神经网络

https://mp.weixin.qq.com/s/SqxooZqSeD3wA4EFK5D3Kg

再生神经网络：利用知识蒸馏收敛到更优的模型

https://mp.weixin.qq.com/s/Nkxy0SUdbwIjp5swU6tS9g

深度互学习-Deep Mutual Learning：三人行必有我师

https://mp.weixin.qq.com/s/I08kMmUohWWbYVpPDgTJsw

Startdt AI提出：使用生成对抗网络用于One-Stage目标检测的知识蒸馏方法

https://mp.weixin.qq.com/s/yFyM5OVp1YLKQBlgXeAzhw

华为诺亚方舟实验室提出无需数据网络压缩技术

https://mp.weixin.qq.com/s/0f0aToVaAsU7yWK4xz-HzQ

剪枝量化初完结，蒸馏学习又上线

https://github.com/patrickwaters1000/DistillingNeuralNets

Implements the technique of distillation

https://mp.weixin.qq.com/s/ckn4RERri-mfqLVPDRHGog

让学生网络相互学习，为什么深度相互学习优于传统蒸馏模型？

https://mp.weixin.qq.com/s/wwtsqjjUGt7MTEWDc5bSvQ

一种无需原始训练数据的Teacher-Student模型压缩方法

https://mp.weixin.qq.com/s/9dHRO80mMTGdRHaa0AdihQ

无需数据集的Student Networks

https://mp.weixin.qq.com/s/fQAkNdNhwkFichSZCwnNqA

北大、华为联合提出无需数据集的Student Networks

https://mp.weixin.qq.com/s/fA5NWLvLQN6kbB563pJnKg

从16.6%到74.2%，谷歌新模型刷新ImageNet纪录（Noisy Student）

https://mp.weixin.qq.com/s/UPm02RtTwhQhP_YhtmheBg

面向视觉智能的知识蒸馏和Student-Teacher方法，附37页pdf下载

https://zhuanlan.zhihu.com/p/143155437

知识蒸馏在推荐系统的应用

https://mp.weixin.qq.com/s/OFCzl8stFU5b1MWrkDU7NA

阿里电商推荐中如何进行特征蒸馏提升模型效果

https://mp.weixin.qq.com/s/ZNjC30F28uX2lBkHBAAU3g

双DNN排序模型：在线知识蒸馏在爱奇艺推荐的实践

https://mp.weixin.qq.com/s/_Wq7qawac1nTfZnV_AKG6w

模型压缩中知识蒸馏技术原理及其发展现状和展望

https://mp.weixin.qq.com/s/W8mLxU48dgWBB4eEFnU2rQ

知识蒸馏经典解读

https://zhuanlan.zhihu.com/p/144982430

强化学习如何用于模型蒸馏？

https://zhuanlan.zhihu.com/p/144987182

模型蒸馏的核心技术点有哪些，如何对其进行长期深入学习

https://mp.weixin.qq.com/s/3zpri6pfVtp-3-5_004B1Q

优势特征蒸馏在淘宝推荐中的应用

https://zhuanlan.zhihu.com/p/163477538

知识蒸馏与推荐系统

https://mp.weixin.qq.com/s/TJVMuaDVZIjwqzuw6gd8uA

无数据知识蒸馏

# 模型压缩与加速进阶

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

https://mp.weixin.qq.com/s/cSYCT1I1asaSCIc5Hgu0Jw

计算成本降低35倍！谷歌发布手机端自动设计神经网络MnasNet

https://zhuanlan.zhihu.com/p/42474017

MnasNet：终端轻量化模型新思路

https://mp.weixin.qq.com/s/p_qdKcQwQ8y_JUw3gQUEnA

谷歌大脑用强化学习为移动设备量身定做最好最快的CNN模型

https://mp.weixin.qq.com/s/OyEIcS5o6kWUu2UzuWZi3g

这么Deep且又轻量的Network，实时目标检测

https://mp.weixin.qq.com/s/8NDOf_8qxMMpcuXIZGJCGg

Google又发大招：高效实时实现视频目标检测

https://mp.weixin.qq.com/s/IxVMMu_7UL5zFsDCcYfzYA

AutoML自动模型压缩再升级，MIT韩松团队利用强化学习全面超越手工调参

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

https://mp.weixin.qq.com/s/3oL0Bso3mwbsfaG8X5-xoA

英特尔提出新型压缩技术DeepThin，适合移动端设备深度神经网络

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

https://mp.weixin.qq.com/s/rzv8VCAxBQi0HsUcnLqqUA

处理移动端传感器时序数据的深度学习框架：DeepSense

https://mp.weixin.qq.com/s/UYk3YQmFW7-44RUojUqfGg

上交大ICCV：精度保证下的新型深度网络压缩框架

https://mp.weixin.qq.com/s/ZuEi32ZBSjruvtyUimBgxQ

揭秘支付宝中的深度学习引擎：xNN

https://mp.weixin.qq.com/s/0KlnQ8UUxpyhBRdeo0EOAA

用于网络压缩的滤波器级别剪枝算法ThiNet

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
