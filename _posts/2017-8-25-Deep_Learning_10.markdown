---
layout: post
title:  深度学习（十）——SPPNet, Fast R-CNN, Faster-RCNN, YOLO
category: theory 
---

# RCNN（续）

## 评价标准

http://blog.sina.com.cn/s/blog_9db078090102whzw.html

多标签图像分类任务的评价方法-mAP

## 总结

![](/images/article/rcnn_p_2.png)

![](/images/article/rcnn_p.png)

## 参考

https://zhuanlan.zhihu.com/p/23006190

RCNN-将CNN引入目标检测的开山之作

http://www.cnblogs.com/edwardbi/p/5647522.html

Tensorflow tflearn编写RCNN

http://blog.csdn.net/u011534057/article/category/6178027

RCNN系列blog

https://www.zhihu.com/question/35887527

如何评价rcnn、fast-rcnn和faster-rcnn这一系列方法？

http://blog.csdn.net/tangwei2014/article/details/50915317

论文阅读笔记：You Only Look Once: Unified, Real-Time Object Detection

http://blog.csdn.net/shenxiaolu1984/article/details/51066975

RCNN算法详解

https://mp.weixin.qq.com/s/XorPkuIdhRNI1zGLwg-55A

斯坦福新深度学习系统 NoScope：视频对象检测快1000倍

https://mp.weixin.qq.com/s/XbgmLmlt5X4TX5CP59gyoA

目标检测算法精彩集锦

https://mp.weixin.qq.com/s/BgTc1SE2IzNH27OC2P2CFg

CVPR-I

https://mp.weixin.qq.com/s/qMdnp9ZdlYIja2vNEKuRNQ

CVPR—II

https://mp.weixin.qq.com/s/tc1PsIoF1RN1sx_IFPmtWQ

CVPR—III

https://mp.weixin.qq.com/s/bpCn2nREHzazJYq6B9vMHg

目标识别算法的进展

https://mp.weixin.qq.com/s/YzxaS4KQmpbUSnyOwccn4A

基于深度学习的目标检测技术进展与展望

https://mp.weixin.qq.com/s/JPCQqyzR8xIUyAdk_RI5dA

RCNN, Fast-RCNN, Faster-RCNN那些你必须知道的事！

http://blog.csdn.net/messiran10/article/details/49132053

Caffe matlab之基于Alex network的特征提取

# SPPNet

SPPNet是何恺明2014年的作品。

论文：

《Spatial Pyramid Pooling in Deep Convolutional Networks for Visual Recognition》

在RCNN算法中，一张图片会有1~2k个候选框，每一个都要单独输入CNN做卷积等操作很费时。而且这些候选框可能很多都是重合的，重复的CNN操作从信息论的角度，也是相当冗余的。

![](/images/article/rcnn_vs_spp.png)

SPPNet的核心思想如上图所示：在feature map上提取ROI特征，这样就只需要在整幅图像上做一次卷积。

这个想法说起来简单，但落到实地，还有如下问题需要解决：

**Problem 1**：原始图像的ROI如何映射到特征图（一系列卷积层的最后输出）。

**Problem 2**：ROI的在特征图上的对应的特征区域的维度不满足全连接层的输入要求怎么办（又不可能像在原始ROI图像上那样进行截取和缩放）？

对于Problem 2我们分析一下：

这个问题涉及的流程主要有: 图像输入->卷积层1->池化1->...->卷积层n->池化n->全连接层。

引发问题的原因主要有：全连接层的输入维度是固定死的，导致池化n的输出必须与之匹配，继而导致图像输入的尺寸必须固定。

解决办法可能有：

1.想办法让不同尺寸的图像也可以使池化n产生固定的输出维度。（打破图像输入的固定性）

2.想办法让全连接层（罪魁祸首）可以接受非固定的输入维度。（打破全连接层的固定性，继而也打破了图像输入的固定性）

以上的方法1就是SPPnet的思想。

![](/images/article/spp.png)

**Step 1**：为图像建立不同尺度的图像金字塔。上图为3层。

**Step 2**：将图像金字塔中包含的feature映射到固定尺寸的向量中。上图为$$(16+4+1)\times 256$$维向量。

总结：

![](/images/article/spp_p.png)

参考：

https://zhuanlan.zhihu.com/p/24774302

SPPNet-引入空间金字塔池化改进RCNN

# Fast R-CNN

Fast R-CNN是Ross Girshick于2015年祭出的又一大招。

论文：

《Fast R-CNN》

代码：

https://github.com/rbgirshick/fast-rcnn

![](/images/article/fast_rcnn_p_2.png)

![](/images/article/fast_rcnn_p.png)

参考：

https://zhuanlan.zhihu.com/p/24780395

Fast R-CNN

http://blog.csdn.net/shenxiaolu1984/article/details/51036677

Fast RCNN算法详解

# Faster-RCNN

![](/images/article/faster_rcnn_p.png)

Region Proposal Networks

参考：

https://zhuanlan.zhihu.com/p/24916624

Faster R-CNN

http://blog.csdn.net/shenxiaolu1984/article/details/51152614

Faster RCNN算法详解

https://mp.weixin.qq.com/s/VKQufVUQ3TP5m7_2vOxnEQ

通过Faster R-CNN实现当前最佳的目标计数

# YOLO

YOLO: Real-Time Object Detection，是一个基于神经网络的实时对象检测软件。

官网：

https://pjreddie.com/darknet/yolo/

参考：

https://zhuanlan.zhihu.com/p/24916786

图解YOLO

https://mp.weixin.qq.com/s/n51XtGAsaDDAatXYychXrg

YOLO比R-CNN快1000倍，比Fast R-CNN快100倍的实时对象检测！

https://mp.weixin.qq.com/s/Wqj6EM33p-rjPIHnFKtmCw

计算机是怎样快速看懂图片的：比R-CNN快1000倍的YOLO算法

https://zhuanlan.zhihu.com/p/25167153

YOLO2

