---
layout: post
title:  深度学习（四十八）——语义分割进阶
category: DL 
---

# 语义分割进阶

## U-NET的另类用法

U-NET除了用于语义分割之外，还可以用于语音分离——将人声/音乐从原始混合声音输入中分离出来。比如卡拉OK中的常见的原声抑制功能。

论文：

《Singing Voice Separation With Deep U-net Convolutional Networks》

代码：

https://github.com/Xiao-Ming/UNet-VocalSeparation-Chainer

Chainer版本的实现

https://github.com/Jeongseungwoo/Singing-Voice-Separation

Keras版本的实现

这种用途的U-NET和原始U-NET的区别在于：

1.输入和输出是音频数据的时序频谱图，从某种意义上来说，其实也是一张二维图片。

2.输入是包含混音的数据，而输出是纯净的人声/音乐的Mask。混音数据*Mask=纯净声音。由于标注数据比较难获得，因此通常的做法是使用纯音和若干噪声进行合成得到混音数据。

3.由于最终结果不再是像素级的分类问题，因此Loss采用了absolute difference。

从上面的论述可以看出，该论文主要是用到了语义分割网络中**输入和输出的尺寸等大**的特点，算是一种很灵巧的构思了。

## 参考

https://mp.weixin.qq.com/s/cANlqQAI-A2mC9vnd3imQA

Instance-Aware图像语义分割

https://mp.weixin.qq.com/s/v_TLYYq6cFWuwR9tXM8m-A

如何通过CRF-RNN模型实现图像语义分割任务

https://mp.weixin.qq.com/s/ceCC7Q6yr0QKESeZXi6lWQ

堆叠解卷积网络实现图像语义分割顶尖效果

https://mp.weixin.qq.com/s/V4_euZRcyyxeimXAA_waAg

贾佳亚：最有效的COCO物体分割算法

https://mp.weixin.qq.com/s/M1Oo4ST2aspgZF8UeSUDww

如何妙笔勾檀妆：像素级语义理解

https://mp.weixin.qq.com/s/xalo2XtKtzR5tA_dPFzaJw

一文介绍3篇无需Proposal的实例分割论文

https://mp.weixin.qq.com/s/BL1xZ_YuuPe9frIc9E1fkA

南开大学提出新物体分割评价指标

https://mp.weixin.qq.com/s/3rfZUhio4Bk1RUGkEk5xoQ

ETH Zurich提出新型网络“ROAD-Net”，解决语义分割域适配问题

https://mp.weixin.qq.com/s/qMLCi-CghxvTcwyPnvFxnQ

ConvCRF：一种结合条件随机场与CNN的高效语义分割方法

https://mp.weixin.qq.com/s/deepxMWCpIEe3jk_kanfMg

金字塔注意力网络：一种利用底层像素与高级特征的语义分割网络

https://mp.weixin.qq.com/s/LVD7rry0BajGh9iQg-Y2jw

MIT用AI实现3分钟自动抠图，精细到头发丝

https://mp.weixin.qq.com/s/5n3jpvv_LxnHB0w4hsCEzQ

NVIDIA ECCV18论文:超像素采样网络助力语义分割与光流估计

https://mp.weixin.qq.com/s/MiChpWim5pGlRj88rcQtaA

谷歌等祭出图像语义理解分割神器，PS再也不用专业设计师！

https://mp.weixin.qq.com/s/J6UMzWSpcmSQVGwWKtm2Hw

UC伯克利提出基于自适应相似场的语义分割

https://zhuanlan.zhihu.com/p/43774180

提升密集预测的平滑的空洞卷积

https://mp.weixin.qq.com/s/01D9wYK92i4PjjGcwFTxOw

揭秘阿里巴巴神奇的人物抠图算法内幕

https://mp.weixin.qq.com/s/sjD36kUDQ5iCmIKpR8_rlA

双重注意力网络：中科院自动化所提出新的自然场景图像分割框架

https://mp.weixin.qq.com/s/Sn3N8IxHtgp53Y0VLIPJCQ

17毫秒每帧！实时语义分割与深度估计

https://mp.weixin.qq.com/s/iNz82GUULxDndBIiGSArmQ

新开源！实时语义分割算法Light-Weight RefineNet
