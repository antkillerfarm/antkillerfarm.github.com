---
layout: post
title:  深度学习（三十六）——手势识别, 深度图像压缩, 深度树学习
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

https://mp.weixin.qq.com/s/ji8sEzJXp1UNgBHVOui0ng

谷歌开源手势识别器，手机能用，还有现成的App，但是被我们玩坏了

# 深度图像压缩

Tiny Network Graphics是图鸭科技推出一种基于深度学习的图片压缩技术。由于商业因素，这里没有论文，技术细节也不详，但是下图应该还是有些用的。

![](/images/img2/TNG.png)

还有视频压缩：

论文：

《Deep Learning-Based Video Coding: A Review and A Case Study》

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

# 深度树学习

决策树是传统ML领域的王者，对于如何将之深度化，一般有两个方向：

- 树结构的深度化。代表：gcForest。

- 树+DL。一般被称为深度树学习。

## gcForest

http://mp.weixin.qq.com/s/aDKLcITA6TBZDyNmuAU4Bw

周志华教授gcForest（多粒度级联森林）算法预测股指期货涨跌

https://mp.weixin.qq.com/s/GU9-rH0gFan620Jhc1HTDg

周志华提出的gcForest能否取代深度神经网络？

https://mp.weixin.qq.com/s/dEmox_pi6KGXwFoevbv14Q

周志华：首个基于森林的自编码器，性能优于DNN

http://mp.weixin.qq.com/s/IfEgSOIkIPA-YtC9NQW1ng

非神经网络的深度模型gcForest

https://mp.weixin.qq.com/s/N80l9PZQposbIOKXbv8ayw

周志华：最新实验表明gcForest已经是最好的非深度神经网络方法

https://mp.weixin.qq.com/s/8QP5X9Hxi_6qyfxP4O0Gwg

周志华团队和蚂蚁金服合作：用分布式深度森林算法检测套现欺诈

https://mp.weixin.qq.com/s/bE9BZQ6wCICvrgomdySDuw

周志华组提出可做表征学习的多层梯度提升决策树

https://mp.weixin.qq.com/s/AwvSTF8j0AinS-EgmPFJTA

周志华团队：深度森林挑战多标签学习，9大数据集超越传统方法

## 深度树学习

https://mp.weixin.qq.com/s/GO7bXBY0cVfGIEEAtp0sKg

什么时候以及为什么基于树的模型可以超过神经网络模型？

https://mp.weixin.qq.com/s/bjOVQu0FZyTWQRlwEn8IVA

基于深度树学习的Zero-shot人脸检测识别

https://mp.weixin.qq.com/s/pWcFuOecG-dZHZ365clDjg

阿里妈妈新突破！深度树匹配如何扛住千万级推荐系统压力

https://mp.weixin.qq.com/s/sw16_sUsyYuzpqqy39RsdQ

阿里妈妈深度树检索技术（TDM）及应用框架的探索实践

https://mp.weixin.qq.com/s/EFDmHH8oUmJk-rG5PNnsAg

阿里妈妈深度树匹配技术演进：TDM->JTM->BSAT

https://mp.weixin.qq.com/s/6r8y7tMqo53lnACWG1K4xA

深度树学习用于Zero-shot人脸的反欺诈

https://mp.weixin.qq.com/s/NBVPlFGO12PhMTF0dUL2hw

DeepGBM:使用树蒸馏提升在线预测任务下深度模型效果

# 视频处理+

https://mp.weixin.qq.com/s/cuejj8atJsbSDae9xnhsxA

锁定视频中的目标：港大提出运动注意力检测方法

https://mp.weixin.qq.com/s/AUfaghIcRwwzgZzhOqHVZg

对标GLUE、ImageNet，谷歌推出视觉任务适应性基准VTAB

https://mp.weixin.qq.com/s/sc3ROchBFK9pAb-FCKNwLg

你说我导！微软玩转标题描述生成视频

https://mp.weixin.qq.com/s/jUPhF6OHtVrmTbg29sQV2Q

谷歌提出TVN视频架构：单CPU处理1s视频仅需37ms、GPU仅需10ms

https://mp.weixin.qq.com/s/wwHUjME5vR1uF3ad_wlMzw

更准确的弱监督视频动作定位，从生成注意力模型出发

https://mp.weixin.qq.com/s/18KzXb4hHrWvt3QK2UUm3w

使用深度学习从视频中估计车辆的速度

https://mp.weixin.qq.com/s/vuhoK4cVaHDn6oTuGqX_Eg

你写脚本，AI自动剪视频：13分钟完成剪辑师7小时创作，清华北航联手打造

https://mp.weixin.qq.com/s/IZs9Ctktmvzi_SAU9kD7Gg

多模态人物识别技术及其在爱奇艺视频场景中的应用

https://mp.weixin.qq.com/s/_OTvNrUtbYaFvKlMxFXoDg

时间可以是二维的吗？基于二维时间图的视频内容片段检测

https://mp.weixin.qq.com/s/8N09Argm9sNJRYipq3Mipw

淘宝如何拥抱短视频时代？视频推荐算法实战

https://mp.weixin.qq.com/s/1h9QvZirPa7EZIpOcC_aiw

超清还不够，商汤插帧算法让视频顺滑如丝

https://mp.weixin.qq.com/s/adUW9QuXaJ9uIb7Co5j_gw

视频物体分割算法：如何提升复杂场景的分割精度？

https://mp.weixin.qq.com/s/mASzImKwlpX7BJZFux3Adw

视频预测领域有哪些最新研究进展？

https://mp.weixin.qq.com/s/f2fYwaPNty72CFUIZZPBoA

让电影动漫统统变丝滑，480帧也毫无卡顿，交大博士生开源插帧软件DAIN

https://mp.weixin.qq.com/s/8EQNRIIKQnyoPhUG8ben6A

基于耦合知识蒸馏，速度提升200倍，一款视频显著区域检测新算法

https://mp.weixin.qq.com/s/MjkdwozIXoCJbooQHype7w

视频异常检测：预测未来帧Future Frame Prediction的3个缺陷

https://mp.weixin.qq.com/s/ZVWxpZPCoUvzZH6F0YkjNg

图像视频深度异常检测简明综述论文

https://zhuanlan.zhihu.com/p/114672282

漫谈视频目标跟踪与分割

https://mp.weixin.qq.com/s/oY51Rk6qD7fxeSRIf7HAFg

图像生成玩腻了？视频生成技术何不来了解一下

https://mp.weixin.qq.com/s/sfdhG7Wv3s2XW6yYA1ELQA

基于记忆增强的全局-局部整合网络：更准确的视频物体检测方法

https://mp.weixin.qq.com/s/B5XrN3rxHsu4VrJLgL3bvg

视频分类与行为识别有哪些核心技术，对其进行长期深入学习

https://mp.weixin.qq.com/s/x-fBo5pFD6UlKLh7p68H2A

用机器学习打造计数君，谷歌RepNet可自动计数视频重复片段

https://mp.weixin.qq.com/s/BSaacpLOVhrm94ufaSPtWA

RepNet：对视频中的重复周期进行计数

https://mp.weixin.qq.com/s/9Vt41Ygn767SnpjoXgnEEg

基于语义流的快速而准确的场景解析

https://mp.weixin.qq.com/s/V6UQ8r43p4ULmm4QgjmHSA

OpenCV实现视频稳流

https://mp.weixin.qq.com/s/Wn5k0VoscaiIHHC36_Jm2g

视频目标检测大盘点

https://mp.weixin.qq.com/s/1DA5KoqDskD-tk2Co8n0nQ

阿里-优酷视频增强和超分辨率挑战赛冠军方案：VESR-Net

https://zhuanlan.zhihu.com/p/340568861

预测未来--随机视频生成

https://mp.weixin.qq.com/s/ZIFEom7wpQll_DgE8bUxtA

不同的AI视频推理场景下，如何构建通用高效的抽帧工具？

https://zhuanlan.zhihu.com/p/347705276

MMAction2: 新一代视频理解工具箱

https://zhuanlan.zhihu.com/p/363872795

无监督/自监督的视觉目标跟踪方法

https://mp.weixin.qq.com/s/IuhOLVRgqnoOuxZ2boUGuw

管中窥“视频”，“理解”一斑——视频理解概览

https://mp.weixin.qq.com/s/4fL-6VSpBFgicJ2MAY7twA

视频异常行为检测算法MPN，在多个数据库上达到SOTA

# 迁移学习+

https://zhuanlan.zhihu.com/p/57656210

Deep Domain Adaptation论文集(五)：基于数据重构的迁移方法

https://zhuanlan.zhihu.com/p/57930557

Deep Domain Adaptation论文集(六)：源域与目标域特征空间不一致的处理方法

https://zhuanlan.zhihu.com/p/58514431

Domain Adaptation：不用深度网络，如何处理源域和目标域异构问题？

https://zhuanlan.zhihu.com/p/272508224

Domain Adaptation基础概念与相关文章解读

https://mp.weixin.qq.com/s/7QrIfNXQgSqYC1SOFUOlgQ

对迁移学习中域适应的理解和3种技术的介绍

https://mp.weixin.qq.com/s/e_ltoKzqBhmicwb7vcFcoQ

迁移学习-该做的和不该做的

https://mp.weixin.qq.com/s/Yzbn8B9DsBErt9VbAQTY3w

深度迁移学习方法的基本思路

https://mp.weixin.qq.com/s/CWoKwJ0tyMm10ffgRXLXvg

机器学习模型如何泛化到未知领域？微软亚研“领域泛化 (Domain Generalization)”综述论文概述理论、算法等

https://mp.weixin.qq.com/s/ZuiKVan3EqOQDR1wjH01WA

基于小样本学习的图像分类技术综述
