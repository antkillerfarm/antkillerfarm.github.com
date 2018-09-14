---
layout: post
title:  深度学习（四十三）——图像检索, 图像变换, Graph NN, Spatial Transformer Networks, 数据增强
category: DL 
---

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

https://mp.weixin.qq.com/s/aRndRlVnY5ZRBFnNbVNecg

李飞飞CS231n项目：这两位工程师想用神经网络帮你还原买家秀

https://mp.weixin.qq.com/s/zHSDFR_Nd4LfvIaq9kSrww

BMVC2018图像检索论文—使用区域注意力网络改进R-MAC方法

https://mp.weixin.qq.com/s/FJCZvc8pl-CwFhyiCD6E-g

Pinterest视觉搜索工程师孙彦：视觉搜索不是“鸡肋”

https://mp.weixin.qq.com/s/QgYtfvsLGcfqLA98mp19tg

KDD2018阿里巴巴论文揭示自家大规模视觉搜索算法

# 图像变换

## 语义分割逆变换

![](/images/img2/SCAN.png)

参考：

https://mp.weixin.qq.com/s/tusm3CYqKKep-BJQ0Vp6Ww

腾讯AI lab & 复旦大学合作提出无监督高分辨率的图像到图像转换方法SCAN

https://mp.weixin.qq.com/s/X96oI-duHwyCFAR79inlig

真实到可怕！英伟达MIT造出马良的神笔

## 参考

https://blog.csdn.net/u011534057/article/details/78935202

风格迁移学习笔记(1):Multimodal Transfer: A Hierarchical Deep Convolutional Neural Network for Fast

https://blog.csdn.net/u011534057/article/details/78935230

风格迁移学习笔记(2):Universal Style Transfer via Feature Transforms

https://mp.weixin.qq.com/s/88tBoAVziGEFXIGfvZZp8g

深度学习图像处理项目集锦：生成可爱的动漫头像，骡子变斑马等入选

https://mp.weixin.qq.com/s/iV-OXiKF1jgAhSmX4QUIXw

综述：图像风格化算法最全盘点

https://mp.weixin.qq.com/s/Zq3ngbTAObQuIr86nkTwjQ

视频换脸技术，女神都下海了吗？

https://mp.weixin.qq.com/s/eqI5fVuF68RQWb1a5O219w

腾讯研发“一键卸妆” ,让女神秒变路人!

https://mp.weixin.qq.com/s/73mkWlqJsVdu9m1kPDvfbQ

用AI让静图变动图：CVPR热文提出动态纹理合成新方法

https://mp.weixin.qq.com/s/gEFzogsteK_1VeywbQxbgQ

(GAN)延时摄影视频的生成

https://zhuanlan.zhihu.com/p/34042498

深度解密换脸应用Deepfake

https://mp.weixin.qq.com/s/gvPTNtrd5Du9Oablp3jZYw

如何使用DeepFake实现视频换脸

https://mp.weixin.qq.com/s/BTzV7ulweqFQokdQ-AX2Rg

三位一体的纯正视频换脸术，拒绝别人的嘴替我说话

https://mp.weixin.qq.com/s/rPDvLnG4MBDRUMCWs2fjcQ

最新StarGAN对抗生成网络实现多领域图像变换

https://mp.weixin.qq.com/s/97Uj-ATLToy1bNhnSUO8Jw

非监督任意姿势人体图像合成

https://mp.weixin.qq.com/s/cfw8mRsmzE1lU8PRM8UC0w

秒变莫扎特、贝多芬，Facebook提出完美转换音乐风格的神经网络

https://mp.weixin.qq.com/s/TiXILy4l6Q3dxVwapyd9KQ

二维码太丑？用风格迁移生成个性二维码了解一下

https://mp.weixin.qq.com/s/7GHBH79kWIpEBLYX-VEd7A

CycleGAN：图片风格，想换就换

https://mp.weixin.qq.com/s/tzPCU1bxQ7NWtQ7o2PjF0g

BAIR提出MC-GAN，使用GAN实现字体风格迁移

https://mp.weixin.qq.com/s/RMz5fwt72j2ufrGpdD6POw

带自注意力机制的生成对抗网络，实现效果怎样？

https://mp.weixin.qq.com/s/PVM7wMsT6TpJkQUlt7d2Aw

用风格迁移搞事情！超越艺术字：卷积神经网络打造最美汉字

https://mp.weixin.qq.com/s/12_Gl4snq-LdMHJSZn4oOA

换脸AI升级版：面部表情、身体动作、视线方向都能实时迁移

https://mp.weixin.qq.com/s/A2VhfO3CkyQGCs5GqBWzOg

实景照片秒变新海诚风格漫画：清华大学提出CartoonGAN

https://mp.weixin.qq.com/s/lPzPfjYiAsNvVcVWdL08dA

照片闭眼也无妨，Facebook黑科技完美补全大眼睛

https://mp.weixin.qq.com/s/h-mp7_oO9aZ1yYxiJMLziQ

作画、写诗、弹曲子，AI还能这么玩？

https://mp.weixin.qq.com/s/txJAnu4FOOjmLhbtGTM-BQ

还敢吹“毫无PS痕迹”？小心被Adobe官方AI打脸

https://mp.weixin.qq.com/s/a1Qg1Hl5NMvEJPXhJR-2BA

效果惊艳！北大团队提出Attentive GAN去除图像中雨滴

https://mp.weixin.qq.com/s/iK7XR0tHV_dE0p1grNQIHw

只需一张照片，运动视频分分钟伪造出来

https://mp.weixin.qq.com/s/-j4p7nUF-rCGk6yK0nccvw

机器人也会画漫画

https://mp.weixin.qq.com/s/3Aq1HXpBzgNdcB130tCKbQ

GAN网络图像翻译机：图像复原、模糊变清晰、素描变彩图

https://mp.weixin.qq.com/s/djkjAfUO_DefTP2drzY_iQ

在《绝地求生》中玩《堡垒之夜》！ 深度学习帮你转换画风

https://mp.weixin.qq.com/s/cTlXMxcpzc7_5NVsTm1jcA

学习一帧，为整段黑白视频上色：谷歌提出自监督视觉追踪模型

https://mp.weixin.qq.com/s/X8osUSPROJqGVTvw0gieDQ

T2T：利用StackGAN和ProGAN从文本生成人脸

https://mp.weixin.qq.com/s/moDVf7h8Q2S1SL0IuxXQtQ

算法音乐往事：二次元女神“初音未来”诞生记

https://mp.weixin.qq.com/s/4UH-XWyIxYHq_ErRfkzzsQ

与神经网络相比，你对P图一无所知

https://mp.weixin.qq.com/s/33VKfq-jFHfn9GBQzFYq0Q

震撼！英伟达用深度学习做图像修复，毫无ps痕迹

https://mp.weixin.qq.com/s/FTfpjo-qT_PK4cbR5j0ulw

AI当“暖男”：给裸照自动穿上比基尼

https://mp.weixin.qq.com/s/LwYzxFO6Fj9Biiww3Y22qw

斯坦福CS230第一名：图像超级补全，效果惊艳

https://mp.weixin.qq.com/s/MFKhmHcaQvufdRawFqisjA

Distill详述“可微图像参数化“：神经网络可视化和风格迁移利器！

https://mp.weixin.qq.com/s/rt4uuqh8IrZTjsYXEZvxKQ

阿里提出全新风格化自编码GAN，图像生成更逼真！

https://mp.weixin.qq.com/s/VFq3BWLpzyKZ3sqVWf1HKA

从换脸到换姿势，AI在图像处理的道路上越走越魔幻

https://mp.weixin.qq.com/s/Mk_EkwTYK2141cU9zC1hvQ

合成逼真图像，试试港中大&英特尔的半参数方法

https://mp.weixin.qq.com/s/K2kptEfezAbcQTE8OgPMtQ

手把手教你用OpenCV和Python实现图像和视频神经风格迁移

https://mp.weixin.qq.com/s/l33D8zNtHVcaMjlw4IY05g

想让照片里的美女“回头”？清华MIT谷歌用AI帮你实现了

https://mp.weixin.qq.com/s/s6VHo8QW9OENMrbqfp6--w

视频换脸新境界：CMU不仅给人类变脸，还能给花草、天气变脸

https://mp.weixin.qq.com/s/UySQTv8uuGungjxFNeHbRw

给动漫人物轻松换装、编舞，这家游戏公司用GAN做到了！

https://mp.weixin.qq.com/s/dRa1mss4dve487FhS4KjTg

AI帮你抠图，阿里妈妈自研算法入选国际顶级学术会议

# Graph NN

https://mp.weixin.qq.com/s/_aydey5ZVwrObmoFXXIYcw

Bengio等人提出图注意网络架构GAT，可处理复杂结构图

https://zhuanlan.zhihu.com/p/34232818

《Graph Attention Networks》阅读笔记

https://zhuanlan.zhihu.com/p/28170197

《Gated Graph Sequence Neural Networks》阅读笔记

https://mp.weixin.qq.com/s/Pm1HiEQOBnbo_GQ_v6Y_zw

腾讯提出自适应图卷积神经网络，接受不同图结构和规模的数据

https://mp.weixin.qq.com/s/bMpugd2Lp35VPr8fQAPzsg

一文概览图卷积网络基本结构和最新进展

https://zhuanlan.zhihu.com/p/31067515

《Semi-Supervised Classification with Graph Convolutional Networks》阅读笔记

https://mp.weixin.qq.com/s/6viSk0Ts_7eTfYrWYi_HDQ

基于图结构的实体和关系联合抽取模型简介

https://mp.weixin.qq.com/s/w5ldyp00CqkX8Kp-8Aw0nQ

图深度学习(GraphDL)，下一个人工智能算法热点？一文了解最新GDL相关文章

https://mp.weixin.qq.com/s/Jt6CjMqNFEXWoL5pkLeVyw

洛桑理工：Graph上的深度学习报告

https://zhuanlan.zhihu.com/p/36117802

《Learn to Represent Programs with Graphs》阅读笔记。这篇论文讲述了DL在程序代码纠错方面的应用。

https://zhuanlan.zhihu.com/p/37278426

Graph2Seq: Graph to Sequence Learning with Attention-based Neural Networks

https://mp.weixin.qq.com/s/iQYVyo2PHuGbEsYgdIf_oQ

DeepMind等机构提出“图网络”：面向关系推理

https://mp.weixin.qq.com/s/TAccHagxXQ82lfE91Y6xWg

CNN已老，GNN来了：重磅论文讲述深度学习的因果推理

https://mp.weixin.qq.com/s/UONtTJJgDawRPWtatAVKkg

如何利用高效的搜索算法来搜索网络的拓扑结构

https://mp.weixin.qq.com/s/lt9lZbulkW0C8A_xi6hodQ

浅析图卷积神经网络

https://mp.weixin.qq.com/s/SGCtwYWfnxjcpMJeeH1b4w

图神经网络+池化模块，斯坦福等提出层级图表征学习

https://mp.weixin.qq.com/s/DOau_vTbwCauQ8mrHkGu9Q

首个面向Facebook、arXiv网络图类的对抗攻击研究

https://mp.weixin.qq.com/s/XSug_qOqq_QaphkiRlGkIg

图卷积GCN前沿方法介绍

https://mp.weixin.qq.com/s/aeQyZ8cpz81cK8Dg-84mjA

网络表征学习综述

# Spatial Transformer Networks

论文：

《Spatial Transformer Networks》

参考：

http://www.cnblogs.com/neopenx/p/4851806.html

Spatial Transformer Networks(空间变换神经网络)

http://blog.csdn.net/shaoxiaohu1/article/details/51809605

论文笔记：Spatial Transformer Networks

http://blog.csdn.net/shaoxiaohu1/article/details/51809605

Spatial Transformer Networks

https://mp.weixin.qq.com/s/ciqQMezcB-oM24X8eQqTNg

花式玩耍Spatial Transformation Networks

https://mp.weixin.qq.com/s/4VE2lZeFf05AyLp_s3nTFQ

理解Spatial Transformer Networks

# 数据增强

https://mp.weixin.qq.com/s/GqPfvWwH1T0XFwiZ86cW8A

SamplePairing：针对图像处理领域的高效数据增强方式

https://mp.weixin.qq.com/s/cQtXvOjSXFc4YKn7ANBc_w

谷歌大脑提出自动数据增强方法AutoAugment：可迁移至不同数据集

https://mp.weixin.qq.com/s/ojFo7-gUh73iK3uImFS2-Q

一文道尽主流开源框架中的数据增强

https://mp.weixin.qq.com/s/xJhWu-1FyhIWbFBC5oHMkw

一文道尽深度学习中的数据增强方法（上）

https://mp.weixin.qq.com/s/OctAGrcBB0a6TOGWMmVKUw

深度学习中的数据增强（下）

https://mp.weixin.qq.com/s/lMU6_ywQqneyunqEV6uDiA

如何改善你的训练数据集？

https://mp.weixin.qq.com/s/ooX9Hj5ejO6po6Ghb4zOug

一文解读合成数据在机器学习技术下的表现

https://zhuanlan.zhihu.com/p/33485388

mixup与paring samples ，ICLR2018投稿论文的数据增广两种方式

https://mp.weixin.qq.com/s/_7xFBLPGT0VRTJ22toHJ3g

深度学习中常用的图像数据增强方法

https://mp.weixin.qq.com/s/sXV9epWguGbJEZYo4yNp5Q

如何正确使用样本扩充改进目标检测性能

