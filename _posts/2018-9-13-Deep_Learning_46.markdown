---
layout: post
title:  深度学习（四十六）——OCR, NetVLAD, 李飞飞
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

# 李飞飞

## AI大佬

李飞飞是吴恩达之后的华裔AI新大佬。巧合的是，他们都是斯坦福AP+AI lab的主任，只不过吴是李的前任而已。

**李飞飞（Fei-Fei Li）**，1976年生，成都人，16岁移民美国。普林斯顿大学本科（1995～1999）+加州理工学院博士（2001～2005）。先后执教于UIUC、普林斯顿、斯坦福等学校。

个人主页：

http://vision.stanford.edu/feifeili/

她的老公Silvio Savarese，也是斯坦福的AP。

## 大佬的门徒

比如可爱的妹子**Serena Yeung**。这个妹子是斯坦福的本硕博。出身不详，但从姓名的英文拼法来看，应该是美国土生的华裔。Yeung是杨、阳、羊等姓的传统英文拼法，但显然不是大陆推行的拼音拼法。（可以对比的是Fei-Fei Li和Bruce Lee，对于同一个姓的不同拼法。）

个人主页：

http://ai.stanford.edu/~syyeung/

还有当红的“辣子鸡”：**Andrej Karpathy**，多伦多大学本科（2009）+英属不列颠哥伦比亚大学硕士（2011）+斯坦福博士（2015）。现任特斯拉AI总监。

吐槽一下：英属不列颠哥伦比亚大学其实是加拿大的一所大学。

个人主页：

http://cs.stanford.edu/people/karpathy/

Andrej Karpathy建了一个检索arxiv的网站，主要搜集了近3年来的ML/DL领域的论文。网址：

http://www.arxiv-sanity.com/

**李佳（Jia Li）**，李飞飞的开山大弟子，追随她从UIUC、普林斯顿到斯坦福。目前又追随其到Google。大约是知道自己的名字是个大路货，她的笔名叫做Li-Jia Li。

个人主页：

http://vision.stanford.edu/lijiali/

## 学神

应该说李飞飞和吴恩达都是万里挑一的超卓人物，但是和学神还是有所差距。下面是两个80后的华裔学神，他们都已经是正教授了：

**尹希**，1983年生，哈佛大学物理系教授。

**张锋**，1982年生，MIT教授，生物学家。

这两个人都是有机会挑战诺奖的人，而李和吴暂时还没有这个可能性。

## 网红

这里收录了一些非李飞飞门下的AI网红。

**Zachary Chase Lipton**，1985年生，哥伦比亚大学本科+UCSD博士，CMU的AP。他的另一身份——Jazz歌手，可比他的学术成就知名多了。

个人主页：

http://zacklipton.com/
