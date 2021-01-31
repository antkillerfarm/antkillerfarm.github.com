---
layout: post
title:  深度学习（四十）——行人重识别, 图像检索
category: DL 
---

* toc
{:toc}

# 行人重识别

行人重识别（Person re-identification）也称行人再识别，是利用计算机视觉技术判断图像或者视频序列中是否存在特定行人的技术。广泛被认为是一个图像检索的子问题。给定一个监控行人图像，检索跨设备下的该行人图像。旨在弥补目前固定的摄像头的视觉局限，并可与行人检测/行人跟踪技术相结合，可广泛应用于智能视频监控、智能安保等领域。

参考：

https://github.com/gjy3035/Awesome-Crowd-Counting

人群计数最全代码、数据、论文合集

https://zhuanlan.zhihu.com/c_1111215695622352896

一个人群计数方面的专栏

https://mp.weixin.qq.com/s/ZmX_ir1pSUZbCaFpbcQ6Lw

一文读懂行人检测算法

https://mp.weixin.qq.com/s/a4zRYKIJUNatTW9YJZtu2Q

关注度越来越高的行人重识别，有哪些热点？

https://zhuanlan.zhihu.com/p/26168232

行人重识别：从哈利波特地图说起

https://zhuanlan.zhihu.com/p/50387521

从零开始行人重识别

https://mp.weixin.qq.com/s/iAMjUHK5ZrVEGVliA0JE9Q

行人Re-ID研究概述

https://mp.weixin.qq.com/s/T-Odxp4K1E0I7Gq-fjCM7g

行人跟踪算法及应用综述

https://mp.weixin.qq.com/s/_NDw7pFmDB07mliHTA6VYQ

旷视行人再识别（ReID）突破

https://zhuanlan.zhihu.com/p/31181247

从人脸识别到行人重识别，下一个风口

https://mp.weixin.qq.com/s/QeNcO_JahvkscxWnd50xLw

最新《深度学习行人重识别》综述论文，24页pdf

https://mp.weixin.qq.com/s/Dy3_I27fa1j7ZdweoPXImw

深度学习行人重识别ReID最新综述与展望

https://mp.weixin.qq.com/s/zRdJktyk1LZWUd2cyTjpiw

基于图像检索的行人重识别

https://zhuanlan.zhihu.com/p/31921944

基于深度学习的行人重识别研究综述

https://zhuanlan.zhihu.com/p/31473785

行人再识别中的迁移学习：图像风格转换

https://mp.weixin.qq.com/s/fX94rPgNHrOaQTqBv-ZADg

基于视频的行人再识别新进展：区域质量估计方法和高质量的数据集

https://mp.weixin.qq.com/s/rf-pGfkQFK3abkOLEEVOeA

PTGAN：针对行人重识别的生成对抗网络

https://zhuanlan.zhihu.com/p/34778414

基于时空模型无监督迁移学习的行人重识别

https://zhuanlan.zhihu.com/p/35296881

刷新三数据集纪录的跨镜追踪(行人再识别-ReID)技术介绍

https://mp.weixin.qq.com/s/ZbmJGO3lqwNM2z-E4_Mpbw

由“刷脸”到“识人”，云从科技刷新跨镜追踪(ReID)技术三项世界纪录！

https://zhuanlan.zhihu.com/p/38603624

云从科技资深算法研究员：详解跨镜追踪(ReID)技术实现及难点

https://mp.weixin.qq.com/s/leuILzYz40PqrwsCatYhPw

2018行人再识别年度进展

https://zhuanlan.zhihu.com/p/64004977

2019行人再识别年度进展回顾

https://zhuanlan.zhihu.com/p/37931822

你需要知道的10种行人属性

https://mp.weixin.qq.com/s/YBorhQrJ0UL3HZQHgd5D6A

清华等机构提出基于内部一致性的行人检索方法，实现当前最优

https://zhuanlan.zhihu.com/p/40514536

一个强力的ReID basemodel

https://mp.weixin.qq.com/s/mktVMZ0Fdo0mubstpl2GDA

Repulsion loss：专注于遮挡情况下的行人检测

https://mp.weixin.qq.com/s/NF7j5NYB4TsYZWhA5VGfkQ

Repulsion Loss：遮挡场景下的目标检测

https://mp.weixin.qq.com/s/LZCYx-VyOAMWXBS76ttkFw

如何在不同摄像头里识别行人？多层相似度感知CNN网络解析

https://mp.weixin.qq.com/s/iUPZ4DfL65_NV2v3eWomww

无监督深度关联学习大幅提高行人重识别性能

https://mp.weixin.qq.com/s/Wj6RBS7gYd9skE3AU1s4Qg

尺度不变提升人群计数性能

https://mp.weixin.qq.com/s/3eLL5Xg2mBFWnbYFbW486A

行人检测全新视角：从人体中轴线标注出发

https://mp.weixin.qq.com/s/VwejfjxjVnGFW3WXXyL1og

ALFNet：向高效行人检测迈进

https://mp.weixin.qq.com/s/3gzlln0yqqXQ6Xbwt-VXjw

OR-CNN行人检测：为‘遮挡’而生

https://mp.weixin.qq.com/s/yrygkZUwsCL-twTFvA--1w

尺度不变网络提升人群计数性能

https://mp.weixin.qq.com/s/K7JNqjuUfotTODtJU1W-YQ

快速精准的人头检测，代码已开源

https://mp.weixin.qq.com/s/DZmAWpptAVIqA_7L24ldHg

行人重识别PCB-RPP，SGGNN

https://mp.weixin.qq.com/s/e_n-BsPkrPd4MsyvlMVeYg

行人重识别告别辅助姿势信息，商汤、中科大提出姿势无关的特征提取GAN

https://mp.weixin.qq.com/s/l8ExvxOERUngHBqtep-ZWw

人群计数（Crowd Counting）研究综述

https://zhuanlan.zhihu.com/p/51511683

Graph Reid系列结合谱聚类做特征变换

https://zhuanlan.zhihu.com/p/47073533

结合时空的Re-ID系列：ECCV2018 TAUDL

https://mp.weixin.qq.com/s/eKHL0F3tnjEHY10d5PqkMg

最新Video-based ReID论文核心解读

https://mp.weixin.qq.com/s/iotqiyRrH4kwWmBvYi-tMQ

中科院&地平线开源state-of-the-art行人重识别算法EANet:增强跨域行人重识别中的部件对齐

https://zhuanlan.zhihu.com/p/54576174

行人重识别新任务：训练只需一张标注图片

https://zhuanlan.zhihu.com/p/52274204

AAAI2019 Spatial-Temporal Person Re-identification

https://zhuanlan.zhihu.com/p/55320029

行人跨模态重识别：双向限制的排序损失

https://zhuanlan.zhihu.com/p/55787893

亚马逊提出：用于人群计数的尺度感知注意力网络

https://mp.weixin.qq.com/s/U-ICoZQWatyJmkPcnXNRbA

最新最简易的迁移学习方法，人员再识别新模型

https://mp.weixin.qq.com/s/ajpxP3b5nw2AC393uBypvA

西工大开源拥挤人群数据集生成工具，大幅提升算法精度

https://mp.weixin.qq.com/s/BDgIf6foDGtCNc48JPqrcg

行人重识别算法优化技巧：Bags of Tricks and A Strong Baseline

https://mp.weixin.qq.com/s/FEJDrCvXcnhl5y7KR8EXKw

行人检测新思路：高级语义特征检测取得精度新突破

https://mp.weixin.qq.com/s/BYAmDulUKJLE-rb-Kh8Xmg

CSP行人检测：无锚点框的检测新思路

https://mp.weixin.qq.com/s/FodjDqc30XuT4fV3XPVy8g

一个更加强力的ReID Baseline

https://mp.weixin.qq.com/s/zUcLZ41mse7qRqZgXlMvvg

C3F：首个开源人群计数算法框架

https://mp.weixin.qq.com/s/51X7NIS1UEXJ1sqx54tuwg

NVIDIA/悉尼科技大学/澳洲国立大学新作：用GAN生成高质量行人图像，辅助行人重识别

https://mp.weixin.qq.com/s/JbZblzgLyHBQxP9AecLZbA

旷视研究院提出Re-ID新方法VPM，优化局部成像下行人再识别

https://zhuanlan.zhihu.com/p/69559437

基于自然语言的跨模态行人re-id的SOTA方法简述（上）

https://mp.weixin.qq.com/s/RBfHrvkZlx90RM1Ohd6m9g

一种行人重识别监督之下的纹理生成网络

https://mp.weixin.qq.com/s/PfhhqW3e2miMVrIA2saeCg

英伟达开源行人生成/重识别代码

https://mp.weixin.qq.com/s/nY3NMCeP6yadJrRdSWM1WA

基于密集语义对齐的行人重识别模型：有效解决语义不对齐

https://zhuanlan.zhihu.com/p/72693526

SPGAN--ReID

https://mp.weixin.qq.com/s/ueAIT-oAQQPtaPkljOmTwg

超阿里、大华，澎思科技行人再识别（ReID）技术刷新三大数据集记录

https://mp.weixin.qq.com/s/ePh_fdgLBXKujYY6KGESrA

你需要知道的10种行人属性

https://mp.weixin.qq.com/s/oXM9Stb65WjqlY4GGAAyHw

北邮提出高阶注意力模型，大幅改进行人重识别SOTA精度

https://mp.weixin.qq.com/s/KwvqvIU8tVDgtYdq3HL4OQ

密集人群分布检测与计数

https://mp.weixin.qq.com/s/BMJP6GJfxpyD4FIsR2Fx2w

旷视研究院提出行人搜索当前最佳新方法

https://mp.weixin.qq.com/s/u-6Nl8jjje8B_DkE8b-Kug

旷视研究院张弛：行人重识别及其应用

https://github.com/xingkongliang/Pedestrian-Detection

行人检测（Pedestrian Detection）论文整理

https://mp.weixin.qq.com/s/TGjW7AjtGe2aMU8G-c177Q

自动驾驶中的行人检测方法设计

https://zhuanlan.zhihu.com/p/85383077

视频行人重识别2019各顶会顶刊文章阅读笔记

https://mp.weixin.qq.com/s/kl01FypFQAmG_TiMTNnvNw

旷视研究院提出VANet：具备视角感知力的车辆重识别网络

https://mp.weixin.qq.com/s/2WgYJR6z3aDpOKqhVtSRVA

最新综述：车辆重识别技术

https://mp.weixin.qq.com/s/nqRJQ6K6e1TC6IUa57dw0g

基于自然语言的跨模态行人ReID的SOTA方法简述

https://mp.weixin.qq.com/s/J1_bs1h_eTqmYU4q5Lzhpg

基于神经网络StarNet的行人轨迹交互预测算法

https://zhuanlan.zhihu.com/p/96999382

重识别（re-ID）特征适合直接用于跟踪（tracking）问题么？

https://mp.weixin.qq.com/s/H98W-Ml2MfrbM4iuEV8uVg

Group LSTM：拥挤场景的群体轨迹预测

https://mp.weixin.qq.com/s/JZPLz1Xczby86id4Zvy1UQ

CrowdHuman+Double Anchor：强强联合，推动密集行人检测技术落地

https://zhuanlan.zhihu.com/p/342249413

深度学习行人重识别综述与展望

# 图像检索

## 传统方法

https://mp.weixin.qq.com/s/sM78DCOK3fuG2JrP2QaSZA

SIFT与CNN的碰撞：万字长文回顾图像检索任务十年探索历程（上）

https://mp.weixin.qq.com/s/yzVMDEpwbXVS0y-CwWSBEA

SIFT与CNN的碰撞：万字长文回顾图像检索任务十年探索历程（下）

https://mp.weixin.qq.com/s/Sda94q-40goiZGSYGgm_Yw

基于内容的图像检索技术综述-传统经典方法

https://mp.weixin.qq.com/s/ED-zovVT_vHId4mYXdEo5w

高效大规模图像搜索开源实现

## DL方法

https://zhuanlan.zhihu.com/p/36479489

图像检索：因缘际会与前瞻

https://mp.weixin.qq.com/s/jv6PvFHsweK_rFOuz-p-mg

图像检索从入门到进阶

https://mp.weixin.qq.com/s/aRndRlVnY5ZRBFnNbVNecg

李飞飞CS231n项目：这两位工程师想用神经网络帮你还原买家秀

https://mp.weixin.qq.com/s/zHSDFR_Nd4LfvIaq9kSrww

BMVC2018图像检索论文—使用区域注意力网络改进R-MAC方法

https://mp.weixin.qq.com/s/FJCZvc8pl-CwFhyiCD6E-g

Pinterest视觉搜索工程师孙彦：视觉搜索不是“鸡肋”

https://mp.weixin.qq.com/s/QgYtfvsLGcfqLA98mp19tg

KDD2018阿里巴巴论文揭示自家大规模视觉搜索算法

https://mp.weixin.qq.com/s/CS4iAMprZizrXhpXDhySzg

视频分类/行为识别研究综述，从数据集到方法

http://yongyuan.name/blog/layer-selection-and-finetune-for-cbir.html

图像检索：layer选择与fine-tuning性能提升验证
