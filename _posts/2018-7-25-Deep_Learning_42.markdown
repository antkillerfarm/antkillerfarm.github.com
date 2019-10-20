---
layout: post
title:  深度学习（四十二）——深度ISP, Spiking Neuron Networks, 深度时间序列, AI可解释性, 手势识别, NetVLAD
category: DL 
---

# 迁移学习（续）

https://mp.weixin.qq.com/s/kPFS_4swYEP6Amw8xfXEgg

中科院自动化所-针对小样本问题的学习生成匹配网络方法

https://mp.weixin.qq.com/s/Kb1KggOE3gxy7Edn48dlMg

自然语言处理的迁移学习--来自DeepMind、AllenNLP等，附238页Slides&代码

https://mp.weixin.qq.com/s/NFziZe6xS_CzVZX_0Den9g

达观数据CTO纪达麒:小标注数据量下自然语言处理实战经验

https://mp.weixin.qq.com/s/6w5S5R_qA0956VMj1isunQ

雷哥带你读论文之深度迁移炼丹！

https://mp.weixin.qq.com/s/uVby0GpWlmAlqRlbvysSyQ

计算机视觉中深度迁移学习，165页PPT

https://mp.weixin.qq.com/s/wB9skXSWoMHoWnME2JAyIw

基于全局类别表征的小样本学习

https://mp.weixin.qq.com/s/6eHmudo6EG47qBe7xwri6g

ImageNet错误率小于4%，数据量依然不够，N-Shot Learning或是终极解决之道？

https://mp.weixin.qq.com/s/06FWeGysBjU9wTMUx_eY0Q

一文看懂自然语言处理中迁移学习的现状

https://mp.weixin.qq.com/s/3AhiaNilRyTX_f7F306yng

主动学习（Active Learning）-少标签数据学习

https://mp.weixin.qq.com/s/Rj55EoopzlR71DZ5XrvH_w

八千字长文深度解读，迁移学习在强化学习中的应用及最新进展

https://mp.weixin.qq.com/s/G-Z6zyYSV95--x0PjQVMiw

自然语言处理中的迁移学习(上)

https://mp.weixin.qq.com/s/WlpmZmmqsepwbZJqxXrUhw

自然语言处理中的迁移学习(下)

https://mp.weixin.qq.com/s/cG0TDIZ3z4lUEQb_N1ysbg

使用迁移学习构建顶尖会话AI

# 深度ISP

## 数据集

### HDR+

HDR+是一个使用连拍摄影生成更好的图像的数据集。

官网：

http://hdrplusdata.org

参考：

https://zhuanlan.zhihu.com/p/34391353

机器感知Google推出HDR+连拍摄影数据集

### HDRNet

HDRNet是一个Image Enhancement方面的数据集。

官网：

https://groups.csail.mit.edu/graphics/hdrnet/

## 综述

论文：

《Unprocessing Images for Learned Raw Denoising》

这篇论文虽然不是综述，但有很多内容讲解ISP的流程。

## 参考

https://mp.weixin.qq.com/s/wA85XFQXeypuoqFnmN2P4g

降噪的新时代

https://mp.weixin.qq.com/s/919VEvennHEG3iXKkMZoQQ

不止是去噪---从去噪看AI ISP的趋势

https://zhuanlan.zhihu.com/p/27902193

利用卷积自编码器对图片进行降噪

https://zhuanlan.zhihu.com/p/39512000

Noise2Noise：图像降噪，无需干净样本

https://mp.weixin.qq.com/s/_tvOQPvybqmvLF19kHcbFg

北大开源ECCV2018深度去雨算法：RESCAN

https://mp.weixin.qq.com/s/Wdxkvlz4nLbJS_gWqHwMjw

无需额外硬件，全卷积网络让机器学习学会夜视能力

https://mp.weixin.qq.com/s/iH7gbRn4opLsWgKWoVFpBA

腾讯优图&港科大提出较大前景运动下的深度高动态范围成像

https://mp.weixin.qq.com/s/WXVZkqCGlj6ym5YrSZS3Vg

谷歌普林斯顿提出首个端到端立体双目系统深度学习方案

https://mp.weixin.qq.com/s/NlYgA-A43q4C155kRdWPAQ

论文复现：谷歌实时端到端双目系统深度学习网络stereonet

https://mp.weixin.qq.com/s/9yfTO2jHz69-k1MsUGIM0Q

双目立体放大！谷歌刚刚开源的这篇论文可能会成为手机双摄的新玩法

https://mp.weixin.qq.com/s/z87Wp3yutq1l5bYfJS2YIA

谷歌新研究用深度学习合成运动模糊效果，手抖也能拍出摄影师级照片

https://mp.weixin.qq.com/s/B5XNmFlSnjEh2xAXB42pHQ

超十亿样本炼就的CNN助力图像质量增强，Adobe推出新功能“增强细节”

https://mp.weixin.qq.com/s/MEjZT_41w2cRqIYDi8a1rw

腾讯优图CVPR中标论文：不靠硬件靠算法，暗光拍照也清晰

https://mp.weixin.qq.com/s/Ek2eNeUPzEJiN6E1Yr6sMw

CVPR2019成像类论文拾英

# LSM

liquid state machine (LSM)

http://www.docin.com/p-390935406.html

基于液体状态机的脑运动神经系统的建模研究

# DNC

https://zhuanlan.zhihu.com/p/27773709

浅析至强RNN可微分神经计算机(DNC)

https://zhuanlan.zhihu.com/p/27964341

浅析至强RNN可微分神经计算机(DNC)-2

https://zhuanlan.zhihu.com/p/28209628

DNC-3滚动分类的模式识别

https://zhuanlan.zhihu.com/p/28433712

DNC4广义线性回归

# Spiking Neuron Networks

除了基于BP算法的NN之外，Spiking Neuron Networks也是一大类NN。Spiking NN和人脑结构更相似，功耗也更小，但是相关训练和数据量化的算法尚不成熟，属于潜力股。

![](/images/img2/BrainChip_Fig2.gif)

![](/images/img3/Tianjic.png)

参考：

https://homepages.cwi.nl/~sbohte/publication/paugam_moisy_bohte_SNNChapter.pdf

Computing with Spiking Neuron Networks

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

# 深度时间序列

https://mp.weixin.qq.com/s/cChJO6YrE_XAgeInS7J1Vw

130页序列推荐系统教程重磅发布

https://mp.weixin.qq.com/s/zv264-dqDQYRkYmjX_QZpQ

郑宇解读地理传感器时间序列预测问题

https://mp.weixin.qq.com/s/JwRXBNmXBaQM2GK6BDRqMw

Kaggle网站流量预测任务第一名解决方案：从模型到代码详解时序预测

https://mp.weixin.qq.com/s/caXnseARUwLIXdsZ7BXOUw

用于金融时序预测的神经网络：可改善移动平均线经典策略

http://mp.weixin.qq.com/s/9fJT0dMLvYQdfMVvSEiG4A

深度学习的时间序列模型评价

https://mp.weixin.qq.com/s/1Gdx-U3DRZtSJoFyCLnD0w

深度学习之Sequence Learning

https://mp.weixin.qq.com/s/SWKWI7BADnX43e-sCBxRPg

神经网络在算法交易上的应用系列——简单时序预测

https://mp.weixin.qq.com/s/Ux7XjLHzlaGFtM1uSMu2gQ

神经网络在算法交易上的应用系列——时序预测+回测

https://mp.weixin.qq.com/s/fUqQo0m7nMLZO85YG-duRw

神经网络在算法交易上的应用系列——多元时间序列

https://mp.weixin.qq.com/s/PS1SqSWckuHuyZP6d5ZUFw

“深度学习”信号处理和时序分析的最后选择？

https://zhuanlan.zhihu.com/p/54471673

基于前馈神经网络的时间序列异常检测算法

https://mp.weixin.qq.com/s/g9upS70qFOCFBMm-T5nI1A

利用深度学习最新前沿预测股价走势

https://mp.weixin.qq.com/s/otwmDtiDfVVID65RQgT4Uw

教你如何鉴别那些用深度学习预测股价的花哨模型？

https://mp.weixin.qq.com/s/xGUcqs3q3yNpVsJ8P7ag_g

以机器学习的视角来看时序点过程的最新进展

https://mp.weixin.qq.com/s/F0z5aEaigQLtlLfDoFIJXQ

时间序列预测：理论与实践教程，300多页PPT带你了解领域最新动态

https://zhuanlan.zhihu.com/p/83130649

深度学习在时间序列分类中的应用

# AI可解释性

XAI(Explainable Artificial Intelligence)

https://github.com/pbiecek/xai_resources

AI可解释性资源汇总

https://mp.weixin.qq.com/s/XVl6voP5cwdC7DcvTMQvVQ

机器学习可解释性工具箱XAI

https://github.com/jphall663/awesome-machine-learning-interpretability

最全的机器学习可解释性资料

https://mp.weixin.qq.com/s/OV4vXu7TAuyV7qU9BAMF6g

机器学习模型的“可解释性”到底有多重要？

https://mp.weixin.qq.com/s/33VQNVvb7JGlk10Jc3mmeg

从可视化到新模型：纵览深度学习的视觉可解释性

https://github.com/ModelOriented/DrWhy

可解释AI(XAI)工具集—DrWhy

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

https://mp.weixin.qq.com/s/ji8sEzJXp1UNgBHVOui0ng

谷歌开源手势识别器，手机能用，还有现成的 App，但是被我们玩坏了

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
