---
layout: post
title:  深度目标检测（四）——YOLOv3, 目标检测进阶
category: Deep Object Detection 
---

# YOLO

## 概述（续）

![](/images/article/yolo.png)

上图是YOLO的大致流程：

**Step 1**：Resize成448x448，图片分割得到7x7网格(cell)。

**Step 2**：CNN提取特征和预测：卷积部分负责提特征。全连接部分负责预测。

a) 7x7x2=98个bounding box(bbox) 的坐标$$x_{center},y_{center},w,h$$和是否有物体的confidence。

b) 7x7=49个cell所属20个物体分类的概率。

![](/images/article/yolo_2.png)

![](/images/article/yolo_3.png)

上图是YOLO的网络结构图，它采用经过修改的GoogLeNet作为base CNN。

从表面看，YOLO的输出只有一个，似乎比Faster RCNN两个输出少一个，然而这个输出本身，实际上要复杂的多。

YOLO的输出是一个7x7x30的tensor，其中7x7对应图片分割的7x7网格。30表明每个网格对应一个30维的向量。

![](/images/article/yolo_4.png)

上图是这个30维向量的编码方式：2个bbox+confidence是5x2=10维，20个物体分类的概率占其余20维。

总结一下，输出tersor的维度是：$$S\times S \times (B \times 5 + C)$$

这里的confidence代表了所预测的box中含有object的置信度和这个box预测的有多准两重信息：

$$\text{confidence} = \text{Pr}(Object) ∗ \text{IOU}_{pred}^{truth}$$

在loss函数设计方面，简单的把结果堆在一起，然后认为它们的重要程度都一样，这显然是不合理的，每个loss项之前的参数$$\lambda$$就是用来设定权重的。

**Step 3**：过滤bbox（通过NMS）。

![](/images/article/yolo_5.png)

上图是Test阶段的NMS的过程示意图。

## 参考

https://zhuanlan.zhihu.com/p/24916786

图解YOLO

https://mp.weixin.qq.com/s/n51XtGAsaDDAatXYychXrg

YOLO比R-CNN快1000倍，比Fast R-CNN快100倍的实时对象检测！

http://blog.csdn.net/tangwei2014/article/details/50915317

论文阅读笔记：You Only Look Once: Unified, Real-Time Object Detection

https://mp.weixin.qq.com/s/Wqj6EM33p-rjPIHnFKtmCw

计算机是怎样快速看懂图片的：比R-CNN快1000倍的YOLO算法

http://lanbing510.info/2017/08/28/YOLO-SSD.html

目标检测之YOLO，SSD

http://www.yeahkun.com/2016/09/06/object-detection-you-only-look-once-caffe-shi-xian/

Object detection: You Look Only Once(YOLO)

http://blog.csdn.net/zy1034092330/article/details/72807924

YOLO

https://mp.weixin.qq.com/s/c9yagjJIe-m07twPvIoPJA

YOLO算法的原理与实现

https://mp.weixin.qq.com/s/JKyX5cdvknIRoF341Au7Ew

单级式目标检测方法概述：YOLO与SSD

# YOLOv3

https://zhuanlan.zhihu.com/p/34945787

YOLOv3：An Incremental Improvement全文翻译

https://mp.weixin.qq.com/s/UWuCiV6dBk9Z0XusEBS6-g

物体检测经典模型YOLO新升级，就看一眼，速度提升3倍！

https://mp.weixin.qq.com/s/BF7mj-_D303cLiD5e6oy6w

期待已久的—YOLO V3

https://mp.weixin.qq.com/s/-Ixqxx6QGJDTM4g50y6LyA

进击的YOLOv3，目标检测网络的巅峰之作

https://zhuanlan.zhihu.com/p/46691043

YOLO v1深入理解

https://zhuanlan.zhihu.com/p/47575929

YOLOv2 / YOLO9000深入理解

https://zhuanlan.zhihu.com/p/49556105

YOLO v3深入理解

https://mp.weixin.qq.com/s/xNaXPwI1mQsJ2Y7TT07u3g

YOLO-LITE:专门面向CPU的实时目标检测

https://zhuanlan.zhihu.com/p/50170492

重磅！YOLO-LITE来了

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
