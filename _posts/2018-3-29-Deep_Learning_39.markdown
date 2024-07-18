---
layout: post
title:  深度学习（三十九）——手势识别, 深度压缩感知, 深度树学习
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

# OpenCV+

https://mp.weixin.qq.com/s/pG5nq1fQ9XHp8WZN1AiLJQ

OpenCV基于Inception模型图像分类

https://mp.weixin.qq.com/s/EqeNS6s72_Qwg6mmDRl6wQ

OpenCV基于DLCO描述子匹配

https://mp.weixin.qq.com/s/JVuxMUmN2_DNfg1Ol5b8cw

OpenCV3新特性-图像无缝克隆函数演示

https://mp.weixin.qq.com/s/O3o8W1KSsZ0rz7aVukDWEg

使用OpenCV测量图像中物体之间的距离

https://mp.weixin.qq.com/s/HmOiQnkaqxcFPOODnOeUkw

使用OpenCV检测坑洼

https://mp.weixin.qq.com/s/BTmozO6Yr-Jsfm4-YXh2Mg

基于OpenCV的图像梯度与边缘检测

https://mp.weixin.qq.com/s/QYnXiAMFC3k_wQIwiaeWQg

三行代码，OpenCV轻松生成19种色彩风格图像

https://mp.weixin.qq.com/s/4LQBY0rMJk0tU8lF3fgHfQ

基于OpenCV的图像阴影去除

https://mp.weixin.qq.com/s/kH6K6L6-BYnYE-g46WhNCg

在OpenCV中使用色彩校正

https://mp.weixin.qq.com/s/K4P8151BuM4DQPSK-KzPFQ

OpenCV图像旋转的原理与技巧

https://mp.weixin.qq.com/s/hGONFisdtwcF5CRSB9IF6g

图像处理基础：颜色空间及其OpenCV实现

https://mp.weixin.qq.com/s/0Z600IIGscgrREgLUqjLuA

手把手教你用Python做一个图像融合demo

https://mp.weixin.qq.com/s/vo1v5dYGLMqzCSUG9gPvag

使用OpenCV进行图像编辑--绘画和素描

https://mp.weixin.qq.com/s/EkxXV7Bizf4JxG15SQb79w

修改OpenCV一行代码，提升14%图像匹配效果（BEBLID(Boosted Efficient Binary Local Image Descriptor)是一个2020年才开发出来的算子）
