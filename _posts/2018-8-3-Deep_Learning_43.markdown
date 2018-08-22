---
layout: post
title:  深度学习（四十三）——图像检索, 图像变换, Attention进阶, 多任务学习, 语义分割进阶
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

https://mp.weixin.qq.com/s/88tBoAVziGEFXIGfvZZp8g

深度学习图像处理项目集锦：生成可爱的动漫头像，骡子变斑马等入选

https://mp.weixin.qq.com/s/iV-OXiKF1jgAhSmX4QUIXw

综述：图像风格化算法最全盘点

https://mp.weixin.qq.com/s/Zq3ngbTAObQuIr86nkTwjQ

视频换脸技术，女神都下海了吗？

https://mp.weixin.qq.com/s/eqI5fVuF68RQWb1a5O219w

腾讯研发“一键卸妆” ,让女神秒变路人!

https://mp.weixin.qq.com/s/joRcvCQwLYgRd29mfPd7XA

中科大&微软提出立体神经风格迁移模型，可用于3D视频风格化

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

还敢吹「毫无PS痕迹」？小心被Adobe官方AI打脸

https://mp.weixin.qq.com/s/a1Qg1Hl5NMvEJPXhJR-2BA

效果惊艳！北大团队提出Attentive GAN去除图像中雨滴

https://mp.weixin.qq.com/s/iK7XR0tHV_dE0p1grNQIHw

只需一张照片，运动视频分分钟伪造出来

https://mp.weixin.qq.com/s/-j4p7nUF-rCGk6yK0nccvw

机器人也会画漫画

https://mp.weixin.qq.com/s/3Aq1HXpBzgNdcB130tCKbQ

GAN网络图像翻译机：图像复原、模糊变清晰、素描变彩图

https://mp.weixin.qq.com/s/fMtuJbWG_d9zyCZ0oYyX_w

经得住考验的“假图片”：用TensorFlow为神经网络生成对抗样本

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

https://mp.weixin.qq.com/s/X96oI-duHwyCFAR79inlig

真实到可怕！英伟达MIT造出马良的神笔

https://mp.weixin.qq.com/s/MFKhmHcaQvufdRawFqisjA

Distill详述「可微图像参数化」：神经网络可视化和风格迁移利器！

# Attention进阶

https://mp.weixin.qq.com/s/y_hIhdJ1EN7D3p2PVaoZwA

阿里北大提出新attention建模框架，一个模型预测多种行为

https://mp.weixin.qq.com/s/Yq3S4WrsQRQC06GvRgGjTQ

打入神经网络思维内部

https://mp.weixin.qq.com/s/MJ1578NdTKbjU-j3Uuo9Ww

基于文档级问答任务的新注意力模型

https://mp.weixin.qq.com/s/C4f0N_bVWU9YPY34t-HAEA

UNC&Adobe提出模块化注意力模型MAttNet，解决指示表达的理解问题

https://mp.weixin.qq.com/s/V3brXuey7Gear0f_KAdq2A

基于注意力机制的交易上下文感知推荐，悉尼科技大学和电子科技大学最新工作

http://mp.weixin.qq.com/s/Bt6EMD4opHCnRoHKYitsUA

结合人类视觉注意力进行图像分类

https://mp.weixin.qq.com/s/POYTh4Jf7HttxoLhrHZQhw

基于双向注意力机制视觉问答pyTorch实现

https://mp.weixin.qq.com/s/2gxp7A38epQWoy7wK8Nl6A

谷歌翻译最新突破，“关注机制”让机器读懂词与词的联系

https://zhuanlan.zhihu.com/p/25928551

用深度学习（CNN RNN Attention）解决大规模文本分类问题-综述和实践

http://blog.csdn.net/leo_xu06/article/details/53491400

视觉注意力的循环神经网络模型

https://mp.weixin.qq.com/s/0yb-YRGe-q4-vpKpuE4D_w

多种注意力机制互补完成VQA（视觉问答）

https://mp.weixin.qq.com/s/l4HN0_VzaiO-DwtNp9cLVA

循环注意力区域实现图像多标签分类

https://mp.weixin.qq.com/s/zhZLK4pgJzQXN49YkYnSjA

自适应注意力机制在Image Caption中的应用

https://mp.weixin.qq.com/s/uvr-G5-_lKpyfyn5g7ES0w

基于注意力机制，机器之心带你理解与训练神经机器翻译系统

https://mp.weixin.qq.com/s/ANpBFnsLXTIiW6WHzGrv2g

自注意力机制学习句子embedding

https://mp.weixin.qq.com/s/49fQX8yiOIwDyof3PD01rA

CMU&谷歌大脑提出新型问答模型QANet：仅使用卷积和自注意力，性能大大优于RNN

https://mp.weixin.qq.com/s/c64XucML13OwI26_UE9xDQ

滴滴披露语音识别新进展：基于Attention显著提升中文识别率

https://mp.weixin.qq.com/s/7OYY3L7gL4wVv_EjoosOHA

如何增强Attention Model的推理能力

https://mp.weixin.qq.com/s/9Kt6_DfeYRnhsb10aCSFGw

FAGAN：完全注意力机制（Full Attention）GAN，Self-attention+GAN

# 多任务学习

https://mp.weixin.qq.com/s/guAgXdhZSbEAkERSB1sLRA

多任务学习-Multitask Learning概述

https://mp.weixin.qq.com/s/A-CVKTz_moaFzTYywSt2gg

张宇 杨强：多任务学习概述

https://mp.weixin.qq.com/s/ZlCI02UdRuFBc-uKqIPE_w

深度学习多任务学习综述

https://mp.weixin.qq.com/s/QXOy2jo4RhCZrD5bSVzBOQ

共享相关任务表征，一文读懂深度神经网络多任务学习

https://mp.weixin.qq.com/s/mm9bXXTEzd8DwyYlMgGMZg

NLP多任务学习：一种层次增长的神经网络结构

https://mp.weixin.qq.com/s/X6FwTgr282hbqgOz3oBX-w

多任务学习概述论文：从定义和方法到应用和原理分析

https://blog.csdn.net/CoderPai/article/details/80080455

多任务学习与深度学习

https://blog.csdn.net/CoderPai/article/details/80087188

利用TensorFlow一步一步构建一个多任务学习模型

https://mp.weixin.qq.com/s/fcFb6WkJVP8TYpoxkQgiWQ

CMU提出“十字绣网络”，自动决定多任务学习的最佳共享层

https://mp.weixin.qq.com/s/i7WAFjQHK1NGVACR8x3v0A

自然语言十项全能：转化为问答的多任务学习

https://mp.weixin.qq.com/s/NpO1UP_mzyaeqW26xLY1Xg

CVPR 2018最佳论文作者亲笔解读：研究视觉任务关联性的Taskonomy

# 语义分割进阶

https://mp.weixin.qq.com/s/cANlqQAI-A2mC9vnd3imQA

Instance-Aware图像语义分割

https://mp.weixin.qq.com/s/v_TLYYq6cFWuwR9tXM8m-A

如何通过CRF-RNN模型实现图像语义分割任务

https://mp.weixin.qq.com/s/ceCC7Q6yr0QKESeZXi6lWQ

堆叠解卷积网络实现图像语义分割顶尖效果

https://zhuanlan.zhihu.com/p/24738319

“见微知著”——细粒度图像分析进展综述

https://mp.weixin.qq.com/s/V4_euZRcyyxeimXAA_waAg

贾佳亚：最有效的COCO物体分割算法

https://mp.weixin.qq.com/s/Amr34SdrPZho1GQpFS7WBA

见微知著：语义分割中的弱监督学习

https://mp.weixin.qq.com/s/zOWA1oKbopZJuYIAYYlKTA

港中文-商汤联合论文：自监督语义分割的混合与匹配调节

https://mp.weixin.qq.com/s/M1Oo4ST2aspgZF8UeSUDww

如何妙笔勾檀妆：像素级语义理解

https://mp.weixin.qq.com/s/xalo2XtKtzR5tA_dPFzaJw

一文介绍3篇无需Proposal的实例分割论文

https://mp.weixin.qq.com/s/BL1xZ_YuuPe9frIc9E1fkA

南开大学提出新物体分割评价指标

https://mp.weixin.qq.com/s/3rfZUhio4Bk1RUGkEk5xoQ

ETH Zurich提出新型网络“ROAD-Net”，解决语义分割域适配问题

https://mp.weixin.qq.com/s/qMLCi-CghxvTcwyPnvFxnQ

ConvCRF：一种结合条件随机场与CNN的高效语义分割方法

https://mp.weixin.qq.com/s/deepxMWCpIEe3jk_kanfMg

金字塔注意力网络：一种利用底层像素与高级特征的语义分割网络

https://mp.weixin.qq.com/s/LVD7rry0BajGh9iQg-Y2jw

MIT用AI实现3分钟自动抠图，精细到头发丝

https://mp.weixin.qq.com/s/5n3jpvv_LxnHB0w4hsCEzQ

NVIDIA ECCV18论文:超像素采样网络助力语义分割与光流估计
