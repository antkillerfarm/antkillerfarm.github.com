---
layout: post
title:  深度学习（四十六）——OCR, 目标检测进阶, 细粒度分类, 元学习
category: DL 
---

# OCR

## 概述

光学字符识别（Optical Character Recognition, OCR），是指对文本资料的图像文件进行分析识别处理，获取文字及版面信息的过程。

华中科大白翔教授的实验室算是目前国内OCR做的比较好的了。

白翔的个人主页：

http://cloud.eic.hust.edu.cn:8071/~xbai/

该主页上有一个OCR方面的综述，是入门的最好资料。

http://www.cnblogs.com/lillylin/

这是一个OCR方面的blog。对白翔的论文，几乎都有阅读笔记。

## tesseract

linux下可以使用tesseract作为OCR工具。安装方法：

`sudo apt install tesseract-ocr libtesseract-dev`

使用方法：

`tesseract ./111.png 1 -l chi_sim+eng`

## CRNN

CRNN是白翔小组的作品。

论文：

《An End-to-End Trainable Neural Network for Image-based Sequence Recognition and Its Application to Scene Text Recognition》

代码：

https://github.com/bgshih/crnn

## ASTER

ASTER也是白翔小组的作品。

《ASTER: An Attentional Scene Text Recognizer with Flexible Rectification》

代码：

https://github.com/bgshih/aster

参考：

https://www.cnblogs.com/lillylin/p/9315180.html

论文阅读笔记

## 参考

https://mp.weixin.qq.com/s/h7HVyGbmtLmNVJp4p0rCRQ

字符识别(OCR)相关工具/库/教材/论文等资源整理

https://mp.weixin.qq.com/s/KFgC8zHWS7ysb9GfbkfLRA

OCR技术简介

https://mp.weixin.qq.com/s/0ysaJGNslckesv21o752FA

图像OCR年度进展

https://mp.weixin.qq.com/s/VIEsfc4qKAGsi-O9LD4mng

深度学习在OCR中的应用

https://blog.csdn.net/linolzhang/article/details/82780071

OCR文字识别

https://mp.weixin.qq.com/s/WmsHrTIMJhSt8MtFO7-yXA

证件全文本OCR技术，了解一下

https://zhuanlan.zhihu.com/p/21344595

端到端的OCR：验证码识别(LSTM+CTC)

http://www.jianshu.com/p/86489f1afd36

端到端的OCR：基于CNN的实现

http://www.jianshu.com/p/4fadf629895b

端到端的OCR：LSTM＋CTC的实现

https://mp.weixin.qq.com/s/axpA7Y_Rhiols5bDIdc6jg

Tesseract-OCR 3.0.1训练自己的语言库之图像文字识别

http://mp.weixin.qq.com/s/n8C80a3B54FhrCe-GhhcDA

文档扫描：深度神经网络在移动端的实践

https://mp.weixin.qq.com/s/MYhQt9uC16BadiZKWjPTzA

华中科技大学提出多向文本检测方法：基于角定位与区域分割

http://ilovin.me/2017-04-23/tensorflow-lstm-ctc-input-output/

tensorflow LSTM+CTC/warpCTC使用详解

https://mp.weixin.qq.com/s/k0dRu1wx49HTi_oJYJEGPw

阿里提出IncepText：全新多向场景文本检测模块

https://zhuanlan.zhihu.com/p/34757009

场景文字检测—CTPN原理与实现

https://mp.weixin.qq.com/s/h3VaKs0Pc44n-hXYNlkALA

开源OCR文字识别软件Calamari

https://mp.weixin.qq.com/s/ynpqG7Vfu5b8lYNW6Y-TpA

快准狠！Intel论文揭示自家车牌识别算法:LPRNet

https://mp.weixin.qq.com/s/FjoJA0gF4LgsB8hw24I0EQ

华科白翔老师团队ECCV2018 OCR论文：Mask TextSpotter

https://mp.weixin.qq.com/s/x-_fuVPgDYnle23IDsYCTw

百度深度学习图像识别决赛代码分享

https://mp.weixin.qq.com/s/r7GaYsdKLELXPmW5u2ysPw

OpenCV深度学习文本检测示例程序（EAST text detector）

https://mp.weixin.qq.com/s/Twki3TeNWwj_SqP9chXuxw

AdvancedEAST高效场景文本检测

https://mp.weixin.qq.com/s/PyV0Ml9ppTx0HZvz5VTe-Q

ICPR图像识别与检测挑战赛冠军方案出炉，基于偏旁部首来识别Duang字

https://mp.weixin.qq.com/s/gxpDyd5Lf0fmNZwFrJJZzg

OCR大突破：Facebook推出大规模图像文字检测识别系统——Rosetta

https://mp.weixin.qq.com/s/zeN-QYXBZzVtWUH10DXjVw

Facebook的新AI“Rosetta”会识别表情包，还会删帖

https://mp.weixin.qq.com/s/JIoTsadw4JBkr0e40RwVQQ

这篇论文开源的车牌识别系统打败了目前最先进的商业软件

https://mp.weixin.qq.com/s/lVRIy6DdwfRxdoVW3qFiNg

OCR如何读取皱巴巴的文件？深度学习在文档图像形变矫正的应用详解

https://mp.weixin.qq.com/s/-KdR3buxFOrPE-w-IMLQ9A

最强开源OCR！印刷体古籍文字识别超越著名商业软件ABBYY

# 目标检测进阶

## CornerNet

https://mp.weixin.qq.com/s/e74-zFcMZzn67KaFXb_fdQ

CornerNet目标检测开启预测“边界框”到预测“点对”的新思路

https://zhuanlan.zhihu.com/p/41865617

CornerNet：目标检测算法新思路

https://mp.weixin.qq.com/s/e6B22xpue_xZwrXmIlZodw

ECCV-2018最佼佼者CornerNet的目标检测算法

https://mp.weixin.qq.com/s/9ldLaYKGkgq-MnJZw7CrDQ

CornerNet为什么有别于其他目标检测领域的主流算法？

## 参考

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

https://zhuanlan.zhihu.com/p/41897137

物体检测中的结构推理网络

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

63页【深度CNN-目标检测】综述

# 细粒度分类

https://mp.weixin.qq.com/s/sdQ0rWbDDMN_P0B_RiYZmw

分段映射：帮助利用少量样本习得新类别细粒度分类器

https://mp.weixin.qq.com/s/zeN7rjmAnvh_7BbTmScrZw

细粒度分类你懂吗？——fine-gained image classification

https://mp.weixin.qq.com/s/LtWMGRBk2sbPDjeC9PmJ7g

弱监督学习下的商品识别：CVPR 2018细粒度识别挑战赛获胜方案简介

https://mp.weixin.qq.com/s/hcoAL1AHm_HtderWU8fSBw

大连理工大学在CVPR18大规模精细粒度物种识别竞赛中获得冠军

https://mp.weixin.qq.com/s/31r9FjuJn9yxrZMnfozkMQ

全卷积注意网络的细粒度识别

https://zhuanlan.zhihu.com/p/24738319

“见微知著”——细粒度图像分析进展综述

https://zhuanlan.zhihu.com/p/42067661

CVPR Look Closer to See Better

https://mp.weixin.qq.com/s/52hm3Cq3TFRnTMfDppivSQ

中山大学等提出HSE：基于层次语义嵌入模型的精细化物体分类

# 元学习

人工智能的历史显示了明确的进展方向：

第一代：**良好的老式人工智能**

>手工预测   
>什么都不学

第二代：**浅学习**

>手工功能   
>学习预测

第三代：**深度学习**

>手工算法（优化器，目标，架构......）   
>端到端地学习功能和预测

第四代：**元学习**

>无手工   
>端到端学习算法和功能以及预测

## 参考

https://mp.weixin.qq.com/s/KKK3VEpwL90g6Aro8qtXxQ

学习如何学习的算法：简述元学习研究方向现状

https://mp.weixin.qq.com/s/KtO3OTZ-bZ6m0ZSI6jTyjw

OpenAI提出Reptile：可扩展的元学习算法

https://mp.weixin.qq.com/s/T4GiL9vW7ALOzWloE_QQBA

OpenAI开发可拓展元学习算法Reptile，能快速学习

https://mp.weixin.qq.com/s/MWcoGsQJg1GBbSqzyPD9uQ

基于梯度的元学习算法，可高效适应非平稳环境

https://zhuanlan.zhihu.com/p/35695477

基于Meta Learning在动态竞争环境中实现策略自适应

https://mp.weixin.qq.com/s/AhadWUjtgsFmb8uTylTvqg

OpenAI提出新型元学习方法EPG，调整损失函数实现新任务上的快速训练

https://mp.weixin.qq.com/s/dmRdp2oMn0vGukclJSVZDg

Uber AI论文：利用反向传播训练可塑神经网络，生物启发的元学习范式

https://mp.weixin.qq.com/s/Cc4EHc6ei-PtZWhewM10xw

学习如何学习的算法：简述元学习研究方向现状

https://mp.weixin.qq.com/s/4f6-gXovdrYk7240TrUwJg

谷歌大脑：基于元学习的无监督学习更新规则

https://mp.weixin.qq.com/s/cAbMB-DB9vu2ua8t5J28ww

从零开始，了解元学习

https://mp.weixin.qq.com/s/Q36vpS1HF2IfeCsFLh656A

基于元强化学习的神经科学新理论

https://mp.weixin.qq.com/s/XtzvHOk7CdXRBy02kUmgsg

近期爆火的Meta Learning，遗传算法与深度学习的火花，再不了解你就out了

https://mp.weixin.qq.com/s/KvgYyuyICueNQPo_S27fEA

BAIR展示新型模仿学习，学会像人那样执行任务

https://zhuanlan.zhihu.com/p/41223529

最前沿:Meta RL论文解读

https://zhuanlan.zhihu.com/p/40600485

最前沿：Meta Learning前沿进展扫描

https://zhuanlan.zhihu.com/p/28639662

百家争鸣的Meta Learning/Learning to learn

https://zhuanlan.zhihu.com/p/45845001

最前沿：用模仿学习来学习增强学习

https://zhuanlan.zhihu.com/p/46059552

Meta Learning单排小教学

https://zhuanlan.zhihu.com/p/46131981

最前沿：Meta Learning在少样本文本翻译上的应用

https://zhuanlan.zhihu.com/p/46339823

谈谈无监督Meta Learning的研究

https://zhuanlan.zhihu.com/p/46340382

ICLR19最新论文解读之Meta Domain Adaptation

https://mp.weixin.qq.com/s/RBMGI20AI92ZcWSlYczqAA

伯克利、OpenAI等提出基于模型的元策略优化强化学习
