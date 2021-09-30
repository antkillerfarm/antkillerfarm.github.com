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

# AutoDL++++

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

# 深度信息检索+

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
