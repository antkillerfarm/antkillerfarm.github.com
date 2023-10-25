---
layout: post
title:  深度学习（四十三）——Conv计算量分析
category: DL 
---

* toc
{:toc}

# Conv计算量分析

I: Input

O: Output

K: Kernel

c: Channel

h: Height

w: Width

forward:

$$O = I * K$$

filter gradient:

因为Conv是不可交换群，所以：

$$I' \star O = I' \star (I * K) = (I' \star I) * K = K$$

I'是I的逆元。

$$K = I' \star O$$

input gradient:

$$O \bullet K = I * (K \bullet K')$$

$$I = O \bullet K'$$

如果是FC运算的话，则上述所有二元运算符退化为矩阵乘法，I'为转置运算。

## forward

single point:

$$I_c \times K_h \times K_w \times 2$$

point number:

$$O_c \times O_h \times O_w$$

all:

$$(O_c \times O_h \times O_w) \times (I_c \times K_h \times K_w \times 2)$$

## backward

- filter gradient

single point:

$$O_h \times O_w \times 2$$

point number:

$$I_c \times O_c \times K_h \times K_w$$

all:

$$(I_c \times O_c \times K_h \times K_w) \times (O_h \times O_w \times 2)$$

- input gradient

single point:

$$K_h \times K_w \times 2$$

point number:

$$I_c \times O_c \times O_h \times O_w$$

all:

$$(I_c \times O_c \times O_h \times O_w) \times (K_h \times K_w \times 2)$$

## 参数对计算量的影响

pad/stride/dilation改变$$O_h,O_w$$。

group改变$$K_o, O_c$$。

multiplier改变$$O_c$$，但不改变$$K_o$$，计算量不变，IO增加N倍。所以将上述公式的$$O_c$$改为$$K_o$$即可满足所有情况。

# 深度信息检索+

https://mp.weixin.qq.com/s/8lRzE5nGCNfD6sQ0lDRDyg

信息检索中的神经匹配和重要性学习，163页pdf

https://mp.weixin.qq.com/s/jZCHyjhTW9JHW3xQSTzyYA

深度学习搜索，Deep Learning for Search，327页pdf

https://mp.weixin.qq.com/s/aZsj1FQnzHOr-YBcy_ljpw

DNN在搜索场景中的应用

https://mp.weixin.qq.com/s/1jgdI-Pt0PtN3oAs0Wh4XA

阿里提出电商搜索全局排序方法，淘宝无线主搜GMV提升5%

https://mp.weixin.qq.com/s/9Fcj5lO-JPfFVnRSSM_56w

深度学习在美团搜索广告排序的应用实践

https://mp.weixin.qq.com/s/N3JBHlqneG9dI0I26M3wHQ

如何做好大规模视觉搜索？eBay基于实践总结出了7条建议

https://mp.weixin.qq.com/s/8Twe3e3WKCY9pTiNtnW2sg

重磅！谷歌等推出基于机器学习的数据库SageDB

https://mp.weixin.qq.com/s/WpITPvYmixMHa0ha0MgWVA

神马搜索如何提升搜索的时效性？

https://zhuanlan.zhihu.com/p/163358322

learning to match for product search

https://mp.weixin.qq.com/s/_MpfRGYG_pleaEQMpQT5Mg

深度学习在视觉搜索和匹配中的应用

https://mp.weixin.qq.com/s/s8swIdAPw_VeAWnZTL1riA

搜你所想，从Query意图识别到类目识别的演变

https://mp.weixin.qq.com/s/dO3eDlhCSYRh3pjbI_gsQg

深度学习图像检索(CBIR): 十年之大综述

https://mp.weixin.qq.com/s/q4aPtUYi27h-0sqD4bokQQ

再谈搜索中的Query扩展技术

https://mp.weixin.qq.com/s/vwIbM2XNvj-OKCtzqjCVWQ

搜索中涉及的算法问题

https://mp.weixin.qq.com/s/LC4ch4O2eUjgbMLH9zT0pw

如何用深度学习来做检索：度量学习中关于排序损失函数的综述（1）

https://mp.weixin.qq.com/s/TwG6_KJvTGCzDMy6ZD6jsA

如何用深度学习来做检索：度量学习中关于排序损失函数的综述（2）

https://mp.weixin.qq.com/s/wni3F9lKuO4OT32BVe0QDQ

谷歌发大招：搜索全面AI化，不用关键词就能轻松“撩书”

https://mp.weixin.qq.com/s/TrWwp-DBTrKqIT_Pfy_o5w

阿里妈妈首次公开新一代智能广告检索模型，重新定义传统搜索框架

https://mp.weixin.qq.com/s/fZv9FgbdQ1bWPoNdl9sF1A

“宝石迷阵”与信息检索

https://mp.weixin.qq.com/s/Vvo3Ti3XiGQz0IwLgATfWQ

电商搜索算法技术的演进

https://mp.weixin.qq.com/s/-UxlJb4GCOTV1uTyZehyZw

ESIM：语义相似度领域小模型的尊严

https://mp.weixin.qq.com/s/hMaZ_6y3b4CRzq2p9eVkVw

使用Python过滤出类似的文本的简单方法

https://mp.weixin.qq.com/s/7F-35VjIp_cmFNEceti1uA

利用孪生网络，Keras，Tensorflow比较图片相似度

https://mp.weixin.qq.com/s/MpuUdZi8CWcu0b-ij-bHjA

Jeff Dean出品：用机器学习索引替代B-Trees，3倍性能提升，10-100倍空间缩小

https://mp.weixin.qq.com/s/uztYEW_azetOkOGiZcbCuw

JeffDean又用深度学习搞事情：这次要颠覆整个计算机系统结构设计。这篇blog介绍了如何用DL方法提高内存访问的命中率。

https://zhuanlan.zhihu.com/p/37020639

读论文系列：CVPR2018 SSAH

https://mp.weixin.qq.com/s/TdnstQaBcLaXg8BvuR7oYA

基于素描图的细粒度图像检索

https://mp.weixin.qq.com/s/YrepffjMkxeuFRVMv8MClg

信息检索技术进展: 从词袋到BERT，230页ppt

https://mp.weixin.qq.com/s/vN9X3CRwJuHLJwzvfDg1kA

强化学习如何用于信息检索？请看ECIR2021《基于强化学习的信息检索》教程

https://mp.weixin.qq.com/s/CAzafDevfNs0hHUmquds2Q

如何构建一个好的电商搜索引擎？

# AutoDL+

https://mp.weixin.qq.com/s/qou0uUQVsU306KvB5JMKVg

贝叶斯分析助你成为优秀的调参侠：自动化搜索物理模型的参数空间

https://mp.weixin.qq.com/s/pJ4cSS4qXQaM8ACv_Aq--A

结合Sklearn的网格和随机搜索进行自动超参数调优

https://mp.weixin.qq.com/s/fqNl6WPqIP7TiAmg5emkRw

针对目标检测任务从基础运算符号自动构建损失函数

https://mp.weixin.qq.com/s/pDvl1LEV5ujJbOfU-PDuiA   

走马观花AutoML

https://mp.weixin.qq.com/s/y-5Gh0Jy3Fpz8xvw_tOICg

MixPath：基于权重共享的神经网络搜索统一方法

https://mp.weixin.qq.com/s/tYV_ratEA0NxzJukF-RTSA

玩转网络结构搜索？你需要更大的搜索空间

https://mp.weixin.qq.com/s/IRDE4RqwXR5W7QFhD_tSPQ

AutoML在深度学习模型设计和优化中有哪些用处？

https://zhuanlan.zhihu.com/p/143730467

One Shot NAS总结

https://mp.weixin.qq.com/s/e7Whr2Im5fu8O7zF95Afkw

进化算法如何用于自动模型搜索(NAS)

https://mp.weixin.qq.com/s/QtYNb1bVnWghNFK6fbynLw

激活函数如何进行自动学习和配置

https://mp.weixin.qq.com/s/0fVNW5hekUNCsD6L9KL1iQ

PNAS：渐进式神经网络搜索，准确率预测，21倍加速

https://mp.weixin.qq.com/s/Ho6XDRYmOBs9TqLMq7aPAg

MetaQNN: 与Google同场竞技，MIT经典作，基于Q-Learning的神经网络搜索

https://mp.weixin.qq.com/s/5NGCr6oMW2p5u4kF7YMCPw

NAS太难了，搜索结果堪比随机采样！华为ICLR 2020论文给出6条建议

https://zhuanlan.zhihu.com/p/110527110

Neural Architecture Search

https://zhuanlan.zhihu.com/p/111213620

AutoML在计算机视觉领域的能与不能

https://mp.weixin.qq.com/s/MIJHiYk-CjYtmW_9zW4hjA

Hyper-Parameter Optimization，56页pdf

https://mp.weixin.qq.com/s/MkXBtGq4xt5YOh1-uhMBbg

循环神经网络自动生成程序：谷歌大脑提出“优先级队列训练”

https://mp.weixin.qq.com/s/vctbsYk4LRwrQ7_Hs7fqkg

谷歌大脑发布神经架构搜索新方法：提速1000倍

https://mp.weixin.qq.com/s/9qpZUVoEzWaY8zILc3Pl1A

进化算法+AutoML，谷歌提出新型神经网络架构搜索方法

https://mp.weixin.qq.com/s/HJ5caV1bQi7qVDeNXZ21qg

手把手教你用Cloud AutoML做毒蜘蛛分类器

https://mp.weixin.qq.com/s/smAlLm-JZn_s2utSCVk7ig

归一化(Normalization)方法如何进行自动学习和配置

https://mp.weixin.qq.com/s/E0ULyXGz3UEcD0cIg-XkNA

优化方法可以进行自动搜索学习吗？

https://mp.weixin.qq.com/s/439I6hHIfq6KpkLEwAgDrw

MIT韩松团队开发“万金油”母网，嵌套10^19个子网，包下全球所有设备

https://mp.weixin.qq.com/s/OL_MuzKqAaDoOU3LF3Xmow

强化学习如何用于自动模型设计(NAS)与优化？

https://mp.weixin.qq.com/s/c7S_hV_8iRhR4ZoFxQYGYQ

CVPR 2019神经网络架构搜索进展综述

https://mp.weixin.qq.com/s/hWRStNp0Gu_ZO7J1Sjr6-w

视频架构搜索的研究

https://mp.weixin.qq.com/s/ABNPCpgyk_2EeYwnJFFehg

自动优化架构，这个算法能帮工程师设计神经网络

https://mp.weixin.qq.com/s/0kGJfKARKs2TuIQ4YJYbUA

比手工模型快10~100倍，谷歌揭秘视频NAS三大法宝

https://zhuanlan.zhihu.com/p/100570132

NAS evaluation is frustratingly hard

https://mp.weixin.qq.com/s/QEkOQG2dQrPG-aa8OsVSOg

NASNet: Google Brain经典作，改造搜索空间，性能全面超越人工网络，继续领跑NAS领域

https://mp.weixin.qq.com/s/ClqOWx1RJGzoS0akI2s93w

多媒体的AutoML与元学习，清华大学朱文武教授等

https://mp.weixin.qq.com/s/dS3nHGcQ1VoOyVBcRPFtkg

AutoML都有哪些核心技术，如何对其进行长期深入学习

https://mp.weixin.qq.com/s/A3Bg4-WoEQxFk3G5hCKNPQ

连续可微分架构如何用于网络结构搜索

https://mp.weixin.qq.com/s/0AE8w_0cZGcMZIwjUTKPrg

如何学习AutoML在模型优化中的应用，这12篇文章可以作为一个参考

https://mp.weixin.qq.com/s/LxLTsz7T_lAkqfZ9voSX5Q

强化学习网络结构搜索(一)

https://mp.weixin.qq.com/s/wL0prNVm5bKWOvEg3PE33g

强化学习网络结构搜索(二)

https://mp.weixin.qq.com/s/9vGQUqKyf9Ct6im0zv6YiQ

强化学习网络结构搜索(三)

https://mp.weixin.qq.com/s/1XDknbIapmuQi5eb61Jlaw

遗传算法与网络结构搜索

https://mp.weixin.qq.com/s/1zm2iMXD142Ug0_coVIGMg

Darts: 可微结构搜索

https://zhuanlan.zhihu.com/p/266102401

Auto Seg-Loss: 自动损失函数设计

https://mp.weixin.qq.com/s/y3SIK1sAscrWQuZ3mB9lew

手动搜索超参数的一个简单方法

https://mp.weixin.qq.com/s/ACxAAvvKK-DX5UDMe-RsXg

最新《神经架构搜索NAS》教程，33页pdf

https://mp.weixin.qq.com/s/2hSmI8xo1jUHgWrEu3YBIA

一文读懂目前大热的AutoML与NAS

https://mp.weixin.qq.com/s/1JgHE1juxuYEmKO9SXMssA

NAS+Det

https://mp.weixin.qq.com/s/xnqAPh3hlQq3_QDmk2lOLg

NAS在检测中的应用

https://mp.weixin.qq.com/s/nG_Mzlevs9ougnyeJyT38A

一文看懂AutoML

https://mp.weixin.qq.com/s/VdjHp5Tb1fSyV3CQblPQmw

利用NAS寻找最佳GAN：AutoGAN架构搜索方案专为GAN打造

https://mp.weixin.qq.com/s/d60EKUJjYfYfcfhMnaCpPA

基于强化学习的自动搜索

https://mp.weixin.qq.com/s/JC1MlSfR2INd5RqSFolV5A

NAS的挑战和解决方案-一份全面的综述

https://mp.weixin.qq.com/s/Nmkcy2nBczUccwIdMJiH_A

MaskConnect: 探究网络结构搜索中的Module间更好的连接

https://mp.weixin.qq.com/s/of9C3l2cmzAVEt9opIgfKw

损失函数也可以进行自动搜索学习吗？

https://mp.weixin.qq.com/s/OX0C9h5l6xffcQt6HOe_tQ

DetNAS：首个搜索物体检测Backbone的方法
