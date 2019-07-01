---
layout: post
title:  深度学习（四十二）——行人重识别, 深度ISP, Spiking Neuron Networks
category: DL 
---

# 行人重识别

行人重识别（Person re-identification）也称行人再识别，是利用计算机视觉技术判断图像或者视频序列中是否存在特定行人的技术。广泛被认为是一个图像检索的子问题。给定一个监控行人图像，检索跨设备下的该行人图像。旨在弥补目前固定的摄像头的视觉局限，并可与行人检测/行人跟踪技术相结合，可广泛应用于智能视频监控、智能安保等领域。

参考：

https://github.com/gjy3035/Awesome-Crowd-Counting

人群计数最全代码、数据、论文合集

https://mp.weixin.qq.com/s/ZmX_ir1pSUZbCaFpbcQ6Lw

一文读懂行人检测算法

https://zhuanlan.zhihu.com/p/26168232

行人重识别：从哈利波特地图说起

https://zhuanlan.zhihu.com/p/50387521

从零开始行人重识别

https://mp.weixin.qq.com/s/_NDw7pFmDB07mliHTA6VYQ

旷视行人再识别（ReID）突破

https://zhuanlan.zhihu.com/p/31181247

从人脸识别到行人重识别，下一个风口

https://mp.weixin.qq.com/s/zRdJktyk1LZWUd2cyTjpiw

基于图像检索的行人重识别

https://zhuanlan.zhihu.com/p/31921944

基于深度学习的行人重识别研究综述

https://zhuanlan.zhihu.com/p/31473785

行人再识别中的迁移学习：图像风格转换

https://mp.weixin.qq.com/s/fX94rPgNHrOaQTqBv-ZADg

基于视频的行人再识别新进展：区域质量估计方法和高质量的数据集

https://mp.weixin.qq.com/s/rf-pGfkQFK3abkOLEEVOeA

PTGAN：针对行人重识别的生成对抗网络

https://zhuanlan.zhihu.com/p/34778414

基于时空模型无监督迁移学习的行人重识别

https://zhuanlan.zhihu.com/p/35296881

刷新三数据集纪录的跨镜追踪(行人再识别-ReID)技术介绍

https://mp.weixin.qq.com/s/ZbmJGO3lqwNM2z-E4_Mpbw

由“刷脸”到“识人”，云从科技刷新跨镜追踪(ReID)技术三项世界纪录！

https://zhuanlan.zhihu.com/p/38603624

云从科技资深算法研究员：详解跨镜追踪(ReID)技术实现及难点

https://mp.weixin.qq.com/s/leuILzYz40PqrwsCatYhPw

2018行人再识别年度进展

https://zhuanlan.zhihu.com/p/64004977

2019行人再识别年度进展回顾

https://zhuanlan.zhihu.com/p/37931822

你需要知道的10种行人属性

https://mp.weixin.qq.com/s/YBorhQrJ0UL3HZQHgd5D6A

清华等机构提出基于内部一致性的行人检索方法，实现当前最优

https://zhuanlan.zhihu.com/p/40514536

一个强力的ReID basemodel

https://mp.weixin.qq.com/s/mktVMZ0Fdo0mubstpl2GDA

Repulsion loss：专注于遮挡情况下的行人检测

https://mp.weixin.qq.com/s/LZCYx-VyOAMWXBS76ttkFw

如何在不同摄像头里识别行人？多层相似度感知CNN网络解析

https://mp.weixin.qq.com/s/iUPZ4DfL65_NV2v3eWomww

无监督深度关联学习大幅提高行人重识别性能

https://mp.weixin.qq.com/s/Wj6RBS7gYd9skE3AU1s4Qg

尺度不变提升人群计数性能

https://mp.weixin.qq.com/s/3eLL5Xg2mBFWnbYFbW486A

行人检测全新视角：从人体中轴线标注出发

https://mp.weixin.qq.com/s/VwejfjxjVnGFW3WXXyL1og

ALFNet：向高效行人检测迈进

https://mp.weixin.qq.com/s/3gzlln0yqqXQ6Xbwt-VXjw

OR-CNN行人检测：为‘遮挡’而生

https://mp.weixin.qq.com/s/yrygkZUwsCL-twTFvA--1w

尺度不变网络提升人群计数性能

https://mp.weixin.qq.com/s/K7JNqjuUfotTODtJU1W-YQ

快速精准的人头检测，代码已开源

https://mp.weixin.qq.com/s/DZmAWpptAVIqA_7L24ldHg

行人重识别PCB-RPP，SGGNN

https://mp.weixin.qq.com/s/e_n-BsPkrPd4MsyvlMVeYg

行人重识别告别辅助姿势信息，商汤、中科大提出姿势无关的特征提取GAN

https://mp.weixin.qq.com/s/l8ExvxOERUngHBqtep-ZWw

人群计数（Crowd Counting）研究综述

https://zhuanlan.zhihu.com/p/51511683

Graph Reid系列结合谱聚类做特征变换

https://zhuanlan.zhihu.com/p/47073533

结合时空的Re-ID系列：ECCV2018 TAUDL

https://mp.weixin.qq.com/s/eKHL0F3tnjEHY10d5PqkMg

最新Video-based ReID论文核心解读

https://mp.weixin.qq.com/s/iotqiyRrH4kwWmBvYi-tMQ

中科院&地平线开源state-of-the-art行人重识别算法EANet:增强跨域行人重识别中的部件对齐

https://zhuanlan.zhihu.com/p/54576174

行人重识别新任务：训练只需一张标注图片

https://zhuanlan.zhihu.com/p/52274204

AAAI2019 Spatial-Temporal Person Re-identification

https://zhuanlan.zhihu.com/p/55320029

行人跨模态重识别：双向限制的排序损失

https://zhuanlan.zhihu.com/p/55787893

亚马逊提出：用于人群计数的尺度感知注意力网络

https://mp.weixin.qq.com/s/U-ICoZQWatyJmkPcnXNRbA

最新最简易的迁移学习方法，人员再识别新模型

https://mp.weixin.qq.com/s/ajpxP3b5nw2AC393uBypvA

西工大开源拥挤人群数据集生成工具，大幅提升算法精度

https://mp.weixin.qq.com/s/BDgIf6foDGtCNc48JPqrcg

行人重识别算法优化技巧：Bags of Tricks and A Strong Baseline

https://mp.weixin.qq.com/s/FEJDrCvXcnhl5y7KR8EXKw

行人检测新思路：高级语义特征检测取得精度新突破

https://mp.weixin.qq.com/s/BYAmDulUKJLE-rb-Kh8Xmg

CSP行人检测：无锚点框的检测新思路

https://mp.weixin.qq.com/s/FodjDqc30XuT4fV3XPVy8g

一个更加强力的ReID Baseline

https://mp.weixin.qq.com/s/zUcLZ41mse7qRqZgXlMvvg

C3F：首个开源人群计数算法框架

https://mp.weixin.qq.com/s/51X7NIS1UEXJ1sqx54tuwg

NVIDIA/悉尼科技大学/澳洲国立大学新作：用GAN生成高质量行人图像，辅助行人重识别

https://mp.weixin.qq.com/s/JbZblzgLyHBQxP9AecLZbA

旷视研究院提出Re-ID新方法VPM，优化局部成像下行人再识别

https://zhuanlan.zhihu.com/p/69559437

基于自然语言的跨模态行人re-id的SOTA方法简述（上）

https://mp.weixin.qq.com/s/RBfHrvkZlx90RM1Ohd6m9g

一种行人重识别监督之下的纹理生成网络

# 深度ISP

## 数据集

### HDR+

HDR+是一个使用连拍摄影生成更好的图像的数据集。

官网：

http://hdrplusdata.org

参考：

https://zhuanlan.zhihu.com/p/34391353

机器感知Google推出HDR+连拍摄影数据集

### HDRNet

HDRNet是一个Image Enhancement方面的数据集。

官网：

https://groups.csail.mit.edu/graphics/hdrnet/

## 综述

论文：

《Unprocessing Images for Learned Raw Denoising》

这篇论文虽然不是综述，但有很多内容讲解ISP的流程。

## 参考

https://mp.weixin.qq.com/s/wA85XFQXeypuoqFnmN2P4g

降噪的新时代

https://mp.weixin.qq.com/s/919VEvennHEG3iXKkMZoQQ

不止是去噪---从去噪看AI ISP的趋势

https://mp.weixin.qq.com/s/1HA6XKnWpqVd8k7IIfzB7w

利用卷积自编码器对图片进行降噪

https://zhuanlan.zhihu.com/p/39512000

Noise2Noise：图像降噪，无需干净样本

https://mp.weixin.qq.com/s/_tvOQPvybqmvLF19kHcbFg

北大开源ECCV2018深度去雨算法：RESCAN

https://mp.weixin.qq.com/s/Wdxkvlz4nLbJS_gWqHwMjw

无需额外硬件，全卷积网络让机器学习学会夜视能力

https://mp.weixin.qq.com/s/iH7gbRn4opLsWgKWoVFpBA

腾讯优图&港科大提出较大前景运动下的深度高动态范围成像

https://mp.weixin.qq.com/s/WXVZkqCGlj6ym5YrSZS3Vg

谷歌普林斯顿提出首个端到端立体双目系统深度学习方案

https://mp.weixin.qq.com/s/NlYgA-A43q4C155kRdWPAQ

论文复现：谷歌实时端到端双目系统深度学习网络stereonet

https://mp.weixin.qq.com/s/9yfTO2jHz69-k1MsUGIM0Q

双目立体放大！谷歌刚刚开源的这篇论文可能会成为手机双摄的新玩法

https://mp.weixin.qq.com/s/z87Wp3yutq1l5bYfJS2YIA

谷歌新研究用深度学习合成运动模糊效果，手抖也能拍出摄影师级照片

https://mp.weixin.qq.com/s/B5XNmFlSnjEh2xAXB42pHQ

超十亿样本炼就的CNN助力图像质量增强，Adobe推出新功能“增强细节”

https://mp.weixin.qq.com/s/MEjZT_41w2cRqIYDi8a1rw

腾讯优图CVPR中标论文：不靠硬件靠算法，暗光拍照也清晰

https://mp.weixin.qq.com/s/Ek2eNeUPzEJiN6E1Yr6sMw

CVPR2019成像类论文拾英

# LSM

liquid state machine (LSM)

http://www.docin.com/p-390935406.html

基于液体状态机的脑运动神经系统的建模研究

# DNC

https://zhuanlan.zhihu.com/p/27773709

浅析至强RNN可微分神经计算机(DNC)

https://zhuanlan.zhihu.com/p/27964341

浅析至强RNN可微分神经计算机(DNC)-2

https://zhuanlan.zhihu.com/p/28209628

DNC-3滚动分类的模式识别

https://zhuanlan.zhihu.com/p/28433712

DNC4广义线性回归

# Spiking Neuron Networks

除了基于BP算法的NN之外，Spiking Neuron Networks也是一大类NN。Spiking NN和人脑结构更相似，功耗也更小，但是相关训练和数据量化的算法尚不成熟，属于潜力股。

![](/images/img2/BrainChip_Fig2.gif)

参考：

https://homepages.cwi.nl/~sbohte/publication/paugam_moisy_bohte_SNNChapter.pdf

Computing with Spiking Neuron Networks

https://mp.weixin.qq.com/s/6dpKSaLFVo-ge4gtbG8GQg

简述脉冲神经网络SNN：下一代神经网络

https://mp.weixin.qq.com/s/0n50YO1jIv_mxqe0EeS6kw

综述AI未来：神经科学启发的类脑计算

https://mp.weixin.qq.com/s/5KA7jtlRmnXxijGQhU1k4A

DeepMind哈萨比斯狂推的神经科学，入门需要看什么书？

https://mp.weixin.qq.com/s/TWdeHVCgEf54STvdA1QUPg

DeepMind哈萨比斯长文：伟大的AI离不开神经科学

https://mp.weixin.qq.com/s/8ibcyvyBLYArAMhQElqRzg

Cell研究揭示生物神经元强大新特性，是时候设计更复杂的神经网络了！

https://mp.weixin.qq.com/s/cb6JBlb11xW0Xw0RWI4vFA

浙大&川大提出脉冲版ResNet：继承ResNet优势，实现当前最佳

https://mp.weixin.qq.com/s/yaAuVpuhSGabOswKnv9q5Q

脉冲神经网络与小样本学习

https://mp.weixin.qq.com/s/m7UHX3XL5sC3oBdu3KoyOg

脉冲神经网络（SNN）概述

# 多模态学习

https://mp.weixin.qq.com/s/ruRkqBEdyj2Dx0WTO5Jhcw

多模态学习研究进展综述

https://mp.weixin.qq.com/s/vpBPkjuCebSWh5qPLYHCkw

上海交大提出多模态框架“EmotionMeter”，更精准地识别人类情绪

https://mp.weixin.qq.com/s/BBg04rDtiqU-XrWortufNA

康奈尔&英伟达提出多模态无监督图像转换新方法

http://mp.weixin.qq.com/s/khOINUyrNV3TFfgNRheH0A

卷积神经网络压缩、多模态的语义分析研究

https://mp.weixin.qq.com/s/ywU4L659iRcmIgmV6RtbXA

DeepMind新研究连接听与看，实现“听声辨位”的多模态学习

https://mp.weixin.qq.com/s/1qhcyTXttgKWlw-Oy556Tw

TPAMI2019最新《多模态机器学习综述》

https://mp.weixin.qq.com/s/BczgUuh2FIvP5MG9xh87wQ

多模态多任务学习新论文

https://mp.weixin.qq.com/s/ipj8qpYRiYbIeXn2PZb1SQ

5G时代下多模态理解做不到位注定要掉队

https://mp.weixin.qq.com/s/UghgWBN7mE8oJdMUvjAjcQ

何晖光：多模态情绪识别及跨被试迁移学习
