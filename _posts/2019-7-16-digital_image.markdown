---
layout: post
title:  数字成像
category: resource 
---

* toc
{:toc}

# 数字成像

计算摄影学（Computational Photography）是计算机图形学，计算机视觉，光学和传感器等领域的交叉学科。

## 教程

http://graphics.cs.cmu.edu/courses/15-463/

CMU: Computational Photography

http://stanford.edu/class/ee367/

EE367/CS448I: Computational Imaging and Display

## blog

https://zhuanlan.zhihu.com/hawkcp

一个计算摄影学的专栏

https://zhuanlan.zhihu.com/c_1035195596059222016

Mars说光场（光场(Light Field)技术科普）

https://zhuanlan.zhihu.com/OpticPhantasm

一个光学的专栏

## CCD

岩间和夫，1919～1982，东京帝国大学理学部地球物理学毕业。索尼前社长。最大的贡献在于推动CCD(电荷耦合元件)的研究，人称他为“CCD小子”。

1970年：“完全回收在CCD上的投资，本世纪内都没有可能实现。”

八年后，CCD终于开发成功。然而岩间并没有等到CCD的量产，就因肠癌去世。继任索尼社长的大贺典雄将一片CCD镶嵌在岩间和夫的墓碑上，来纪念他对索尼的贡献。

![](/images/img2/CMOS.png)

参考：

http://science.china.com.cn/2016-09/26/content_9058067.htm

睹物思人--索尼历史档案馆的另类发现

---

>笔者曾经问过Sony图像传感器部门的大佬，CCD的画质更好，到底是什么因素导致相机市场最终倒向cmos image sensor，是因为对高像素的需求？低功耗的需求？容易集成后处理电路？还是因为cmos sensor成本低？   
>大佬给出了一个出乎我预料的答案：最主要是‘产能’。手机相机的迅速普及，图像传感器的需求暴增，当时采用CCD工艺的工厂数远远少于cmos工艺的工厂数，cmos工艺的工厂迅速转过去生产图像传感器满足了市场需求，同时cmos image sensor的设计者逐渐改进cmos sensor设计，融合CCD和Cmos的设计方法，逐渐提高了cmos image sensor的图像质量。

参考：

https://mp.weixin.qq.com/s/cAN1Qbe_sRfPdjlLy0ig1A

一亿像素的是与非

## 深度相机

深度相机按照深度测量原理不同，一般分为：飞行时间法、结构光法、双目立体视觉法。

https://zhuanlan.zhihu.com/p/31921046

深度相机的主流技术方案一览Structure Light，ToF，Stereo Dual

https://mp.weixin.qq.com/s/7es1wtJXatJR2eHScrk2_w

深度相机大盘点（1）

https://zhuanlan.zhihu.com/p/32170603

iPhone X的原深感相机到底是个什么玩意？

https://zhuanlan.zhihu.com/p/32171299

深度相机原理揭秘--飞行时间（TOF）

https://zhuanlan.zhihu.com/p/32199990

深度相机原理揭秘--双目立体视觉

https://mp.weixin.qq.com/s/veFDawkUJFidUFLFaykmGg

双目立体视觉原理及应用

https://mp.weixin.qq.com/s/8YRosO_wuE7Nx-IowldloQ

关于双目立体视觉的三大基本算法及发展现状的总结

http://mp.weixin.qq.com/s/MWQDwMoRc0c1ofkENzn3Ew

结构光，TOF，双目视觉的异同

https://zhuanlan.zhihu.com/p/98492961

深度相机(TOF)的工作原理

https://mp.weixin.qq.com/s/uT_jrFzfttnG_VYiJIsRbQ

走近3D ToF摄像头，揭秘ToF传感器工作原理

https://mp.weixin.qq.com/s/H85oyZh5SzJh6SyF49Pzmg

一文看懂TOF

https://mp.weixin.qq.com/s/jEwrEkAp1-UqSuNAASEwCA

清华创业团队发布3D视觉技术白皮书，万字长文详述ToF

https://zhuanlan.zhihu.com/p/112131317

多视角立体视觉简介

https://zhuanlan.zhihu.com/p/73748124

多视角立体视觉MVS简介

https://mp.weixin.qq.com/s/xEOSica0kYxOgwmGFQKbRA

​多视图立体视觉: CVPR 2019 与 AAAI 2020 上的ACMH、ACMM及ACMP算法介绍

https://mp.weixin.qq.com/s/WEbdO1F_Fz84_-Go7Fgm2Q

计算机视觉中的双目立体视觉和体积度量

https://mp.weixin.qq.com/s/9RUnWSSlEiwfoBB19faTcg

双目立体视觉I：标定和校正

## 全向视图

在需要大视野的场景，如虚拟现实应用程序或自动机器人，全向（Omnidirectional）摄像机比传统摄像机具有巨大优势。不幸的是，标准的卷积神经网络不适用于这种情况，因为自然投影表面是一个球体，尤其是在两极地区（polar regions），如果不引入明显的畸变（distortions），无法将其展开（unwrapped）到一个平面。

将球面图像映射为平面图像的过程，一般被称为dewarp。

https://zhuanlan.zhihu.com/p/88675419

在鱼眼和全向视图图像的深度学习方法（上）

https://zhuanlan.zhihu.com/p/88675790

在鱼眼和全向视图图像的深度学习方法（下）

## 结构光

https://mp.weixin.qq.com/s/o_XsmqLSPhvuC0iiK7xHag

结构光（1）基本介绍

https://mp.weixin.qq.com/s?__biz=MzU3ODU0Nzk5MA==&mid=2247483804&idx=1&sn=9e37b8cbd9c47f108c1153e3165fffa3

小米结构光VS iPhone 结构光（一）

https://zhuanlan.zhihu.com/p/54761392

结构光综述

## 解析力

https://zhuanlan.zhihu.com/p/20726167

解析力（1）MTF SFR

https://zhuanlan.zhihu.com/p/20726175

解析力（2）空间采样和奈奎斯特

https://blog.csdn.net/jaych/article/details/50889664

SFR算法详解1

https://blog.csdn.net/jaych/article/details/50700576

SFR算法详解2

https://blog.csdn.net/jaych/article/details/51030939

SFR算法详解3

https://blog.csdn.net/jaych/article/details/51031064

SFR算法详解4

## 双目成像

https://zhuanlan.zhihu.com/p/50801189

OpenCV双目稠密匹配BM算法源代码详细解析

https://www.jianshu.com/p/cbe50ede70aa

半全局块匹配（Semi-Global Block Matching）算法

https://mp.weixin.qq.com/s/SD1pkTNHG7TGKrwVbFaMZg

中科慧眼崔峰：下一代双目视觉产品预警距离将达到80米

https://mp.weixin.qq.com/s/mYijtRNvkSeVxk0sWkORSw

双目视觉简介

https://zhuanlan.zhihu.com/p/56236308

基于双目视觉的自动驾驶技术

https://mp.weixin.qq.com/s/j4jfTKkbdQ9bHD7IZFdkbA

国防科大提出双目超分辨算法，效果优异代码已开源

https://mp.weixin.qq.com/s/QDMMP05hUqj6tqUWyCDiTQ

来聊聊双目视觉的基础知识

## HDR

多媒体软硬件的快速发展使得广播电视和互联网平台上的视频质量变得越来越高。我们最直观的感受是，视频变得更清晰，也更真实了。但与此同时，视频的动态范围（可以简单地理解为对比度）并没有太大提升。这就造成视频画面中的像素很多，但每个像素点质量并不高，不能展现场景丰富的层次和细节。

高动态范围（High Dynamic Range，HDR）这一概念的提出，正是为了从画面动态范围这个维度，进一步提升视频质量。相对地，我们称传统视频为标准动态范围（Standard Dynamic Range，SDR）视频。

https://mp.weixin.qq.com/s/sXC263Lq25tKp2AK2Tsb-A

Omnivision HDR sensor简介

https://mp.weixin.qq.com/s/NuJYejYJ1kKdD2pRo5Wxlw

HDR Imaging(2)--Digital Overlap

https://mp.weixin.qq.com/s/_znyQAPeGt_IiZ8C5Rv6zw

HDR imaging(3)---split/sub pixel技术

https://mp.weixin.qq.com/s/X1GcQywkM-hZUu-1koP_VA

视频增强之“动态范围扩展”HDR技术漫谈

https://mp.weixin.qq.com/s/zPHNtK4wUQnVLkIXcjqd_A

韩国科技院Lin Wang最新TPAMI《深度学习HDR成像》综述论文阐述深度学习高动态范围成像方法

## Noise Reduce

2D降噪：只在2维空间域上进行降噪处理。基本方法：对一个像素将其与周围像素平均，平均后噪声降低，但缺点是会造成画面模糊，特别是物体边缘部分。因此对这种算法的改进主要是进行边缘检测，边缘部分的像素不用来进行模糊。

3D降噪：增添了时域处理，因此变为3维。和2d降噪的不同在于，2d降噪只考虑一帧图像，而3d降噪进一步考虑帧与帧之间的时域关系，对每个像素进行时域上的平均。例如，假设场景静止，那么连续两帧图像内容没变，他们的差值就是2倍的噪声。通过减少时域上的改变降低噪声。

相比2d降噪，3d降噪效果更好，且不会造成边缘的模糊，但存在的主要问题是：画面不会是完全静止的，如果对不属于同一物体的两个点进行降噪处理会造成错误。因此该方法需要运动估计，其效果好坏也与运动估计相关。而运动估计计算量大，耗时长，是制约3d降噪的主要瓶颈。

## 参考

https://mp.weixin.qq.com/s/od9uDZdRU4QaBKPHb0tLag

cmos sensor基础

https://mp.weixin.qq.com/s/lVS3CgZGItUkWG-OtolbSA

数字成像书籍推荐与资料分享

https://mp.weixin.qq.com/s/v6OzcLvjLetSBspNI_NtSA

图像传感器的演进与创新

https://mp.weixin.qq.com/s/1IHHo4sV79sXdtXNzKNhMA

计算摄影--google相机的王者之道

https://mp.weixin.qq.com/s/CDeatCRZ1utgx2X20H_4Lg

CMOS图像传感器科普

https://mp.weixin.qq.com/s/T4XULnykHANEetmDIhZdaw

mobile camera sensor技术方向选择的经验与教训

https://mp.weixin.qq.com/s/EcAAc_ypajEKXUQbOoYAMA

如何选择IR filter

https://mp.weixin.qq.com/s/typXBdisUI-d_viVInx9XQ

解析DXO图像质量评价体系

https://mp.weixin.qq.com/s/t-Mzx0IEdZG7_TmMpT_cBA

小谈CMOS Sensor设计之FSI和BSI

https://mp.weixin.qq.com/s/gyuKlDwlIqaZmC3w-r8oqg

手机相机如何排名

https://mp.weixin.qq.com/s/cDrVpqsY7f2HZCJa4X8Iww

Google Pixel 2/2XL视频稳定技术探究

https://mp.weixin.qq.com/s/7E9QDSM095KyYwPs-wDznA

成像相关颜色测量仪器简介

https://mp.weixin.qq.com/s/oeMqE0MzXyGeABGekY9eiQ

眼擎科技CEO朱继志：如何设计自动驾驶的视觉成像系统

https://blog.csdn.net/weixin_38285131/article/details/80457068

光场相机重聚焦原理介绍及代码解析

https://blog.csdn.net/weixin_38490884/article/details/83009344

光场的可视化与重聚焦原理

https://mp.weixin.qq.com/s/pzfZcfytXMyGSz3kc6Jm9A

空间-角度信息交互的光场图像超分辨

https://mp.weixin.qq.com/s/ofoh7xH9MB0ifORi-ZQbcA

苹果iphone Xs/Xr camera有哪些改进

https://mp.weixin.qq.com/s/24ipxnkh6TNiqRrakgw6ew

Google Pixel3拍照为什么那么牛？

https://mp.weixin.qq.com/s/cxsdBLnguAFJcFP65_cC0A

3D摄像头产业链解析

https://mp.weixin.qq.com/s/Kp1CLCz0eG38r1HplQ02Ew

即将兴起的汽车内视相机

https://mp.weixin.qq.com/s/jmvcuiRu82zVC2kq91uS0g

谷歌AI用“深度”学习来虚化背景，单摄手机可用，Jeff Dean表示优秀

https://mp.weixin.qq.com/s/tg1Rjl-8t93IVIh1PHTuWg

什么是‘log灰’

https://mp.weixin.qq.com/s/al1rP_LQLe1xIGuwK2Gr2A

AI camera时代对成像带来了哪些影响

https://mp.weixin.qq.com/s/YW3anc5S9_BDrXiyVro5fQ

从光学成像到计算光学成像
