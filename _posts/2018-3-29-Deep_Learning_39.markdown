---
layout: post
title:  深度学习（三十九）——手势识别, 深度压缩感知, SNN, BNN
category: DL 
---

* toc
{:toc}

# 手势识别

https://zhuanlan.zhihu.com/p/26630215

浅谈手势识别在直播中的运用

https://zhuanlan.zhihu.com/p/30561160

2017-最全手势识别/跟踪相关资源大列表分享

http://www.sohu.com/a/203306961_465975

浙江大学CSPS最佳论文：使用卷积神经网络的多普勒雷达手势识别

https://www.zhihu.com/question/20131478

我打算只根据手的形状来识别手势。用哪种机器学习算法比较好？

https://www.leiphone.com/news/201502/QM7LdSN874dWXFLo.html

带你了解世界最先进的手势识别技术

https://mp.weixin.qq.com/s/DbvH6jM1VV47xKylbW-pug

掌纹识别近十年进展综述

https://mp.weixin.qq.com/s/mnPh8w3VuG9apprOkugbLA

中科大提出新型连续手语识别框架LS-HAN，帮助“听”懂听障人士

https://mp.weixin.qq.com/s/pUciYFjOKL3ea91fLCy0Yw

基于OpenCV与tensorflow实现实时手势识别

https://mp.weixin.qq.com/s/xtTmPtjCk4FQuQ3RnPZxEg

UC伯克利黑科技：用语音数据预测说话人手势

https://blog.csdn.net/wangyaninglm/article/details/87296595

指纹的对比分析系统概述

https://mp.weixin.qq.com/s/oybz1DsC8lO5fmgA-3gEfQ

指纹识别不灵敏怎么办？

https://mp.weixin.qq.com/s/ji8sEzJXp1UNgBHVOui0ng

谷歌开源手势识别器，手机能用，还有现成的App，但是被我们玩坏了

# 深度压缩感知

Tiny Network Graphics是图鸭科技推出一种基于深度学习的图片压缩技术。由于商业因素，这里没有论文，技术细节也不详，但是下图应该还是有些用的。

![](/images/img2/TNG.png)

还有视频压缩：

论文：

《Deep Learning-Based Video Coding: A Review and A Case Study》

---

TSAC的原理基于RVQGAN。

https://www.zhihu.com/question/652616403

如何评价FFmpeg之父发布音频压缩工具TSAC？

---

参考：

https://mp.weixin.qq.com/s/YBJwLqqL7aVUTG0LaUbwxw

深度学习助力数据压缩，一文读懂相关理论

https://mp.weixin.qq.com/s/WYsxFX4LyM562bZD8rO95w

图鸭发布图片压缩TNG，节省55%带宽

https://mp.weixin.qq.com/s/meK8UBnVHzA9YspQ2RFp6Q

体积减半画质翻倍，他用TensorFlow实现了这个图像极度压缩模型

https://mp.weixin.qq.com/s/_5tyt7pU0gIXbkmTOVEtDw

嫌图片太大？！卷积神经网络轻松实现无损压缩到20%！

https://mp.weixin.qq.com/s/a4oU8UK_hLMrKXNRQizAag

图鸭科技获CVPR 2018图像压缩挑战赛单项冠军，技术解读端到端图像压缩框架

https://mp.weixin.qq.com/s/VDyPjzXdwMGEsoXQmhrp9g

图鸭科技斩获CVPR图像压缩挑战赛冠军，TNGcnn4p技术全解读

https://mp.weixin.qq.com/s/B7reSwa9sCZqbkYVM5-VOA

图像压缩哪家强？请看这份超详细对比

https://mp.weixin.qq.com/s/K17wlC3tueNBfHkYBUFcQg

基于深度学习的HEVC复杂度优化。这是篇视频压缩的blog。

https://mp.weixin.qq.com/s/exUYS2v5VyRaMdFylWlobw

用循环神经网络进行文件无损压缩：斯坦福大学提出DeepZip

https://mp.weixin.qq.com/s/GEMOfh04XR5IyWWlvZeeng

CLIC图像压缩挑战赛冠军方案解读

https://zhuanlan.zhihu.com/p/78050429

基于深度学习的视频压缩方案介绍

https://mp.weixin.qq.com/s/gNtxBI0Alk70cEujxQmSFQ

如何将图像压缩10倍？阿里工程师有个大胆的想法！这是一篇传统算法的blog。

https://mp.weixin.qq.com/s/OkywKX4XygM8VqkL8A1fcA

TIP 2019开源论文：基于深度学习的HEVC多帧环路滤波方法

https://mp.weixin.qq.com/s/Qod-SHNa-El48_n-w5PCLQ

超越H.265，中科大使用多帧数据改进视频压缩新方法

https://zhuanlan.zhihu.com/p/150340687

可逆图像缩放：完美恢复降采样后的高清图片

https://mp.weixin.qq.com/s/jFT6jULJXypFryLB5VEjpw

基于深度学习的图像与视频压缩

https://mp.weixin.qq.com/s/sBdAj6tS_FJQ-uxqrMRuOQ

大话实时视频编码中的人工智能（上）

https://mp.weixin.qq.com/s/9_ZHrgWwGwX0pyafgCvCsg

大话实时视频编码中的人工智能（下）

https://mp.weixin.qq.com/s/wzUbYyrBOxU-2bY-EJm4KA

极端图像压缩的生成对抗网络，可生成低码率的高质量图像

https://mp.weixin.qq.com/s/3bi5Timesxi1YbFdzBs8AA

DeepMind论文：深度压缩感知，新框架提升GAN性能

# Spiking Neural Network

除了基于BP算法的NN之外，Spiking Neural Network也是一大类NN。Spiking NN和人脑结构更相似，功耗也更小，但是相关训练和数据量化的算法尚不成熟，属于潜力股。

![](/images/img6/SNN.jpg)

SNN使用脉冲——这是一种发生在时间点上的离散事件——而非常见的连续值。每个峰值由代表生物过程的微分方程表示出来，其中最重要的是神经元的膜电位。本质上，一旦神经元达到了某一电位，脉冲就会出现，随后达到电位的神经元会被重置。对此，最常见的模型是 Leaky Integrate-And-Fire (LIF) 模型。

SNN的训练方法主要有：

- Spike timing dependent plasticity (STDP) ：一种无监督学习方法
- Spatio-temporal backpropagation (STBP)：一种反向传播算法。

在ANN中，计算开销主要由FC的MAC运算决定。

而在SNN中，主要的计算成本来自脉冲输入的积分过程，实际上就是统计Spike事件的次数，是个纯加法运算。

![](/images/img2/BrainChip_Fig2.gif)

![](/images/img3/Tianjic.png)

参考：

https://homepages.cwi.nl/~sbohte/publication/paugam_moisy_bohte_SNNChapter.pdf

Computing with Spiking Neuron Networks

https://blog.csdn.net/Extremevision/article/details/123853471

详解脉冲神经网络的架构原理、数据集和训练方法

https://blog.csdn.net/weixin_37864449/article/details/126772830

脉冲神经网络(SNN)·第三代神经网络

https://mp.weixin.qq.com/s/6dpKSaLFVo-ge4gtbG8GQg

简述脉冲神经网络SNN：下一代神经网络

https://mp.weixin.qq.com/s/0n50YO1jIv_mxqe0EeS6kw

综述AI未来：神经科学启发的类脑计算

https://mp.weixin.qq.com/s/5KA7jtlRmnXxijGQhU1k4A

DeepMind哈萨比斯狂推的神经科学，入门需要看什么书？

https://mp.weixin.qq.com/s/TWdeHVCgEf54STvdA1QUPg

DeepMind哈萨比斯长文：伟大的AI离不开神经科学

https://mp.weixin.qq.com/s/8ibcyvyBLYArAMhQElqRzg

Cell研究揭示生物神经元强大新特性，是时候设计更复杂的神经网络了！

https://mp.weixin.qq.com/s/cb6JBlb11xW0Xw0RWI4vFA

浙大&川大提出脉冲版ResNet：继承ResNet优势，实现当前最佳

https://mp.weixin.qq.com/s/yaAuVpuhSGabOswKnv9q5Q

脉冲神经网络与小样本学习

https://mp.weixin.qq.com/s/m7UHX3XL5sC3oBdu3KoyOg

脉冲神经网络（SNN）概述

https://mp.weixin.qq.com/s/vwZfmyIEdEdpqJWKOSKYYw

清华天机AI芯片登Nature封面：全球首款异构融合类脑芯片，实现自行车无人驾驶

https://mp.weixin.qq.com/s/skA3NZIAzTrsnSsNCxCYSA

类脑计算背后的计算神经科学框架

https://mp.weixin.qq.com/s/ku78_exDM-OUwWPBCNahCg

Spiking-YOLO：前沿性研究，脉冲神经网络在目标检测的首次尝试

https://blog.csdn.net/u011853479/article/details/61414913

脉冲神经网络的五脏六腑

https://blog.csdn.net/Yannan_Strath/article/details/105761023

脉冲神经网络（Spiking Neural Network）叙述

https://blog.csdn.net/Yannan_Strath/article/details/108190281

脉冲神经网络（Spiking Neural Network）发展现状

# Binary Neural Network

二值神经网络的主要缺点在于：它们无法实现与完全精度的深层网络一样高的精度。但这一直在缓慢地变化，已经有了很多进步。

综述：

《BiBench: Benchmarking and Analyzing Network Binarization》

![](/images/img5/BNN.png)

BNN,DoReFA,Bi-Real,ReActNet一脉相承。

XNOR,XNOR++,ReCU为另一个流派。

参考：

http://blog.csdn.net/tangwei2014/article/details/55077172

二值化神经网络介绍

https://mp.weixin.qq.com/s/0twiT2mrVdnwyS-mqgrjVA

低比特量化之XNOR-Net

https://mp.weixin.qq.com/s/oumf8l28ijYLxc9fge0FMQ

嵌入式深度学习之神经网络二值化（1）

https://mp.weixin.qq.com/s/tbRj5Wd69n9gvSzW4oKStg

嵌入式深度学习之神经网络二值化（2）

https://mp.weixin.qq.com/s/RsZCTqCKwpnjATUFC8da7g

嵌入式深度学习之神经网络二值化（3）

https://blog.csdn.net/stdcoutzyx/article/details/50926174

二值神经网络（Binary Neural Network，BNN）

https://mp.weixin.qq.com/s/Q54AdQmqa5JD0v9CEeFtSQ

二值化神经网络(BNN)综述

https://zhuanlan.zhihu.com/p/431680710

谈谈BNN二值化神经网络的设计，以及几代学界工作的演进 -（1）架构与原理

https://zhuanlan.zhihu.com/p/433429767

谈谈BNN二值化神经网络的设计，以及几代学界工作的演进 -（2）二值训练

https://zhuanlan.zhihu.com/p/435285316

谈谈BNN二值化神经网络的设计，以及几代学界工作的演进 -（3）二值化设计法则、推理框架与发展潜力

https://mp.weixin.qq.com/s/lVja7woyFWpmr9sH0CitAA

BMXNet：基于MXNet的开源二值神经网络实现

https://mp.weixin.qq.com/s/naDk0mmxd08dNl9LawLUnw

不使用先验知识与复杂训练策略，从头训练二值神经网络！

https://mp.weixin.qq.com/s/Xvlxs-Os2meduHrEQFc7vg

第一次胜过MobileNet的二值神经网络，-1与+1的三年艰苦跋涉

https://mp.weixin.qq.com/s/Ak9Yh_MBDR6i7J2rDR99eQ

低成本的二值神经网络介绍以及它能代替全精度网络吗?

https://mp.weixin.qq.com/s/tbRj5Wd69n9gvSzW4oKStg

异或神经网络

https://mp.weixin.qq.com/s/XzLJzfvpP93cDYplf6-LXA

港科腾讯等提出Bi-Real net：超XNOR-net 10%的ImageNet分类精度

https://mp.weixin.qq.com/s/wCx7rQFwC2mW45FMR77tGQ

二值网络，围绕STE的那些事儿

https://mp.weixin.qq.com/s/7L26ghhDqdMU6LRV0iD6vQ

模型量化从1bit到8bit，二值到三值
