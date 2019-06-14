---
layout: post
title:  深度学习（四十六）——OCR, 问答系统
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

# 问答系统

GA-Reader

Match-LSTM

Bi-DAF

R-Net

QA-Net

S-Net

R3

## 参考

https://mp.weixin.qq.com/s/IahvlkiACOAjicX68teA0A

套路深深深几许？管窥问答系统、阅读理解的小江湖

https://mp.weixin.qq.com/s/h51gh-9LISEBdJx8X--Lsg

近期有哪些值得读的QA论文？

https://mp.weixin.qq.com/s/lB44D_RFBqbNB6YX5gEULg

近期值得读的QA论文！专题论文解读

http://www.taodocs.com/p-4795349.html

基于大规模问答语料的问题检索系统

https://wenku.baidu.com/view/d38ab1e8856a561252d36fab.html

短文本相似度计算在用户交互式问答系统中的应用

https://mp.weixin.qq.com/s/5ajhhc4mpIhoqkl5VkGnaw

深度学习实现问答机器人

https://mp.weixin.qq.com/s/2aKoOx18RB0GGQTzb3Tjzg

Facebook开源DrQA的PyTorch实现：基于维基百科的问答系统

https://mp.weixin.qq.com/s/4TC7180LmD7V6u1CdVYRFw

漫谈机器阅读理解之Facebook提出的DrQA系统

https://mp.weixin.qq.com/s/SYUq1Qd-IOmVjgs7Kp4OAg

如何打造智能问答引擎

https://mp.weixin.qq.com/s/Mzycy0chQUNWjJYdpERBOw

一文详解维基百科的开放性问答系统

https://www.cnblogs.com/combfish/p/6708667.html

(QA-LSTM)自然语言处理：智能问答IBM保险QA QA-LSTM实现笔记

https://mp.weixin.qq.com/s/BhDy55Mj5oMArO0MKwvnKw

基于异构社交网络学习的社区问答方法，同时建模问题、回答和回答者

https://mp.weixin.qq.com/s/bZgis3dxMbv6AnLNyk04Og

用数据做酷的事！手把手教你搭建问答系统

https://mp.weixin.qq.com/s/nIMk-xl8Wzy1ANcT3ApAng

百度提出问答模型GNR：检索速度提高25倍

https://mp.weixin.qq.com/s/eDA6-BqPmjGBOPsOom0VIw

自动组合神经网络做问答系统！

https://mp.weixin.qq.com/s/vazeuiFvCC8aIhP2uAXoew

达观数据智能问答技术研究

https://mp.weixin.qq.com/s/eaxyhk93PbEFzHk-qiniOQ

ReQuest: 使用问答数据产生实体关系抽取的间接监督

https://mp.weixin.qq.com/s/1VYdCLw6q4rS91_YV4SclA

理解智能问答系统（Ⅰ）

https://mp.weixin.qq.com/s/x4yXjl0YXytAgp8a64ebXg

智能问答开源项目之YodaQA（Ⅱ）

https://mp.weixin.qq.com/s/2VPfk7jX0eBP2NZMB9igBg

智能问答之使用UIMA进行文本挖掘（Ⅲ）

https://mp.weixin.qq.com/s/NohUxQKHw8Z3kX_riGM10g

智能问答之答案抽取（Ⅳ）

https://mp.weixin.qq.com/s/EU0O-WR0ynI04NsPZ-bn2g

无从下手落地问答系统？实用百度开源框架了解一下

https://mp.weixin.qq.com/s/F1z7evNDqghEzSD-_Xbgfw

基于卷积深度相关性计算的社区问答方法，建模问题和回答的匹配关系

https://mp.weixin.qq.com/s/_klvAhH-K8tcYmamlr6FcA

Logistic Regression Models分析交互式问答

https://mp.weixin.qq.com/s/_XyqNPkInWfy-gUQtHjNIg

基于深度神经网络的自动问答系统概述

https://mp.weixin.qq.com/s/GyE9qdXPGvrq12dMAf4nrQ

论文推荐：QA，增强学习，知识图谱，机器阅读理解

https://mp.weixin.qq.com/s/6dKticG2I2zqlxnZ3W0ZgQ

“猜你所想，答你所问”，携程智能客服算法实践

https://mp.weixin.qq.com/s/yiAC6SddIFFo3bR351b_0Q

Embodied Question Answering

https://mp.weixin.qq.com/s/L227SZXQPinNOkKJjwr58w

85页《基于神经方法的人机对话系统》，微软+谷歌出品

https://mp.weixin.qq.com/s/_R9LUJhAb0eDCs14BJsoDA

平安人寿资深算法工程师谢舒翼：智能问答系统探索与实践

https://mp.weixin.qq.com/s/wH-YYzifsEqMCD_4n8B-Kw

12张图看懂Gartner《智能客服机器人行业最佳实践》报告

https://mp.weixin.qq.com/s/U2r4qDLh4WZFgAIoF_SRPg

金融客服AI新玩法：语言学运用、LSTM+DSSM算法、多模态情感交互

https://mp.weixin.qq.com/s/tntgiltyHfbIBktgdL3peA

15年研发经验博士手把手教学：从零开始搭建智能客服

https://mp.weixin.qq.com/s/hhWRhBeRCmxjr9Lh8aE_Ww

如何用20万条客服咨询消息“喂”出定制化聊天机器人

https://mp.weixin.qq.com/s/LFmZMcunhJ-9ey9_igrTZA

行业智能客服构建探索

https://mp.weixin.qq.com/s/OHSqHNo7vjLGaTKyzjZjyg

基于智能客服的用户日志研究
