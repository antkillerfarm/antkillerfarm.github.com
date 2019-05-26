---
layout: post
title:  深度目标检测（六）——其它目标检测网络, 目标检测进阶
category: Deep Object Detection 
---

# 其它目标检测网络

## A-Fast-RCNN

A-Fast-RCNN首次将对抗学习引入到了目标检测领域，idea是非常创新的。

http://blog.csdn.net/jesse_mx/article/details/72955981

A-Fast-RCNN论文笔记

## FPN

FPN(Feature Pyramid Network)

参考：

https://mp.weixin.qq.com/s/mY_QHvKmJ0IH_Rpp2ic1ig

目标检测FPN

https://mp.weixin.qq.com/s/TelGG-uVQyxwQjiDGE1pqA

特征金字塔网络FPN

https://zhuanlan.zhihu.com/p/58603276

FPN-目标检测

## R-FCN

FCN在目标检测领域的应用。

http://blog.csdn.net/zijin0802034/article/details/53411041

R-FCN: Object Detection via Region-based Fully Convolutional Networks

https://blog.csdn.net/App_12062011/article/details/79737363

R-FCN

https://mp.weixin.qq.com/s/HPzQST8cq5lBhU3wnz7-cg

R-FCN每秒30帧实时检测3000类物体，马里兰大学Larry Davis组最新目标检测工作

https://mp.weixin.qq.com/s/AddHG_I00uaDov0le4vdvA

R-FCN和FPN

## G-CNN

G-CNN是MaryLand大学的工作，论文主要的思路也是消除region proposal，和YOLO，SSD不同，G-CNN的工作借鉴了迭代的想法，把边框检测等价于找到初始边框到最终目标的一个路径。但是使用one-step regression不能处理这个非线性的过程，所以作者采用迭代的方法逐步接近最终的目标。

http://blog.csdn.net/zijin0802034/article/details/53535647

G-CNN: an Iterative Grid Based Object Detector

## CornerNet

https://mp.weixin.qq.com/s/e74-zFcMZzn67KaFXb_fdQ

CornerNet目标检测开启预测“边界框”到预测“点对”的新思路

https://zhuanlan.zhihu.com/p/41865617

CornerNet：目标检测算法新思路

https://mp.weixin.qq.com/s/e6B22xpue_xZwrXmIlZodw

ECCV-2018最佼佼者CornerNet的目标检测算法

https://mp.weixin.qq.com/s/9ldLaYKGkgq-MnJZw7CrDQ

CornerNet为什么有别于其他目标检测领域的主流算法？

https://mp.weixin.qq.com/s/ZhfnZ4IwOnTQlqeB6Ilr3A

CornerNet: Detecting Objects as Paired Keypoints解读

https://zhuanlan.zhihu.com/p/63134919

普林斯顿大学提出：CornerNet-Lite，基于关键点的目标检测算法，已开源！

# 目标检测进阶

https://mp.weixin.qq.com/s/dcrBQ-t3tLOTouEyofOBxg

间谍卫星：利用卷积神经网络对卫星影像进行多尺度目标检测

https://mp.weixin.qq.com/s/LtXylKTKsHdjMPw9Q1HyXA

优于MobileNet、YOLOv2：移动设备上的实时目标检测系统Pelee

https://mp.weixin.qq.com/s/Gq3bflJq59Tx-nDCvbweNA

无需预训练分类器，清华&旷视提出专用于目标检测的骨干网络DetNet

https://mp.weixin.qq.com/s/u3eXhoFvo7vZujc0XoQQWQ

旷视研究院解读Light-Head R-CNN：平衡精准度和速度

https://mp.weixin.qq.com/s/6cUP9vvfcuv8rIEnGnAFiA

NCSU&阿里巴巴论文：可解释的R-CNN

https://mp.weixin.qq.com/s/1vOdOMyByBacSBMVrscq5Q

黄畅：基于DenesBox的目标检测在自动驾驶中的应用

https://mp.weixin.qq.com/s/-PeXMU_gkcT5YnMcLoaKag

CVPR清华大学研究，高效视觉目标检测框架RON

https://mp.weixin.qq.com/s/AupXIoVmhcOBrX1z1vgdtw

弱监督实现精确目标检测，上交大提出协同学习框架

https://mp.weixin.qq.com/s/Lt00ASVSb_fDDJdtCO0-tQ

物体检测中的结构推理网络

https://mp.weixin.qq.com/s/f0Ynln-27z5A6LXt8j5qKQ

据说以后在探头下面用帽子挡脸没用了：SymmNet遮挡物检测的对称卷积神经网络

https://mp.weixin.qq.com/s/cEg6HmS651riJVAtHdPafg

基于域适应弱监督学习的目标检测

https://mp.weixin.qq.com/s/PpT-NmTVjRi0_SEq0lISXw

旷视科技Oral论文解读：IoU-Net让目标检测用上定位置信度

https://mp.weixin.qq.com/s/tmp1HXU7cLerLr0DY9NluQ

杂乱环境下的显著性物体： 将显著性物体检测推向新高度

https://mp.weixin.qq.com/s/OqlZ2TRGbHURYW00440lgQ

微软亚洲研究院与北京大学共同提出用于物体检测的可学习区域特征提取模块

https://mp.weixin.qq.com/s/5I9uzGCNFD93L1mzakTl0Q

目标检测网络学习总结（RCNN-->YOLO V3）

https://mp.weixin.qq.com/s/ZQqcsJenqkXtH1czOe5WnA

阿里巴巴提出Auto-Context R-CNN算法，刷出Faster RCNN目标检测新高度

https://mp.weixin.qq.com/s/aLYQepnr_BjS27Fb-zoZ_g

迈向完全可学习的物体检测器：可学习区域特征提取方法

https://zhuanlan.zhihu.com/p/43655912

“别挡我，我要C位出道！”谈谈深度学习目标检测中的遮挡问题

https://mp.weixin.qq.com/s/VtlSVF4d9LwPJhDEYSbgTg

无监督难分样本挖掘改进目标检测

https://mp.weixin.qq.com/s/h_ENriEXr7WI_XR_DtxpMQ

这样可以更精确的目标检测——超网络

https://mp.weixin.qq.com/s?__biz=MzI5MDUyMDIxNA==&mid=2247486104&idx=1&sn=5580a4680f3190adb98638471e9b5982

百度视觉团队斩获 ECCV Google AI 目标检测竞赛冠军，获奖方案全解读

https://mp.weixin.qq.com/s/nL9l7hvG3RG7G7LzCzzvug

旷视科技2018 COCO负责人俞刚：如何构建检测与分割的冠军系统

https://mp.weixin.qq.com/s/zeruKQOye_QNWgluVIN0BA

从R-CNN到RFBNet，目标检测架构5年演进全盘点

https://mp.weixin.qq.com/s/sCGNUI-mUSYxD69uBDQNoQ

基于深度学习的目标检测算法综述：算法改进

https://mp.weixin.qq.com/s/XdH54ImSfgadCoISmVyyVg

基于单目摄像头的物体检测

https://mp.weixin.qq.com/s/yswy7VwEapQJ9M5n_Uo93w

目标检测最新进展总结与展望

https://mp.weixin.qq.com/s/s1qmCA8djEEanwCxeLSV2Q

63页《深度CNN-目标检测》综述

https://mp.weixin.qq.com/s/XoKdsQKyaI3LsDxF7uyKuQ

聊聊目标检测中的多尺度检测（Multi-Scale），从YOLO到FPN，SNIPER，SSD填坑贴和极大极小目标识别

https://mp.weixin.qq.com/s/iW-k12CIO0gSx8Y6etTzgA

三分支网络——目前目标检测性能最佳网络框架

https://mp.weixin.qq.com/s/JN1N-IqIL4tAh4rIkZcxpg

Grid R-CNN解读：商汤最新目标检测算法

https://mp.weixin.qq.com/s/baPfFVi7deEsCAFu3ColoQ

CVPR2018目标检测算法总览

https://mp.weixin.qq.com/s/t5p1xGKVnwd7wbiOzucFqQ

基于深度学习的目标检测算法剖析与实现

https://mp.weixin.qq.com/s/-zQZjHVs7bYyGkGuMUf3qg

目标检测领域还有什么可做的？19个方向给你建议

https://zhuanlan.zhihu.com/p/54334986

TridentNet：处理目标检测中尺度变化新思路

https://mp.weixin.qq.com/s/S6sz5dPgGNcJvrIAZ3ZjGg

基于深度学习的通用物体检测算法对比探索

https://mp.weixin.qq.com/s/oF3MAkl1UikRkOhrj3equw

深度学习的目标检测算法是如何解决尺度问题的？

https://mp.weixin.qq.com/s/oxStDMh90jB7_EY4vqja2w

目标检测论文阅读：DetNet

https://zhuanlan.zhihu.com/p/55972055

SimpleDet: 一套简单通用的目标检测与物体识别框架

https://mp.weixin.qq.com/s/Sl958JkcJjy-HW9_c-SH4g

Guided Anchoring: 物体检测器也能自己学Anchor

https://mp.weixin.qq.com/s/uzG8sic5Y6LVqBS6iKQDhw

目标检测中图像增强，mixup如何操作？

https://mp.weixin.qq.com/s/pkFcmm15gnuRJtngFX7f0w

目标检测训练trick超级大礼包—不改模型提升精度，值得拥有

https://mp.weixin.qq.com/s/flXzhQ-Ypf3fwTqLelLzOQ

李沐等将目标检测绝对精度提升5%，不牺牲推理速度

https://mp.weixin.qq.com/s/6QsyYtEVjavoLfU_lQF1pw

目标检测新文：Generalized Intersection over Union

https://mp.weixin.qq.com/s/Xs3nThAcUOq62bO2p61YFA

论文解读 Receptive Field Block Net for Accurate and Fast

https://mp.weixin.qq.com/s/-G47vOGx2iNQCarYRAiNPg

基于区域分解集成的目标检测

https://mp.weixin.qq.com/s/rlmgN0LbUfd2n9MI8OMT2w

性能大幅度提升（速度&遮挡）:基于区域分解&集成的目标检测

https://zhuanlan.zhihu.com/p/59398728

CVPR2019目标检测方法进展综述

https://mp.weixin.qq.com/s/NWILStthG4klkwrYVcGQSQ

ILC：用于自然场景多目标的计数模型

https://mp.weixin.qq.com/s/9BCf0rCp660a5xQ2JNz3AQ

深入理解one-stage目标检测算法（上篇）

https://mp.weixin.qq.com/s/p9XaI8PSG0o1NWlkmCIn7w

深入理解one-stage目标检测算法（下篇）

https://mp.weixin.qq.com/s/reNCLvOyJHkJZimnMpbIig

目标检测算法优化技巧：Bag of Freebies for Training Object Detection

https://mp.weixin.qq.com/s/psXJNlEawZlQ-ZdktDpjOw

目标检测小tricks之样本不均衡处理

https://mp.weixin.qq.com/s/Pl8HABuVN27CZv-lvGROTw

基于深度学习的目标检测算法近5年发展历史

https://mp.weixin.qq.com/s/apLEAMshqd3O8nU8Q0Wycg

李祥泰：Context modeling in semantic segmentation

https://mp.weixin.qq.com/s/svqygu4nFkW4ci7dYMnKsw

小目标检测的数据增广秘籍

https://mp.weixin.qq.com/s/bzgMWR2kzAI9NeXEY92GmA

目标检测任务的优化策略tricks

https://mp.weixin.qq.com/s/0_ap6CsBlz4pvx21c57-ag

旷视研究院提出新型损失函数：改善边界框模糊问题

https://mp.weixin.qq.com/s/wR5jUHBJHWFm8UTsobKDqw

目标检测中的Consistent Optimization

https://mp.weixin.qq.com/s/j-arl6qiD6mei4crfQPrgw

《深度学习显著目标检测综述》

https://mp.weixin.qq.com/s/ts4WFnuN4cHLfUh8--96Kw

Libra R-CNN：全面平衡的目标检测器

https://mp.weixin.qq.com/s/groq55Cbts272k1mfhJwaQ

超越bounding box的代表性点集：视觉物体表示的新方法

https://mp.weixin.qq.com/s/BXwL33qOf3f7BtJvHsi23Q

目标检测：Segmentation is All You Need？

https://mp.weixin.qq.com/s/2PLp2xNfhkHB3fPQr5Ts6g

密歇根大学40页《20年目标检测综述》最新论文，带你全面了解目标检测方法

https://mp.weixin.qq.com/s/HmUhlw90b2aTsoEwBdYbdQ

目标检测二十年技术综述

https://www.zhihu.com/question/270143544

目标检测中，不同物体之间的距离非常接近如何解决？

https://mp.weixin.qq.com/s/b4s8Te29DyS71xwQU789pQ

实体零售场景下密集商品的精确探测
