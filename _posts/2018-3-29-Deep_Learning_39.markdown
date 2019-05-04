---
layout: post
title:  深度学习（三十九）——视频处理
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

# GAN参考资源

https://blog.csdn.net/liuxiao214/article/category/6940697

某GAN专栏

https://mp.weixin.qq.com/s/oCDlhzjOYTIhsr5JuoRCJQ

IRGAN：大一统信息检索模型的博弈竞争

https://mp.weixin.qq.com/s/4Daw-2aRmzcCMtxdvB3uYQ

IRGAN：生成对抗网络在搜狗图片搜索排序中的应用

https://mp.weixin.qq.com/s/PkZ069S8ysY_JCQx1nzfGg

NVIDIA新作解读：用GAN生成前所未有的高清图像（附PyTorch复现）

https://mp.weixin.qq.com/s/Q_1IUS-65ZAFt9w0RlZUpw

谷歌开源TFGAN：轻量级生成对抗网络工具库

https://mp.weixin.qq.com/s/E28lA-fcAQ6Sp6Qv64H3TQ

GAN in NLP

https://mp.weixin.qq.com/s/7-oHa-8Q8ThcctaVOZFfew

Facebook创意生成网络CAN，比GAN更有创造力

http://mp.weixin.qq.com/s/UkZdUcdz7h4DqcyjSbNncw

zi2zi：用条件生成对抗网络玩转中文书法，绝妙汉字字体自动生成

http://blog.csdn.net/v_JULY_v/article/details/52683959

没GPU也能玩梵高作画：Ubuntu tensorflow CPU版

http://mp.weixin.qq.com/s/zNmJuevHaagKbyGFdKTwoQ

tensorflow实现基于深度学习的图像补全

https://zhuanlan.zhihu.com/p/27199954

用GAN去除动作片中的马赛克和衣服

https://mp.weixin.qq.com/s/Qzlg1MzRT3josy2RJpQSVg

Image to Image Translation Using GAN

https://mp.weixin.qq.com/s/AswdyjPeKbX7yhAPloP2og

基于对抗学习的生成式对话模型

https://mp.weixin.qq.com/s/YXWTslQXIKVihBb2Bgtafg

GAN在信息检索领域的应用

https://mp.weixin.qq.com/s/0tTLotV-8w2j3VdkH-qjCQ

让机器告诉你故事的结局应该是什么：利用GAN进行故事型常识阅读理解

https://zhuanlan.zhihu.com/p/28488946

AI可能真的要代替插画师了……

https://mp.weixin.qq.com/s/OXN8Y5truLeslX8-UWgqmg

宅男的福音：用GAN自动生成二次元萌妹子

https://mp.weixin.qq.com/s/NFqTpSXtFdP43MBeb3Ovrw

GAN眼中的图像翻译

http://blog.csdn.net/amds123/article/details/70199708

生成对抗网络GAN最近在NLP领域有哪些应用？

https://mp.weixin.qq.com/s/W4WVHMIMKPNvt5lcCe1v4g

半监督学习需要Bad GAN，清华特奖学霸与苹果AI总监提出

https://mp.weixin.qq.com/s/jG7r7THhVAnJTYgDeWX2Eg

百度提出使用GAN构建语音识别新框架

https://mp.weixin.qq.com/s/I7axl00VZeCHJpaaITCO2Q

对抗学习GAN提升跨模态检索效果

https://mp.weixin.qq.com/s/OZWih_xfqYdeP-x9MawogA

GAN做图像翻译的一点总结

https://zhuanlan.zhihu.com/p/32103958

GAN调研：多极扩展（跨域和条件的GAN扩展模型调研）

https://mp.weixin.qq.com/s/RZ6UbTgfn30UFaUYNtApRg

如何用Masking GAN让100,000人笑起来？

https://mp.weixin.qq.com/s/OPn4_jmJ7pUg8xa0FZvyiw

超火的漫画线稿上色AI出新版了！无监督训练，效果更美好

https://mp.weixin.qq.com/s/wrxKwY2SvJZc8bRuZ_wF_A

用生成对抗网络给雪人上色，探索人工智能时代的美学

https://mp.weixin.qq.com/s/lPeySqEr3pzPdZqnp1Eh8A

CMU提出对抗生成网络：可实现对人脸识别模型的神经网络攻击

https://mp.weixin.qq.com/s/f5dGUJw3bv8WppPZVzqyAA

反GAN传统，Petuum自动驾驶新研究提出从复杂真实图像生成简单虚拟表征以预测驾驶指令

https://mp.weixin.qq.com/s/FXKWP4OlUsprSDgU7ue8CA

OCGAN: 使用具有约束潜在表示的GAN的一类新颖性检测

https://mp.weixin.qq.com/s/SvIC6DXUKH93jU9R-7fcyw

CNN对抗补丁之谜

https://mp.weixin.qq.com/s/V1XX8WWstR00m_OimMrDDw

微软亚洲研究院论文解读：GAN在网络特征学习中的应用

https://mp.weixin.qq.com/s/YNkx03D2Ok2xyMpPI0XXKg

Petuum新研究提出形义感知型Grad-GAN：可基于虚拟游戏生成更具真实感的城市场景

http://mp.weixin.qq.com/s/RlpI3hCKjcJ8VHSbBXf84g

用生成对抗网络解决NLP问题：谷歌大脑提出MaskGAN

https://mp.weixin.qq.com/s/gDzti2DISq_cwGbP5T7ICQ

聊聊对抗自编码器

https://mp.weixin.qq.com/s/nuT6Glyx0-tU7WJyoPji9w

GANs正在多个层面有所突破

https://mp.weixin.qq.com/s/cI4xZOw6eL0w9sz9Q2mSCw

Ian Goodfellow盛赞：一个GAN生成ImageNet全部1000类物体

https://mp.weixin.qq.com/s/BCA7MmYnivuGbwyjHqDQUw

手把手教你实现GAN半监督学习

https://zhuanlan.zhihu.com/p/32135185

从一篇ICLR'2017被拒论文谈起：行走在GAN的Latent Space

https://mp.weixin.qq.com/s/SPAbCZloiHp2mtGu0IDWDA

DeepMind高级研究员：重新理解GAN，最新算法、技巧及应用

https://mp.weixin.qq.com/s/EI57G3ph9WCrT7kudOAjvQ

Petuum提出对偶运动生成对抗网络：可合成逼真的视频未来帧和流
