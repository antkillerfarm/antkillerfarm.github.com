---
layout: post
title:  深度学习（四十六）——OCR
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

# 功率谱（续）

我们定义：

$$K_N(\omega)=\sum_{n=-N}^N e^{j\omega n}=\frac{\sin((2N+1)\omega/2)}{\sin(\omega/2)}\tag{7}$$

则公式6可改写为：

$$U_N(\omega)=\frac{1}{2\pi}\int_{-\pi}^{\pi}K_N(\omega-v)\mathrm{d}Z(v)\tag{8}$$

这里的K被称作Dirichlet Kernel。参见《数学狂想曲（一）》的相关章节。

一般来说，在公式8中，$$U_N(\omega)$$是已知的，而$$\mathrm{d}Z(\omega)$$是未知的。从数学上来说，这个积分方程可看做第一类Fredholm积分方程的一个例子。

>Erik Ivar Fredholm，1866～1927，瑞典数学家。Uppsala University博士（1898）+Stockholm University教授。不知道是不是瑞典的保险业比较发达，他和Cramér居然都当过兼职的精算师。。。瑞典皇家科学院院士。

>Uppsala University是瑞典，也是北欧最古老的大学，始建于1477年。

功率谱密度的估计方法主要包括参数法和非参数法两大类。

参数法包括：

1.模型辨识法。基本就是上面提到的ARIMA或者其变种。

2.最小方差无失真响应法（MVDR）。

3.特征分解法。将相关矩阵R分解为两个子空间：信号子空间和噪声子空间。

非参数法包括：

1.周期图法。

2.多窗口法。

参考：

https://www.zhihu.com/question/29520851

功率谱密度如何理解？

# 张量分析

在同构的意义下，第零阶张量（r = 0）为标量（Scalar），第一阶张量（r = 1）为向量（Vector），第二阶张量（r = 2）则成为矩阵（Matrix）。

《张量分析》，黄克智著。

>注：黄克智，1927年生，固体力学家。江西中正大学本科+清华硕士+莫斯科大学博士（因应召回国，放弃博士学位）。清华大学工程力学系教授、工程力学研究所所长，中国科学院院士。断裂力学领域权威。

# 拓扑学

《Topopogy Without Tears》，University of New South Wales的Sidney A. Morris著。

该书的中文版：

http://www.topologywithouttears.net/topbookchinese.pdf
