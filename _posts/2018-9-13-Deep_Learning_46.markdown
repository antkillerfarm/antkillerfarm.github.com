---
layout: post
title:  深度学习（四十六）——OCR, 目标检测进阶
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

https://mp.weixin.qq.com/s/0ysaJGNslckesv21o752FA

图像OCR年度进展

https://mp.weixin.qq.com/s/VIEsfc4qKAGsi-O9LD4mng

深度学习在OCR中的应用

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

# 目标检测进阶

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

https://mp.weixin.qq.com/s/e74-zFcMZzn67KaFXb_fdQ

CornerNet目标检测开启预测“边界框”到预测“点对”的新思路

https://zhuanlan.zhihu.com/p/41865617

CornerNet：目标检测算法新思路

https://mp.weixin.qq.com/s/e6B22xpue_xZwrXmIlZodw

ECCV-2018最佼佼者CornerNet的目标检测算法

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

# 功率谱（续）

功率谱密度的估计方法主要包括参数法和非参数法两大类。

参数法包括：

1.模型辨识法。基本就是上面提到的ARIMA或者其变种。

2.最小方差无失真响应法（MVDR）。

3.特征分解法。将相关矩阵R分解为两个子空间：信号子空间和噪声子空间。

非参数法包括：

1.周期图法。

2.多窗口法。

一般来说，随机过程的功率谱包含两个分量：确定性分量和连续分量。前者是增量过程$$\mathrm{d}Z(\omega)$$的一阶矩，后者是$$\mathrm{d}Z(\omega)$$的二阶中心矩。

参数法一般在知道相关物理规律时使用，它具有较高的精确度。而非参数法由于只依赖增量过程的一阶矩和二阶中心矩，因此适用范围更广泛，即使不知道系统的物理规律也可以使用。（有些类似万能拟合的GMM）

参考：

https://www.zhihu.com/question/29520851

功率谱密度如何理解？

# 高阶统计

上面讨论的基本都是一阶和二阶统计量，实际上我们还可以使用更高阶的统计量。使用高阶统计量的学科，一般被称为高阶统计学（higher-order statistics）。

## Moment

Moment（矩）的定义为：

$$\mu_n = \int_{-\infty}^\infty (x - c)^n\,f(x)\,\mathrm{d}x$$

其中，c=0时，被称作Raw Moment。c为均值时，被称作Central Moment。如果用$$\mu_n/\sigma^n$$替换$$\mu_n$$，就是所谓的Normalised Moment了。

1阶Raw Moment叫做Mean，2阶Central Moment叫做Variance，3阶Normalised Moment叫做Skewness，4阶Normalised Moment叫做kurtosis。

## Cumulants

Cumulants（累积量）的思想最早是Thorvald Thiele提出的，后来被Ronald Fisher和John Wishart发扬光大。

>Thorvald Nicolai Thiele，1838～1910，丹麦天文学家。哥本哈根大学博士。哥本哈根天文台台长（1978～1907）。曾研究过三体问题。被Ronald Fisher誉为“最伟大的统计学家”。

>John Wishart，1898～1956，苏格兰数学家和农业统计学家。Edinburgh University本科+Cambridge University硕士+University College London博士。导师是Karl Pearson，和Ronald Fisher也有过合作。Royal Society of Edinburgh会员。Cambridge University统计实验室首任主任。

>苏格兰人的自我意识真是强，足球有自己的协会，就连皇家学会也有自己的。




## Polyspectrum



## 参考

https://www.zhihu.com/question/25344430

随机变量的矩和高阶矩有什么实在的含义？

https://www.zhihu.com/question/43469699

信号的矩和高阶累积量的定义是什么？

# 张量分析

在同构的意义下，第零阶张量（r = 0）为标量（Scalar），第一阶张量（r = 1）为向量（Vector），第二阶张量（r = 2）则成为矩阵（Matrix）。

《张量分析》，黄克智著。

>注：黄克智，1927年生，固体力学家。江西中正大学本科+清华硕士+莫斯科大学博士（因应召回国，放弃博士学位）。清华大学工程力学系教授、工程力学研究所所长，中国科学院院士。断裂力学领域权威。

# 拓扑学

《Topopogy Without Tears》，University of New South Wales的Sidney A. Morris著。

该书的中文版：

http://www.topologywithouttears.net/topbookchinese.pdf

