---
layout: post
title:  深度学习（十八）——数据增强, 深度信息检索, 手势识别, 语义分割
category: DL 
---

# 数据增强

https://mp.weixin.qq.com/s/GqPfvWwH1T0XFwiZ86cW8A

SamplePairing：针对图像处理领域的高效数据增强方式

https://mp.weixin.qq.com/s/cQtXvOjSXFc4YKn7ANBc_w

谷歌大脑提出自动数据增强方法AutoAugment：可迁移至不同数据集

https://mp.weixin.qq.com/s/ojFo7-gUh73iK3uImFS2-Q

一文道尽主流开源框架中的数据增强

https://mp.weixin.qq.com/s/xJhWu-1FyhIWbFBC5oHMkw

一文道尽深度学习中的数据增强方法（上）

https://mp.weixin.qq.com/s/OctAGrcBB0a6TOGWMmVKUw

深度学习中的数据增强（下）

https://mp.weixin.qq.com/s/lMU6_ywQqneyunqEV6uDiA

如何改善你的训练数据集？

https://mp.weixin.qq.com/s/ooX9Hj5ejO6po6Ghb4zOug

一文解读合成数据在机器学习技术下的表现

https://zhuanlan.zhihu.com/p/33485388

mixup与paring samples ，ICLR2018投稿论文的数据增广两种方式

https://mp.weixin.qq.com/s/_7xFBLPGT0VRTJ22toHJ3g

深度学习中常用的图像数据增强方法

https://mp.weixin.qq.com/s/sXV9epWguGbJEZYo4yNp5Q

如何正确使用样本扩充改进目标检测性能

https://zhuanlan.zhihu.com/p/46833956

图像数据增强之弹性形变（Elastic Distortions）

https://mp.weixin.qq.com/s/iaeHnfepyeLuOioHqMO9bQ

一种小目标检测中有效的数据增强方法

https://mp.weixin.qq.com/s/ws1R-VPyJY6J18OttBDYog

超少量数据训练神经网络：IEEE论文提出径向变换实现图像增强

https://mp.weixin.qq.com/s/g4022Rc1RNvr3IOC_bWuaQ

深度学习中的数据增强方法都有哪些？

https://mp.weixin.qq.com/s/YuFVEhO3wzCN5dIM_YqA7A

EDA：最简单的自然语言处理数据增广方法

https://mp.weixin.qq.com/s/IeqSfjt4x8HquXBeQN2gdQ

深度学习中的数据增强方法总结

# 深度信息检索

Information Retrieval是用户进行信息查询和获取的主要方式，是查找信息的方法和手段。狭义的信息检索仅指信息查询（Information Search）。即用户根据需要，采用一定的方法，借助检索工具，从信息集合中找出所需要信息的查找过程。广义的信息检索是信息按一定的方式进行加工、整理、组织并存储起来，再根据信息用户特定的需要将相关信息准确的查找出来的过程。

这方面的DL应用可参见以下的综述文章：

《MatchZoo: A Toolkit for Deep Text Matching》

## ARC-I & ARC-II

《Convolutional neural network architectures for matching natural language sentences》

## DSSM

《Learning deep structured semantic models for web search using clickthrough data》

## CDSSM

《Learning semantic representations using convolutional neural networks for web search》

## MV-LSTM

《A deep architecture for semantic matching with multiple positional sentence representations》

## CNTN

《Convolutional Neural Tensor Network Architecture for Community-Based Question Answering》

## DRMM

《A deep relevance matching model for ad-hoc retrieval》

## MatchPyramid

《Text Matching as Image Recognition》

## Match-SRNN

《Match-SRNN: Modeling the Recursive Matching Structure with Spatial RNN》

## K-NRM

《End-to-End Neural Ad-hoc Ranking with Kernel Pooling》

## 参考

https://github.com/harpribot/awesome-information-retrieval

信息检索优质资源汇总

https://mp.weixin.qq.com/s/5ba3EM6e9R-i3UpzUhm49w

神经信息检索导论，微软研究员129页最新书册

https://mp.weixin.qq.com/s/aZsj1FQnzHOr-YBcy_ljpw

DNN在搜索场景中的应用

https://mp.weixin.qq.com/s/1jgdI-Pt0PtN3oAs0Wh4XA

阿里提出电商搜索全局排序方法，淘宝无线主搜GMV提升5%

https://mp.weixin.qq.com/s/9Fcj5lO-JPfFVnRSSM_56w

深度学习在美团搜索广告排序的应用实践

https://mp.weixin.qq.com/s/wni3F9lKuO4OT32BVe0QDQ

谷歌发大招：搜索全面AI化，不用关键词就能轻松“撩书”

https://mp.weixin.qq.com/s/TrWwp-DBTrKqIT_Pfy_o5w

阿里妈妈首次公开新一代智能广告检索模型，重新定义传统搜索框架

https://mp.weixin.qq.com/s/fZv9FgbdQ1bWPoNdl9sF1A

“宝石迷阵”与信息检索

https://mp.weixin.qq.com/s/Vvo3Ti3XiGQz0IwLgATfWQ

电商搜索算法技术的演进

https://mp.weixin.qq.com/s/MpuUdZi8CWcu0b-ij-bHjA

Jeff Dean出品：用机器学习索引替代B-Trees，3倍性能提升，10-100倍空间缩小

https://mp.weixin.qq.com/s/uztYEW_azetOkOGiZcbCuw

JeffDean又用深度学习搞事情：这次要颠覆整个计算机系统结构设计。这篇blog介绍了如何用DL方法提高内存访问的命中率。

https://zhuanlan.zhihu.com/p/37020639

读论文系列：CVPR2018 SSAH

https://mp.weixin.qq.com/s/TdnstQaBcLaXg8BvuR7oYA

基于素描图的细粒度图像检索

https://mp.weixin.qq.com/s/N3JBHlqneG9dI0I26M3wHQ

如何做好大规模视觉搜索？eBay基于实践总结出了7条建议

https://mp.weixin.qq.com/s/8Twe3e3WKCY9pTiNtnW2sg

重磅！谷歌等推出基于机器学习的数据库SageDB

https://mp.weixin.qq.com/s/NJf5e25tvT_xKXLD7UY1AQ

MySQL智能调度系统。这篇blog其实和MySQL关系不大，算是DL在负载均衡方面的应用吧。

https://mp.weixin.qq.com/s/fzdK4YPTUgiW0D0aeH7WlQ

用于跨模态检索的综合距离保持自编码器

https://mp.weixin.qq.com/s/AWsiAYyVWY83s5uJ01Lg6Q

千亿级照片，毫秒间匹配最佳结果，微软开源Bing搜索背后的关键算法

https://mp.weixin.qq.com/s/fw5dRWmvZ17lqzxjKFrCtQ

相关性特征在图片搜索中的实践

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

https://mp.weixin.qq.com/s/DbvH6jM1VV47xKylbW-pug

掌纹识别近十年进展综述

https://mp.weixin.qq.com/s/mnPh8w3VuG9apprOkugbLA

中科大提出新型连续手语识别框架LS-HAN，帮助“听”懂听障人士

https://mp.weixin.qq.com/s/pUciYFjOKL3ea91fLCy0Yw

基于OpenCV与tensorflow实现实时手势识别

https://mp.weixin.qq.com/s/xtTmPtjCk4FQuQ3RnPZxEg

UC伯克利黑科技：用语音数据预测说话人手势

https://blog.csdn.net/wangyaninglm/article/details/87296595

指纹的对比分析系统概述

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

https://mp.weixin.qq.com/s/Nmr5oLe_MSLjYjWXUILiMw

视觉分割任务：论文与评测基准列表汇总

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

https://mp.weixin.qq.com/s/mQqEe4LC0VHBH2ZAtFanWQ

基于深度学习的图像语义分割方法回顾

https://mp.weixin.qq.com/s/9G3kahaoOSoB-DiGey1VLA

基于深度学习的图像语义分割算法综述

https://mp.weixin.qq.com/s/9F2UB_5ah1nEe3dfyoeRhg

图像分割算法综述

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
