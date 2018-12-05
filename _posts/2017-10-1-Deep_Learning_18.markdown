---
layout: post
title:  深度学习（十八）——语义分割
category: DL 
---

# 语义分割

Semantic segmentation是图像理解的基石性技术，在自动驾驶系统（具体为街景识别与理解）、无人机应用（着陆点判断）以及穿戴式设备应用中举足轻重。

我们都知道，图像是由许多像素（Pixel）组成，而“语义分割”顾名思义就是将像素按照图像中表达语义含义的不同进行分组（Grouping）/分割（Segmentation）。

![](/images/article/image_enet.png)

上图是语义分割网络ENet的实际效果图。其中，左图为原始图像，右图为分割任务的真实标记（Ground truth）。

显然，在图像语义分割任务中，其输入为一张HxWx3的三通道彩色图像，输出则是对应的一个HxW矩阵，矩阵的每一个元素表明了原图中对应位置像素所表示的语义类别（Semantic label）。

因此，图像语义分割也称为“图像语义标注”（Image semantic labeling）、“像素语义标注”（Semantic pixel labeling）或“像素语义分组”（Semantic pixel grouping）。

由于图像语义分割不仅要识别出对象，还要标出每个对象的边界。因此，与分类目的不同，相关模型要具有像素级的密集预测能力。

目前用于语义分割研究的两个最重要数据集是PASCAL VOC和MSCOCO。

参考：

https://zhuanlan.zhihu.com/p/21824299

从特斯拉到计算机视觉之“图像语义分割”

https://zhuanlan.zhihu.com/SemanticSegmentation

一个语义分割的专栏

https://mp.weixin.qq.com/s/zZ-i54_wqzVQxTCFABNIMQ

闲聊图像分割这件事儿

https://zhuanlan.zhihu.com/p/22308032

图像语义分割之FCN和CRF

https://zhuanlan.zhihu.com/p/25515361

图像语义分割之特征整合和结构预测

https://zhuanlan.zhihu.com/p/27794982

语义分割中的深度学习方法全解：从FCN、SegNet到各代DeepLab

https://mp.weixin.qq.com/s/4BvvwV11f9MrrYyLwUrX9w

还在用ps抠图抠瞎眼？机器学习通用背景去除产品诞生记

https://mp.weixin.qq.com/s/mQqEe4LC0VHBH2ZAtFanWQ

基于深度学习的图像语义分割方法回顾

https://mp.weixin.qq.com/s/9G3kahaoOSoB-DiGey1VLA

基于深度学习的图像语义分割算法综述

https://mp.weixin.qq.com/s/JbdwtpA3iRXReyerO4HYIg

一文了解什么是语义分割及常用的语义分割方法有哪些

https://mp.weixin.qq.com/s/jCv259hI0vl7st80Obfrcg

图像语义分割的工作原理和CNN架构变迁

https://mp.weixin.qq.com/s/KcVKKsAyz-eVsyWR0Y812A

分割算法——可以分割一切目标

# 前DL时代的语义分割

从最简单的像素级别“阈值法”（Thresholding methods）、基于像素聚类的分割方法（Clustering-based segmentation methods）到“图划分”的分割方法（Graph partitioning segmentation methods），在DL“一统江湖”之前，图像语义分割方面的工作可谓“百花齐放”。在此，我们仅以“Normalized cut”和“Grab cut”这两个基于图划分的经典分割方法为例，介绍一下前DL时代语义分割方面的研究。

## Normalized cut

Normalized cut （N-cut）方法是基于图划分（Graph partitioning）的语义分割方法中最著名的方法之一，于2000年Jianbo Shi和Jitendra Malik发表于相关领域顶级期刊TPAMI。

通常，传统基于图划分的语义分割方法都是将图像抽象为图（Graph）的形式$$\bf{G}=(\bf{V},\bf{E})$$（$$\bf{V}$$为图节点，$$\bf{E}$$为图的边），然后借助图理论（Graph theory）中的理论和算法进行图像的语义分割。

常用的方法为经典的最小割算法（Min-cut algorithm）。不过，在边的权重计算时，经典min-cut算法只考虑了局部信息。如下图所示，以二分图为例（将$$\bf{G}$$分为不相交的$$\bf{A},\bf{B}$$两部分），若只考虑局部信息，那么分离出一个点显然是一个min-cut，因此图划分的结果便是类似$$n_1$$或$$n_2$$这样离群点，而从全局来看，实际想分成的组却是左右两大部分。

![](/images/article/N_cut.jpg)
