---
layout: post
title:  深度学习（三十九）——视频处理, 多模态学习
category: DL 
---

# 视频处理

## 视频目标分割

视频目标分割任务和语义分割有两个基本区别：

1.视频目标分割任务分割的是一般的、非语义的目标；

2.视频目标分割添加了一个时序模块：它的任务是在视频的每一连续帧中寻找感兴趣目标的对应像素。

![](/images/article/Segmentation.png)

上图是Segmentation的细分，其中的每一个叶子都有一个示例数据集。

基于视频任务的特性，我们可以将问题分成两个子类：

无监督（亦称作视频显著性检测）：寻找并分割视频中的主要目标。这意味着算法需要自行决定哪个物体才是“主要的”。

半监督：在输入中（只）给出视频第一帧的正确分割掩膜，然后在之后的每一连续帧中分割标注的目标。

## 参考

http://mp.weixin.qq.com/s/pGrzmq5aGoLb2uiJRYAXVw

一文概览视频目标分割

https://www.zhihu.com/question/52185576

视频中的目标检测与图像中的目标检测具体有什么区别？

https://mp.weixin.qq.com/s/noljXreGfoMfiZb_n90R3w

模仿人类的印象机制，商汤提出精确实时的视频目标检测方法

http://mp.weixin.qq.com/s/-Av3-ZNi6UGlKNv_jduAeQ

微软新论文：如何利用深度特征流提高视频识别准确率？

https://mp.weixin.qq.com/s/z1APyCxlOEPHn48OeJAHkQ

基于深度学习的视频内容识别

https://mp.weixin.qq.com/s/WMakTEN68KPi7X9kMQetiw

OpenAI:3段视频演示无人驾驶目标检测强大的对抗性样本！

https://mp.weixin.qq.com/s/j5YPHYEPioLiEIDc6lK3kA

在线视频衣物精确检索技术，开启刷剧败明星同款时代

https://mp.weixin.qq.com/s/CXKuSMi0Vd43BGDf5BgoqA

弱监督视频物体识别新方法：香港科技大学联合CMU提出TD-Graph LSTM

https://mp.weixin.qq.com/s/nZVIVJ9z0AWA8VFouNpafg

深度学习之视频摘要简述

https://mp.weixin.qq.com/s/7ccEaDRngVo42OSU6FBlVg

从视频到语句，优必选获TRECVID 2017子任务冠军

https://mp.weixin.qq.com/s/XorPkuIdhRNI1zGLwg-55A

斯坦福新深度学习系统 NoScope：视频对象检测快1000倍

https://mp.weixin.qq.com/s/7w5nYWfZO-YOJ4cA47kQXg

无监督视频物体分割新思路：实例嵌入迁移

https://mp.weixin.qq.com/s/PhMPa-e4sbzqWKmFzRZE4Q

实时替换视频背景：谷歌展示全新移动端分割技术

https://mp.weixin.qq.com/s/ovjoHCcR1xYb9N6kyFJUTg

视频广告段落检测——从一个偏门说计算机视觉的发展历史

https://mp.weixin.qq.com/s/0JgwBizaCwvPP9TfLKTang

密歇根大学&谷歌提出TAL-Net：将Faster R-CNN泛化至视频动作定位中

http://mp.weixin.qq.com/s/LAgDobWyK0SOH08GCLXG7A

减少30%流量，增加清晰度：MIT提出人工智能视频缓存新算法

https://mp.weixin.qq.com/s/_ZmbwM-lmS0o2DjAAc_TWQ

美图云+中科院AAAI2018：视频语义理解的类脑智能NOASSOM

https://mp.weixin.qq.com/s/iqLHjbmLOmvfEeEUB_SqSA

计算机视觉视频理解领域的经典方法和最新成果

https://mp.weixin.qq.com/s/LzKsD_vFlA1n-TYOGJkDZg

商汤科技开源DAVIS2017视频目标分割冠军代码

https://mp.weixin.qq.com/s/FiAju9F_MWexstP7FrIquw

凭一张照片找到视频中你所有的镜头，包括背影

https://mp.weixin.qq.com/s/3H0ZJjnPsh1BzALmG0W7og

DAVIS2017视频目标分割冠军代码开源了

https://mp.weixin.qq.com/s/ZqnfSL6U5E9NzE15QMdxtg

腾讯AI Lab提出视频再定位任务，准确定位相关视频内容

https://mp.weixin.qq.com/s/6MXLtUDi_idMYqbHARkbcg

港中文林达华团队提出计算机视觉新方向：电影情节分析

https://mp.weixin.qq.com/s/Np4xyvPrncd7MJ9q1WShBA

Python视频深度学习：计算任意影片中所有演员出镜时间

https://mp.weixin.qq.com/s/Nt4QLX_lbHhszb8fFlmOLA

DeepVS：基于深度学习的视频显著性方法

https://mp.weixin.qq.com/s/NgsSQS6opjOsusTIr9Vx-w

腾讯AI Lab、MIT等机构提出TVNet：可端到端学习视频的运动表征

https://mp.weixin.qq.com/s/26OZ5sLK3floF8I1SNIKuA

时空建模新文解读：用于高效视频理解的TSM 

https://mp.weixin.qq.com/s/TzNqZNEPBewR7neU7Or9nQ

更侧重工业的应用：PRCV2018美图短视频实时分类挑战赛冠军技术方案

https://mp.weixin.qq.com/s/lBu1q5Pyw9dZIxSYXUp2pw

视频语义分割介绍

https://mp.weixin.qq.com/s/qtRV9Sb54o8TnDEhLlB69Q

基于视频的目标检测的发展

https://mp.weixin.qq.com/s/MzVPesFK0vJ1UuQPPSSN2w

百度、MIT等提出StNet：局部+全局的视频时空联合建模

https://mp.weixin.qq.com/s/UeQc3orm2ooZ5zlvrSLzOw

视频内容理解在Hulu的应用与实践

https://mp.weixin.qq.com/s/syZObdxjPv6jq3B_mgP9Sw

拒绝“不可描述”！爱奇艺短视频软色情识别技术解析

https://mp.weixin.qq.com/s/8YpyfdhDypSZOP3dQegQdQ

谷歌大脑提出基于流的视频预测模型，可产生高质量随机预测结果

https://mp.weixin.qq.com/s/-FF3tuEB2V8RlQCjQhu5Bg

人大ML研究组提出新的视频测谎算法

https://mp.weixin.qq.com/s/T-Rg9xLfdYmV8bJESK0h8g

快速端到端嵌入学习用于视频中的目标分割

https://mp.weixin.qq.com/s/pKSrokV_j8Repa-JMloUHg

Google又发大招：高效实时实现视频目标检测

https://mp.weixin.qq.com/s/ySAfdII8291hvTxUBtE5qA

详解爱奇艺ZoomAI视频增强技术的应用

https://mp.weixin.qq.com/s/l6WMJnrGNNK4A1cTD2drcg

视频跟踪新思路，完全无需手工标注。这是一篇Visual Tracking和Optical Flow Estimation相互结合的论文

https://zhuanlan.zhihu.com/p/59915784

Video Action Recognition的近期进展

https://mp.weixin.qq.com/s/NQSJvlcjOAoIZjI2cgjhkw

谷歌AI：根据视频生成深度图，效果堪比激光雷达

https://mp.weixin.qq.com/s/k4Ilj11wbuj5oIXLHo-5ew

《视频目标分割与跟踪》最新39页综述论文

https://mp.weixin.qq.com/s/fxKHMVRYCR9CycifjEnArQ

视频显著性目标检测

https://mp.weixin.qq.com/s/oSoCIAEF78iKIxLxj2H1mA

基于光流的视频目标检测系列文章解读

https://mp.weixin.qq.com/s/1tcoGGbJnnWARu-2wefWdQ

不同视角构造cycle-consistency，降低视频标注成本

https://mp.weixin.qq.com/s/pDMBnX3CeQbv8hr-79Mvvg

商汤EDVR算法获NTIRE 2019视频恢复比赛全部四项冠军

https://mp.weixin.qq.com/s/GCCqIm4Q7UfUhhiqFbBS3g

Pytorch视频分类教程

https://mp.weixin.qq.com/s/ua8V2g2uZAditKui-IcoKw

物体检测算法在视频中的应用

https://mp.weixin.qq.com/s/BtIYc7SSi0E6mT3muV6NhQ

视频编辑利器，不喜欢就框除！开源视频物体移除软件video object removal

https://mp.weixin.qq.com/s/YwZijgTbhd2ILiVxug1dWg

框一下就能从视频隐身，这是现实版的“隐身衣”？

https://zhuanlan.zhihu.com/p/73599727

基于深度学习的视频帧内插技术

https://mp.weixin.qq.com/s/PnDjXF_ZPYPJ3cgZ_w6v-g

视频分类/行为识别网络和数据集上新

https://mp.weixin.qq.com/s/cNvQy4MW9vHTbUPsrqnUdA

视频PS神器！人物隐身、水印去除，简直像重拍了一遍，这项登上CVPR的研究刚刚开源了

https://mp.weixin.qq.com/s/umLqkfSDCBUGaEa0yygAIw

有了这款DVD-GAN，DeepMind就生成了逼真视频

https://mp.weixin.qq.com/s/7ygF4zxZxp0Iz7KRDdWebg

R-C3D视频活动检测

https://mp.weixin.qq.com/s/pfAVyS_JQLqsXLYdX2m8IQ

基于深度学习的视频目标检测综述

https://mp.weixin.qq.com/s/N6nL9D1wjsBNzxedXX3qNQ

Hulu在视频内容理解上的应用和最佳实践

https://mp.weixin.qq.com/s/0z6jrEYTdIUSOYzW3gA9FQ

ActivityNet 挑战赛冠军方案—时序动作提名，边界匹配网络详解

https://mp.weixin.qq.com/s/r47N4HVAydNeBOTtP-iruQ

阿里文娱资深算法专家：视频物体分割算法的三个方向与最新应用

https://zhuanlan.zhihu.com/p/75444151

时序动作提名：边界匹配网络详解

https://mp.weixin.qq.com/s/8x1ne425aRb2aqP8dSrClg

玩王者荣耀用不好英雄？两阶段算法帮你精准推荐精彩视频

https://mp.weixin.qq.com/s/YL08krIuf0WOz8KsoBYhew

25个视频神同步，还能给视频声音移花接木，谷歌开源最新自监督算法

https://mp.weixin.qq.com/s/THUjd-wXI6_57mdw4XfYUQ

商汤-港中文MMLab开源图像视频超分辨率工具箱MMSR

https://mp.weixin.qq.com/s/NcA178JxgZLrAE1efBG2dA

深度神经网络在视频编码中的应用

https://mp.weixin.qq.com/s/XZ-fn_Xd7vVoLiVr6Esb3g

Ranking Attention-适用于视频分割的全新Attention机制

https://zhuanlan.zhihu.com/p/79719747

Super SloMo,基于光流和U-Net的视频插值

https://mp.weixin.qq.com/s/8uxeOqcUvnZcKHv7qXwWJQ

如何在视频里任意抠图？阿里工程师做到了！

https://zhuanlan.zhihu.com/p/83604007

Deep Burst Denoising/RFCN

https://mp.weixin.qq.com/s/dl8kbhKUCHzwJ-5lSv6VMw

高清彩色开国典礼将上映：技术加持之下，历史不会褪色

https://mp.weixin.qq.com/s/-lNM3bVxK8YtmPWdXpSf5A

UC信息流视频标签识别技术

# 多模态学习

https://mp.weixin.qq.com/s/ruRkqBEdyj2Dx0WTO5Jhcw

多模态学习研究进展综述

https://mp.weixin.qq.com/s/vpBPkjuCebSWh5qPLYHCkw

上海交大提出多模态框架“EmotionMeter”，更精准地识别人类情绪

https://mp.weixin.qq.com/s/BBg04rDtiqU-XrWortufNA

康奈尔&英伟达提出多模态无监督图像转换新方法

http://mp.weixin.qq.com/s/khOINUyrNV3TFfgNRheH0A

卷积神经网络压缩、多模态的语义分析研究

https://mp.weixin.qq.com/s/ywU4L659iRcmIgmV6RtbXA

DeepMind新研究连接听与看，实现“听声辨位”的多模态学习

https://mp.weixin.qq.com/s/1qhcyTXttgKWlw-Oy556Tw

TPAMI2019最新《多模态机器学习综述》

https://mp.weixin.qq.com/s/BczgUuh2FIvP5MG9xh87wQ

多模态多任务学习新论文

https://mp.weixin.qq.com/s/ipj8qpYRiYbIeXn2PZb1SQ

5G时代下多模态理解做不到位注定要掉队

https://mp.weixin.qq.com/s/UghgWBN7mE8oJdMUvjAjcQ

何晖光：多模态情绪识别及跨被试迁移学习
