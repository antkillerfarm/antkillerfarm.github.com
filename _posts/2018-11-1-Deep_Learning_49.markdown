---
layout: post
title:  深度学习（四十九）——Fast Image Processing, DMN, 元学习
category: DL 
---

# 语义分割进阶（续）

https://mp.weixin.qq.com/s/LZNla-NKAu8wmi-OaJD8yA

旷视研究院推出可学习的树状滤波器，实现保留结构信息的特征变换

https://mp.weixin.qq.com/s/lTPm229gzV_9RU-aiY-xQg

2019年5篇图像分割算法最佳综述

https://mp.weixin.qq.com/s/n9Xdj5RKGR9cyXXNzxzvSw

基于深度学习的图像分割在高德的实践

https://zhuanlan.zhihu.com/p/98508347

论文速读：PointRend: Image Segmentation as Rendering

https://blog.csdn.net/sanshibayuan/article/details/103642419

单阶段实例分割（Single Shot Instance Segmentation）

https://mp.weixin.qq.com/s/v_TTwTWx0lu2rJmxvzQQ4g

北航、旷视联合，打造最强实时语义分割网络

# Fast Image Processing

![](/images/article/FIP.png)

上图是照片界常用的几种修图方式之一。一般将这些图片风格转换的算法，称为图像处理算子（image processing operators）。如何加速image processing operators的计算，就成为了学界研究的课题之一。

本文提出的模型就是用来加速image processing operators计算的。它是Intel Lab的Qifeng Chen和Jia Xu于2017年提出的。

论文：

《Fast Image Processing with Fully-Convolutional Networks》

代码：

https://github.com/CQFIO/FastImageProcessing

Demo网站：

http://cqf.io/ImageProcessing/

这个课题一般使用MIT-Adobe FiveK Dataset作为基准数据集。网址：

http://groups.csail.mit.edu/graphics/fivek_dataset/

这个数据集包含了5K张原始照片，并雇用了5个专业修图师，对每张图片进行修图。

众所周知，多层神经网络只要有足够的深度和宽度，就可以任意逼近任意连续函数。然而从Fast Image Processing的目的来说，神经网络的深度和宽度注定是有限的，否则肯定快不了。而这也是该课题的研究意义所在。

本文只使用了MIT-Adobe数据集中的原始图片，并使用了10种常用的算子对图片进行处理。因此，该网络训练时的输入是原始图片，而输出是处理后的图片。

![](/images/article/MCA.png)

上图是本文模型的网络结构图。它的设计特点如下：

1.采用Multi-Scale Context Aggregation作为基础网络。MCA的内容参见《深度学习（十）》中的Dilated convolution一节。

2.传统MCA一般有下采样的过程，但这里由于网络输入和输出的尺寸维度是一样的，因此，所有的feature maps都是等大的。

3.借鉴FCN的思想，去掉了池化层和全连接层。

4.L1~L3主要用于图片的特征提取和升维，而L4~L5则用于特征的聚合和降维，并最终和输出数据的尺寸维度相匹配。

在normalization方面，作者发现有的operators经过normalization之后，精度会上升，而有的精度反而会下降，因此为了统一模型，定义如下的normalization运算：

$$\Psi^s(x)=\lambda_sx+\mu_sBN(x)$$

Loss函数为：

$$\mathcal{l(K,B)}=\sum_i\frac{1}{N_i}\|\hat f (I_i;\mathcal{K,B})-f(I_i)\|^2$$

这实际上就是RGB颜色空间的MSE误差。

为了检验模型的泛化能力，本文还使用RAISE数据集作为交叉验证的数据集。该数据集的网址：

http://mmlab.science.unitn.it/RAISE/

RAISE数据集包含了8156张高分辨率原始照片，由3台不同的相机拍摄，并给出了相机的型号和参数。

# DMN

Question answering是自然语言处理领域的一个复杂问题。它需要对文本的理解力和推理能力。大部分NLP问题都可以转化为一个QA问题。Dynamic Memory Networks可以用来处理QA问题。DMN的输入包含事实输入，问题输入，经过内部处理形成片段记忆，最终产生问题的答案。

DMN可进行端到端的训练，并在多种任务上取得了state-of-the-art的效果：包括QA（Facebook的bAbI数据集），情感分析文本分类（Stanford Sentiment Treebank）和词性标注（WSJ-PTB）。

![](/images/article/DMN.png)

参考：

http://blog.csdn.net/javafreely/article/details/71994247

动态记忆网络

# 元学习

人工智能的历史显示了明确的进展方向：

第一代：**良好的老式人工智能**

- 手工预测
- 什么都不学

第二代：**浅学习**

- 手工功能
- 学习预测

第三代：**深度学习**

- 手工算法（优化器，目标，架构......）
- 端到端地学习功能和预测

第四代：**元学习（Meta-Learning）**

- 无手工
- 端到端学习算法和功能以及预测

## 教程

http://web.stanford.edu/class/cs330/

CS 330: Deep Multi-Task and Meta Learning

http://metalearning.ml

这是一个Meta-Learning方面的专题讨论会，有不少好东西。

https://mp.weixin.qq.com/s/sQmDZsVGIADwO97yEFATkw

ICML2019《元学习》教程与必读论文列表

李宏毅的教程中也有一章介绍Meta-Learning。

## 参考

https://github.com/gopala-kr/meta-learning

元学习（meta-learning）相关文献资源大列表

https://github.com/sudharsan13296/Awesome-Meta-Learning

元学习相关资源汇总

https://github.com/floodsung/Meta-Learning-Papers

另一个元学习相关资源汇总

https://coladrill.github.io/2018/10/24/元学习总览/

元学习总览

https://mp.weixin.qq.com/s/KKK3VEpwL90g6Aro8qtXxQ

学习如何学习的算法：简述元学习研究方向现状

https://mp.weixin.qq.com/s/jlD5p5GXFmrWlxg9xvehxg

元学习—Meta Learning的兴起

https://mp.weixin.qq.com/s/qoKQwEvOnP384i5Z-_jO1A

CVPR2019最新元学习教程：基于元学习的计算机视觉应用

https://mp.weixin.qq.com/s/KtO3OTZ-bZ6m0ZSI6jTyjw

OpenAI提出Reptile：可扩展的元学习算法

https://mp.weixin.qq.com/s/T4GiL9vW7ALOzWloE_QQBA

OpenAI开发可拓展元学习算法Reptile，能快速学习

https://mp.weixin.qq.com/s/MWcoGsQJg1GBbSqzyPD9uQ

基于梯度的元学习算法，可高效适应非平稳环境

https://zhuanlan.zhihu.com/p/35695477

基于Meta Learning在动态竞争环境中实现策略自适应

https://mp.weixin.qq.com/s/AhadWUjtgsFmb8uTylTvqg

OpenAI提出新型元学习方法EPG，调整损失函数实现新任务上的快速训练

https://mp.weixin.qq.com/s/dmRdp2oMn0vGukclJSVZDg

Uber AI论文：利用反向传播训练可塑神经网络，生物启发的元学习范式

https://mp.weixin.qq.com/s/Cc4EHc6ei-PtZWhewM10xw

学习如何学习的算法：简述元学习研究方向现状

https://mp.weixin.qq.com/s/4f6-gXovdrYk7240TrUwJg

谷歌大脑：基于元学习的无监督学习更新规则

https://mp.weixin.qq.com/s/cAbMB-DB9vu2ua8t5J28ww

从零开始，了解元学习

https://mp.weixin.qq.com/s/Q36vpS1HF2IfeCsFLh656A

基于元强化学习的神经科学新理论

https://mp.weixin.qq.com/s/XtzvHOk7CdXRBy02kUmgsg

近期爆火的Meta Learning，遗传算法与深度学习的火花，再不了解你就out了

https://mp.weixin.qq.com/s/KvgYyuyICueNQPo_S27fEA

BAIR展示新型模仿学习，学会像人那样执行任务

https://zhuanlan.zhihu.com/p/41223529

最前沿：Meta RL论文解读

https://zhuanlan.zhihu.com/p/40600485

最前沿：Meta Learning前沿进展扫描

https://zhuanlan.zhihu.com/p/28639662

百家争鸣的Meta Learning/Learning to learn

https://zhuanlan.zhihu.com/p/45845001

最前沿：用模仿学习来学习增强学习

https://zhuanlan.zhihu.com/p/46059552

Meta Learning单排小教学

https://zhuanlan.zhihu.com/p/46131981

最前沿：Meta Learning在少样本文本翻译上的应用

https://zhuanlan.zhihu.com/p/46339823

谈谈无监督Meta Learning的研究

https://zhuanlan.zhihu.com/p/46340382

ICLR19最新论文解读之Meta Domain Adaptation

https://mp.weixin.qq.com/s/RBMGI20AI92ZcWSlYczqAA

伯克利、OpenAI等提出基于模型的元策略优化强化学习

https://mp.weixin.qq.com/s/p0dcov84pZqsU7XP30bexQ

Meta-Learning元学习：学会快速学习

https://mp.weixin.qq.com/s/wl8j7dLu3OxPV7MNaO2-7Q

《基于梯度的元学习》199页伯克利博士论文带你回顾元学习最新发展脉络

https://mp.weixin.qq.com/s/ftiGPBhAx5iqlW_Ltg1yhg

《元监督视觉学习》132页伯克利博士论文带你回顾元监督视觉应用最新发展脉络

https://mp.weixin.qq.com/s/K7sLM-LMcF6-gQrV1ddrDw

让智能体主动交互，DeepMind提出用元强化学习实现因果推理

https://mp.weixin.qq.com/s/8sBXlnXiZNsPRwFsgJVRQQ

谷歌提出元奖励学习，两大基准测试刷新最优结果

https://mp.weixin.qq.com/s/x7uk7jBNvnM7Tgk9lFKy3Q

元学习(Meta-Learning)综述及五篇顶会论文推荐

https://mp.weixin.qq.com/s/GF_NLkSw64_6msmFep81fw

Google Brain ICLR Talk：元学习的前沿与挑战

https://zhuanlan.zhihu.com/p/70782949

最前沿：General Meta Learning

https://zhuanlan.zhihu.com/p/72920138

Meta Learning入门：MAML和Reptile

https://mp.weixin.qq.com/s/MsIAkJAcYHWkkMjzd7qXKA

元学习与强化学习的概率视角，47页ppt，DeepMind牛津Yee Whye Teh

https://mp.weixin.qq.com/s/IdUhvWJYviKtPs9jCbtybA

元知识图谱推理

https://www.zhihu.com/question/291656490

求问meta-learning和few-shot learning的关系是什么？

https://mp.weixin.qq.com/s/LZbprcnben6vPqsoC1DgDA

DeepMind提出元梯度强化学习算法，显著提高大规模深度强化学习应用的性能

https://mp.weixin.qq.com/s/AH35EGTH1YDSx4WzUwY15g

三四行代码打造元学习核心，PyTorch元学习库L2L现已开源

https://github.com/tristandeleu/pytorch-meta

PyTorch上方便好用的元学习工具包

https://mp.weixin.qq.com/s/Fte0SQ7J57AVGyTiIwWKAw

元学习与深度强化学习的机器人应用，84页ppt

https://mp.weixin.qq.com/s/xu5ieaPP2de0GML7b-1BsA

谈谈元学习的技术实现框架

https://mp.weixin.qq.com/s/spRlzjFTh4KeyFfd8pmZgw

新框架ES-MAML：基于进化策略、简易的元学习方法

https://mp.weixin.qq.com/s/uAdFWT5rP40IMsLfFyr7XQ

一种深度网络快速适应的模型无关元学习方法(元学习经典论文)
