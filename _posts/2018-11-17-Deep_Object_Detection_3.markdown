---
layout: post
title:  深度目标检测（三）——Fast R-CNN, YOLOv3, 目标检测进阶
category: Deep Object Detection 
---

# Fast R-CNN

Fast R-CNN是Ross Girshick于2015年祭出的又一大招。

论文：

《Fast R-CNN》

代码：

https://github.com/rbgirshick/fast-rcnn

![](/images/article/fast_rcnn_p_2.png)

上图是Fast R-CNN的结构图。从该图可以看出Fast R-CNN和SPPnet的主要差异在于：

1.使用ROI（Region of interest） Pooling，替换SPP。

2.去掉了SVM分类。

以下将对这两个方面，做一个简述。

## ROI Pooling

SPP将图像pooling成多个固定尺度，而RoI只将图像pooling到单个固定的尺度。（虽然多尺度学习能提高一点点mAP，不过计算量成倍的增加。）

普通pooling操作中，pooling区域的大小一旦选定，就不再变化。

而ROI Pooling中，为了将ROI区域pooling到单个固定的目标尺度，我们需要根据ROI区域和目标尺度的大小，动态计算pooling区域的大小。

ROI Pooling有两个输入：feature map和ROI区域。Pooling方式一般为Max Pooling。Pooling的kernel形状可以不为正方形。

## Bounding-box Regression

从Fast R-CNN的结构图可以看出，与一般的CNN不同，它在FC之后，实际上有两个输出层：第一个是针对每个ROI区域的分类概率预测（上图中的Linear+softmax），第二个则是针对每个ROI区域坐标的偏移优化（上图中的Linear）。

然后，这两个输出层（即两个Task）再合并为一个multi-task，并定义统一的loss。

![](/images/article/fast_rcnn_multi_task.png)

由于两个Task的信息互为补充，使得分类预测任务的softmax准确率大为提升，SVM也就没有存在的必要了。

上图中提到的smooth L1 Loss可参见：

http://pages.cs.wisc.edu/~gfung/GeneralL1/L1_approx_bounds.pdf

Fast Optimization Methods for L1 Regularization: A Comparative Study and Two New Approaches Suplemental Material

## 全连接层提速

Fast R-CNN的论文中还提到了全连接层提速的概念。这个概念本身和Fast R-CNN倒没有多大关系。因此，完全可以将之推广到其他场合。

![](/images/article/fc_svd.png)

它的主要思路是，在两个大的FC层（假设尺寸为u、v）之间，利用SVD算法加入一个小的FC层（假设尺寸为t），从而减少了计算量。

$$u\times v\to u\times t+t\times v$$

## 总结

![](/images/article/fast_rcnn_p.png)

参考：

https://zhuanlan.zhihu.com/p/24780395

Fast R-CNN

http://blog.csdn.net/shenxiaolu1984/article/details/51036677

Fast RCNN算法详解

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
