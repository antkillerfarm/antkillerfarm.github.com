---
layout: post
title:  深度学习（七）——GAN
category: theory 
---

# GAN

## 概况

GAN是“生成对抗网络”（Generative Adversarial Networks）的简称，由2014年还在蒙特利尔读博士的Ian Goodfellow引入深度学习领域。

>注：Ian J. Goodfellow，斯坦福大学本硕+蒙特利尔大学博士。导师是Yoshua Bengio。现为Google研究员。   
>个人主页：   
>http://www.iangoodfellow.com/

论文：

《Generative Adversarial Nets》

教程：

http://www.iangoodfellow.com/slides/2016-12-04-NIPS.pdf

## 通俗解释

对于GAN来说，最通俗的解释就是**“伪造者-鉴别者”**的解释，如艺术画的伪造者和鉴别者。一开始伪造者和鉴别者的水平都不高，但是鉴别者还是比较容易鉴别出伪造者伪造出来的艺术画。但随着伪造者对伪造技术的学习后，其伪造的艺术画会让鉴别者识别错误；或者随着鉴别者对鉴别技术的学习后，能够很简单的鉴别出伪造者伪造的艺术画。这是一个双方不断学习技术，以达到最高的伪造和鉴别水平的过程。

从上面的解释可以看出，GAN实际上一种**零和游戏上的无监督算法**。

## 基本原理

上面的解释虽然通俗，却并未涉及算法的实现。要实现上述原理，至少要解决三个问题：

1.什么是鉴别者。

2.什么是伪造者。

3.如何对抗。



## JS散度

因为KL散度不是对称的，有时候将它对称化，即得到JS散度（Jensen–Shannon divergence）：

$$JS\Big(p_1(x),p_2(x)\Big)=\frac{1}{2}KL\Big(p_1(x)\|p_2(x)\Big)+\frac{1}{2}KL\Big(p_2(x)\|p_1(x)\Big)$$

>注：Claude Elwood Shannon，1916～2001，美国数学家，信息论之父。密歇根大学双学士+MIT博士。先后供职于贝尔实验室和MIT。

>注：Rudolf Otto Sigismund Lipschitz，1832～1903，德国数学家，先后就读于柯尼斯堡大学和柏林大学，导师Dirichlet。波恩大学教授。

## 参考

http://kexue.fm/archives/4439/

互怼的艺术：从零直达WGAN-GP

https://mp.weixin.qq.com/s/xa3F3kCprE6DEQclas4umg

GAN的数学原理

http://www.jianshu.com/p/e2d2d7cbbe49

50行代码实现GAN

http://mp.weixin.qq.com/s/bzwG0QxnP2drqS4RwcZlBg

微软详解：到底什么是生成式对抗网络GAN？

https://mp.weixin.qq.com/s/oCDlhzjOYTIhsr5JuoRCJQ

IRGAN：大一统信息检索模型的博弈竞争

https://mp.weixin.qq.com/s/QacQCrjh3KmrQSMp-G_rEg

贝叶斯生成对抗网络

https://github.com/hindupuravinash/the-gan-zoo

GAN的各种变种。

https://zhuanlan.zhihu.com/p/24897387

GAN的基本原理、应用和走向

https://mp.weixin.qq.com/s/E28lA-fcAQ6Sp6Qv64H3TQ

GAN in NLP

https://mp.weixin.qq.com/s/7-oHa-8Q8ThcctaVOZFfew

Facebook创意生成网络CAN，比GAN更有创造力

https://mp.weixin.qq.com/s/aSQ2-QxbToGF0ROyjxw2yw

萌物生成器：如何使用四种GAN制造猫图

https://mp.weixin.qq.com/s/YUMIL-f019vKpQ84mKS-8g

这篇TensorFlow实例教程文章告诉你GANs为何引爆机器学习？

http://mp.weixin.qq.com/s/UkZdUcdz7h4DqcyjSbNncw

zi2zi：用条件生成对抗网络玩转中文书法，绝妙汉字字体自动生成

http://blog.csdn.net/v_JULY_v/article/details/52683959

没GPU也能玩梵高作画：Ubuntu tensorflow CPU版

https://github.com/cysmith/neural-style-tf

TensorFlow (Python API) implementation of Neural Style.这个项目实现了两张图片的画风融合，非常牛。

https://github.com/jinfagang/pytorch_style_transfer

这个和上面的一样，不过是用pytorch实现的。

http://mp.weixin.qq.com/s/zNmJuevHaagKbyGFdKTwoQ

tensorflow实现基于深度学习的图像补全

https://zhuanlan.zhihu.com/p/25204020

条条大路通罗马LS-GAN：把GAN建立在Lipschitz密度上


