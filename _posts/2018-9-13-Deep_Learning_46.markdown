---
layout: post
title:  深度学习（四十六）——OCR, NetVLAD
category: DL 
---

# AutoDL（续）

https://mp.weixin.qq.com/s/s10z5pkYf5Sha0V58T8fBw

单路径NAS: 在四小时内设计出给定硬件内最有效的网

https://www.zhihu.com/question/339002300

One-Shot 神经网络搜索(NAS)的searching阶段到底在做什么？

https://mp.weixin.qq.com/s/323RA6UtnyTySoXVXcJ2vA

华人团队打造：AutoML+GAN=AutoGAN！AI设计GAN模型比人类更好

https://mp.weixin.qq.com/s/nN7tgkvvkv8B8N-z4yAgcQ

四小时搜索NN结构，厦大提出快速NAS检索方法

https://zhuanlan.zhihu.com/p/71547478

NAS发展史：从放弃到入门

https://mp.weixin.qq.com/s/rgUTcuH2mfIFBDoOfd26Cw

NASNet：神经架构搜索网络在图像分类中的表现

# OCR

## 概述

光学字符识别（Optical Character Recognition, OCR），是指对文本资料的图像文件进行分析识别处理，获取文字及版面信息的过程。

华中科大白翔教授的实验室算是目前国内OCR做的比较好的了。

白翔的个人主页：

http://cloud.eic.hust.edu.cn:8071/~xbai/

该主页上有一个OCR方面的综述，是入门的最好资料。

http://www.cnblogs.com/lillylin/

这是一个OCR方面的blog。对白翔的论文，几乎都有阅读笔记。

https://github.com/hwalsuklee/awesome-deep-text-detection-recognition

Github：深度学习文本检测识别（OCR）精选资源汇总

## tesseract

linux下可以使用tesseract作为OCR工具。当然这个工具目前使用的还是传统算法。

安装方法：

`sudo apt install tesseract-ocr libtesseract-dev`

使用方法：

`tesseract ./111.png 1 -l chi_sim+eng`

## 车牌识别

https://github.com/liuruoze/EasyPR

一个开源的中文车牌识别系统。（使用传统算法）

https://blog.csdn.net/Relocy/article/details/78629441

HyperLPR：一个基于深度学习的支持多种车牌的中文开源车牌识别框架

https://blog.csdn.net/yang_daxia/article/details/90408160

车牌识别论文survey

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

https://mp.weixin.qq.com/s/Ai43GvLsEtFrfArHUvqLXg

Aster:具有柔性矫正功能的注意力机制场景文本识别方法

## CTPN

https://zhuanlan.zhihu.com/p/34757009

场景文字检测—CTPN原理与实现

https://mp.weixin.qq.com/s/lV2yjhvnRLDfNbZXOSjofg

ctpn：图像文字检测方法

## 参考

https://mp.weixin.qq.com/s/h7HVyGbmtLmNVJp4p0rCRQ

字符识别(OCR)相关工具/库/教材/论文等资源整理

https://mp.weixin.qq.com/s/XvtmPUbS2KV0zNXTPgtgDA

史上最全端到端场景文本检测识别资源合集（14篇重要论文 + 5个开源代码 + 49个实验结果 + 222个统计信息）

https://mp.weixin.qq.com/s/qLPfs5nHCVn5J-q3CbeECg

史上最全场景文字识别资源汇集（56篇重要论文 + 20 个开源代码 + 330 个实验结果 + 1882个统计信息）

https://mp.weixin.qq.com/s/rsJZ3R71gSAC4T501-LSQA

史上最全场景文字检测资源合集（70篇重要论文 + 15个开源代码 + 176个实验结果 + 1305个统计信息）

https://mp.weixin.qq.com/s/KFgC8zHWS7ysb9GfbkfLRA

OCR技术简介

https://zhuanlan.zhihu.com/p/65707543

文字OCR方法整理

https://mp.weixin.qq.com/s/0ysaJGNslckesv21o752FA

图像OCR年度进展

https://mp.weixin.qq.com/s/VIEsfc4qKAGsi-O9LD4mng

深度学习在OCR中的应用

https://blog.csdn.net/linolzhang/article/details/82780071

OCR文字识别

https://www.cnblogs.com/skyfsm/category/1123384.html

一个OCR方面的blog

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

http://mp.weixin.qq.com/s/rS4xCsytYGqBLMUzlyA12A

构建多层感知器神经网络对数字图片进行文本识别

https://mp.weixin.qq.com/s/qTnFQK0CkvdJfZaKj2wUtQ

海康威视联合提出注意力聚焦网络FAN：提升场景文本识别精确度

https://zhuanlan.zhihu.com/p/50521715

云从科技在自然场景OCR任务取得技术突破

https://zhuanlan.zhihu.com/p/51397423

SPCNet

https://mp.weixin.qq.com/s/9f158VM_FoNVuODNER-BUw

端到端的弯曲文本检测与识别

https://mp.weixin.qq.com/s/J5DGF3JRZxk1-fAQSQNvkQ

MORAN文本识别算法开源，刷新多个OCR数据集state-of-the-art

https://mp.weixin.qq.com/s/cLB2CPjLVJAuDVVmHRKHEA

弯曲文字检测之SPCNet

https://mp.weixin.qq.com/s/_znXs8pkfgmWsb26HIfhLQ

复杂环境文字识别技术研究及应用进展

https://mp.weixin.qq.com/s/JA6ey6N3Zho92L6jh_bRkg

EAST场景文字检测模型使用

https://mp.weixin.qq.com/s/uMzx_U-wx94GRTQtM11d8Q

OCR技术在携程业务中的应用

https://mp.weixin.qq.com/s/-zMVO47AL1iKFmF16KsfOw

文本检测算法PSENet解读与开源实现

https://mp.weixin.qq.com/s/UFfBoxQwsB_GUqTDRv3Asg

免费数学神器！再复杂的公式，只要有照片就能转成LaTeX

https://mp.weixin.qq.com/s/x8iyl4gwz31XOCw3oSHSwg

开源！基于TensorFlow/Keras/PyTorch实现对自然场景的文字检测及端到端的OCR中文文字识别

https://mp.weixin.qq.com/s/eVhId0OqKB-jNCg4j8qaNg

仅用200个样本就能得到当前最佳结果：手写字符识别新模型TextCaps

https://mp.weixin.qq.com/s/z5hRafxepA4Zj5pbbK8HzA

基于模板的文字识别结果结构化处理技术

https://mp.weixin.qq.com/s/Buv6m66kj3RjKq0LjqNAiA

mathAI手写拍照自动能解高数题，还不快试试？

https://mp.weixin.qq.com/s/gKsfsRxsbfXGOsSqeQEDbw

CVPR2019：文本检测算法综述

https://zhuanlan.zhihu.com/p/68058851

自然场景下的文字检测：从多方向迈向任意形状

https://mp.weixin.qq.com/s/jO2xc7Ri6TvrHyXHH0uX4w

金山WPS：基于本地TensorFlow Lite和AI云服务的文档矫正功能

https://mp.weixin.qq.com/s/3t8yzjMXiZwgqBb8izVfFg

场景文字检测模型PixelLink详解与使用

https://zhuanlan.zhihu.com/p/68356509

TensorFlow 2.0中文手写字识别（汉字OCR）

https://mp.weixin.qq.com/s/F0bPat3KUEVnej77OdfYAg

文本检测大满贯！自适应文本区域表示改进形状任意的文本检测

https://mp.weixin.qq.com/s/QwkKqybzW0tYcGojQg9r2w

深度学习场景文字检测识别，20页综述带你了解领域最新动态

https://mp.weixin.qq.com/s/j8Xvj7OSoFZp-zIrd3Xjow

华科提出对称性约束的校正网络ScRN，显著改进场景文本识别

https://github.com/ctripcorp/C-OCR

携程自研的OCR项目

https://mp.weixin.qq.com/s/fZN8mtZdB4Fd6gj2dAKURw

金连文：“文字检测与识别：现状及展望”

https://mp.weixin.qq.com/s/_GU5y7v8HNDOjEGvWh8Fkw

旷视研究院提出文字检测新方法：像素聚合网络PAN

https://mp.weixin.qq.com/s/5eTugA70myKxDTZUby87Pw

任意形状文本检测：Look More Than Once

https://mp.weixin.qq.com/s/RuYZ-nmW2p7jMIJ7sbxbIw

任意形状文本检测的像素聚合网络

https://mp.weixin.qq.com/s/T0QeYbkmD4jwhWB47Fnn2w

基于深度学习的端到端在线手写数学公式识别

# NetVLAD

NetVLAD算的上是CNN+传统算子的一个范例。

论文：

《NetVLAD: CNN architecture for weakly supervised place recognition》

《GhostVLAD for set-based face recognition》

数据集：

http://places.csail.mit.edu/

## VLAD

Vector of Locally Aggregated Descriptors

https://www.cnblogs.com/minemine/p/7364950.html

场景分类(scene classification)摘录

http://www.cnblogs.com/mafuqiang/p/6909556.html

图像检索——VLAD

## 参考

https://www.oukohou.wang/2018/11/27/NetVLAD/

论文阅读-NetVLAD

https://www.oukohou.wang/2018/12/26/GhostVLAD/

论文阅读-GhostVLAD

https://mp.weixin.qq.com/s/cfUl0Eym0mu7rSJJL7Zt1A

基于深度学习的视觉实例搜索研究进展

https://zhuanlan.zhihu.com/p/25013378

深度纹理编码网络 (Deep TEN: Texture Encoding Network)

https://blog.csdn.net/LiGuang923/article/details/85416407

图像检索与降维（一）：VLAD

https://blog.csdn.net/LiGuang923/article/details/85470289

图像检索与降维（二）：NetVLAD
