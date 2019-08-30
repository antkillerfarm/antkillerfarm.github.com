---
layout: post
title:  深度学习（三十）——自动求导, 深度哈希, 深度贝叶斯学习, 手势识别
category: DL 
---

# Graph NN（续）

https://mp.weixin.qq.com/s/XApSbi-Pg-AeYGkPN3fldg

旷视研究院提出ML-GCN：基于图卷积网络的多标签图像识别模型

https://mp.weixin.qq.com/s/49vnVOO0G_JvKrWcsN2_Ww

关系图注意力网络-Relational Graph Attention Networks

https://mp.weixin.qq.com/s/rvcj9-6KlBsVmF_CAsip2A

超越标准GNN！DeepMind、谷歌提出图匹配网络

https://mp.weixin.qq.com/s/UotqgRjCTpjPrsIEWBRPxA

基于随机游走的图匹配算法

https://mp.weixin.qq.com/s/t2kjxrcn6O9tbJ-IQELboQ

高君宇：图神经网络在视频分类中的应用

https://mp.weixin.qq.com/s/7DyPJ9LnqZ9XyAop33SxSw

ST-GCN动作识别算法详解

https://mp.weixin.qq.com/s/fxVsN2dDmayxJfxBRIXHhQ

解读PingSage：图卷积神经网络在数十亿数据网络级别推荐系统的应用

https://mp.weixin.qq.com/s/SWcJut6QqOvbziirxTd2Kg

斯坦福教授ICLR演讲：图网络最新进展GraphRNN和GCPN

https://mp.weixin.qq.com/s/Lakq83_ngUJf1ES3N7J9_g

图卷积在基于骨架的动作识别中的应用

https://mp.weixin.qq.com/s/5wSgC4pXBfRLoCX-73DLnw

什么是图卷积网络？行为识别领域新星

https://mp.weixin.qq.com/s/1-Dmckby2NcXsaoK08zk8w

视频理解中的图表示学习

https://mp.weixin.qq.com/s/lyy3AhqLDBT88B2LSSIbZQ

图表示解决长文本关系匹配问题：腾讯提出概念交互图算法

https://mp.weixin.qq.com/s/bvp3NIrrarJc_MesKy1x_A

崔泽宇：套装搭配推荐在图神经网络上的应用

https://mp.weixin.qq.com/s/_8K0s9WceJ-xlRViHhz2Zw

Google图挖掘团队最新博客《图表示学习中的创新》

https://mp.weixin.qq.com/s/c3SBGlxzJOYhQBrJ2h3j0g

呼奋宇：深度层次化图卷积神经网络

https://mp.weixin.qq.com/s/YPV2BR6eayKUlPazUeZVnQ

何时能懂你的心——图卷积神经网络（GCN）

https://mp.weixin.qq.com/s/sRKW8DLXZXWLUUVTb12F4Q

“AI新贵”图神经网络算法及平台在阿里的大规模实践

https://mp.weixin.qq.com/s/tAfTmGWqG6IR8SOP0uKW6g

什么限制了GNN的能力？首篇探究GNN普适性与局限性的论文出炉！

https://mp.weixin.qq.com/s/zOdy-1vCJD_dPFSoe0ELFA

图论与图学习（一）：图的基本概念

https://mp.weixin.qq.com/s/0ZdS1WOSDZiXnxP8fybBAw

图论与图学习（二）：图算法

https://mp.weixin.qq.com/s/Orv47r4EchVIR7VcleoJ5Q

谷歌图表征学习创新：学习单个节点多个嵌入&自动学习最优超参数

https://mp.weixin.qq.com/s/sJB4N_ObUqKM8H65yU_1sg

Graph基础知识介绍

https://mp.weixin.qq.com/s/jBQOgP-I4FQT1EU8y72ICA

图神经网络的“开山之作”CGN模型

https://mp.weixin.qq.com/s/DJAimuhrXIXjAqm2dciTXg

何时能懂你的心——图卷积神经网络（GCN）

https://mp.weixin.qq.com/s/DNePTCpyjrlZEixw5L7w5A

GraphSAGE：我寻思GCN也没我牛逼

https://mp.weixin.qq.com/s/1DHvLLysMU24dBeLzbSpUA

GraphSAGE

https://mp.weixin.qq.com/s/IcLk-fMjKO19BaHbuUCeXg

GraphSAGE 算法原理，实现和应用

https://mp.weixin.qq.com/s/C-Pa1jznQntyhocdxS-4Hg

节点嵌入训练加快300倍！解读开源高性能图嵌入系统GraphVite

https://mp.weixin.qq.com/s/edrh-HXqW01Yx7c8tQ8UxA

从数据结构到算法：图网络方法初探

https://mp.weixin.qq.com/s/9MWoCmtKPPVs3Rmko-7adQ

10亿节点异构网络中，GCN如何应用？

https://mp.weixin.qq.com/s/ftz8E5LffWFfaSuF9uKqZQ

Graph Neural Network：GCN算法原理，实现和应用

https://mp.weixin.qq.com/s/JvtrGa0YiUmR6UA5wBQ-pQ

图神经网络GNN最新理论进展和应用探索

https://mp.weixin.qq.com/s/zQU47tjpTCPiLdEmUmZx3Q

图卷积神经网络及其应用

# 自动求导

DL发展到现在，其基本运算单元早就不止CNN、RNN之类的简单模块了。针对新运算层出不穷的现状，各大DL框架基本都实现了自动求导的功能。

论文：

《Automatic Differentiation in Machine Learning: a Survey》

## Numerical differentiation

数值微分最大的特点就是很直观，好计算，它直接利用了导数定义：

$$f'(x)=\lim_{h\to 0}{f(x+h)-f(x)\over h}$$

不过这里有一个很大的问题：h怎么选择？选大了，误差会很大；选小了，不小心就陷进了浮点数的精度极限里，造成舍入误差。

第二个问题是对于参数比较多时，对深度学习模型来说，上面的计算是不够高效的，因为每计算一个参数的导数，你都需要重新计算$$f(x+h)$$。

因此，这种方法并不常用，而主要用于做梯度检查（Gradient check），你可以用这种不高效但简单的方法去检查其他方法得到的梯度是否正确。

## Symbolic differentiation

符号微分的主要步骤如下：

1.需要预置基本运算单元的求导公式。

2.遍历计算图，得到运算表达式。

3.根据导数的代入法则和四则运算法则，求出复杂运算的求导公式。

这种方法没有误差，是目前的主流，但遍历比较费时间。

## Automatic differentiation

除此之外，常用的自动求导技术，还有Automatic differentiation。（请注意这里的AD是一个很狭义的概念。）

类比复数的概念：

$$x = a + bi \quad (i^2 = -1)$$

我们定义Dual number：

$$x \mapsto x = x + \dot{x} d \quad (d^2=0)$$

定义Dual number的运算法则：

$$(x + \dot{x}d) + ( y + \dot{y}d) = x + y + (\dot{x} + \dot{y})d$$

$$(x + \dot{x}d) ( y + \dot{y}d) = xy + \dot{x}yd + x\dot{y}d  +  \dot{x}\dot{y}d^2 = xy + (\dot{x}y+ x\dot{y})d$$

$$-(x + \dot{x}d) = - x - \dot{x}d$$

$$\frac{1}{x + \dot{x}d} = \frac{1}{x} - \frac{\dot{x}}{x^2}d$$

dual number有很多非常不错的性质。以下面的指数运算多项式为例：

$$f(x) = p_0 + p_1x + p_2x^2 + ... + p_nx^n$$

用$$x + \dot{x}d$$替换x，则有：

$$f(x + \dot{x}d) =   p_0 + p_1(x + \dot{x}d) + ... +  p_n(x + \dot{x}d)^n \\ 
= p_0 + p_1x + p_2x^2 + ... + p_nx^n + \\ 
p_1\dot{x}d + 2p_2x\dot{x}d + ... + np_{n-1}x\dot{x}d\\ 
= f(x) + f'(x)\dot{x}d$$

可以看出d的系数就是$$f'(x)$$。

## 不可导函数的求导

不可导函数的求导，一般采用泰勒展开的方式。典型的算法有PGD（Proximal Gradient Descent）。

参考：

https://blog.csdn.net/bingecuilab/article/details/50628634

Proximal Gradient Descent for L1 Regularization

## 参考

https://mp.weixin.qq.com/s/7Z2tDhSle-9MOslYEUpq6g

从概念到实践，我们该如何构建自动微分库

https://mp.weixin.qq.com/s/bigKoR3IX_Jvo-re9UjqUA

机器学习之——自动求导

https://www.jianshu.com/p/4c2032c685dc

自动求导框架综述

https://mp.weixin.qq.com/s/xXwbV46-kTobAMRwfKyk_w

自动求导--Deep Learning框架必备技术二三事

https://mp.weixin.qq.com/s/f0xFfA1inOVOdJnSZR4k6Q

自动微分技术

https://mp.weixin.qq.com/s/0tTlPG4hd9hcHORkZF6w1A

PyTorch的自动求导机制详细解析，PyTorch的核心魔法

# 深度哈希

https://mp.weixin.qq.com/s/iVKnLyNJGVRsR5fWc92Rwg

深度离散哈希算法，可用于图像检索！

https://mp.weixin.qq.com/s/XUYJub0559wwQ9H1wA_SAg

机器学习时代的哈希算法，将如何更高效地索引数据

https://mp.weixin.qq.com/s/vFBlFAQLvDZP7IvwKoaPhA

无问西东，只问哈希

https://mp.weixin.qq.com/s/XAxuLg2i3q5_uKDo1wU_rA

从哈希到卷积神经网络：高精度&低功耗

https://mp.weixin.qq.com/s/i8iQtCC7ahXLY1a1wOacsA

Science：最新发现哈希可能是大脑的通用计算原理

https://mp.weixin.qq.com/s/ZOVWXNym5yHoo-MmpxXo0A

自监督对抗哈希SSAH：当前最佳的跨模态检索框架

https://mp.weixin.qq.com/s/VldzlYg5AfDRho8bsROL_g

HashGAN:基于注意力机制的深度对抗哈希模型提升跨模态检索效果

https://mp.weixin.qq.com/s/3Z2Zc8zTq2uiPyw7ZuuZfw

解密美图大规模多媒体数据检索技术DeepHash

# 深度贝叶斯学习

https://mp.weixin.qq.com/s/pHAbxeYBI2q6pUHNrAt1og

贝叶斯学习与未来人工智能

https://mp.weixin.qq.com/s/Zd4rFU7Lebr4zmzxThNyVw

详解珠算：清华大学开源的贝叶斯深度学习库

https://mp.weixin.qq.com/s/RpaOrngeXTKycLb3iCygZw

利用贝叶斯神经网络进行随机动力系统中的学习与策略搜索

https://mp.weixin.qq.com/s/lKm_ypn5I7tSjoQHceJ0jQ

概率编程：使用贝叶斯神经网络预测金融市场价格

https://mp.weixin.qq.com/s/cDqxmRVQCIqdM5oiUh82YQ

Yee Whye Teh：《贝叶斯深度学习与深度贝叶斯学习》

https://mp.weixin.qq.com/s/Zk2YG-IJNhJxTBU8THSM-g

让DL可解释？这一份66页贝叶斯深度学习教程告诉你

https://mp.weixin.qq.com/s/-izo9VUdxN33pwVFGV_tjw

299页PPT带你回顾深度贝叶斯学习最新发展脉络

https://github.com/bayesgroup/deepbayes-2018

Seminars DeepBayes Summer School 2018

https://mp.weixin.qq.com/s/WCRYppBLdl_M4etUChnfgw

PyMC3和Theano代码构建贝叶斯深度网络

https://mp.weixin.qq.com/s/7mwJpQFWWXJ3dvTAwDFI7Q

贝叶斯卷积神经网络：架起深度学习与统计学的桥梁

https://mp.weixin.qq.com/s/2LkpuchuHs82Sxs5rD8bWA

《深度贝叶斯与序列学习》，279页PPT带你知晓深度贝叶斯序列模型在NLP最新进展

https://zhuanlan.zhihu.com/p/74573041

针对推荐系统的深度贝叶斯多目标学习

https://mp.weixin.qq.com/s/b041h_hbHQYiXCiDHGaD5w

深度贝叶斯自然语言处理，304页ppt带你了解最新研究进展

https://zhuanlan.zhihu.com/p/77140176

构建贝叶斯深度学习分类器

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
