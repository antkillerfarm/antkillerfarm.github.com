---
layout: post
title:  深度学习（九）——fine-tuning
category: DL 
---

# GAN（续）

## GAN的发展

最早的GAN出现在2014年6月，但直到2015年底，也只有5个变种，发展并不迅速。

2016年，GAN开始发力，年底时已有52个变种。2017年6月底，更达到142个变种。

![](/images/article/GAN_structure.png)

上图的源地址：

https://github.com/hwalsuklee/tensorflow-generative-model-collections

参考：

https://github.com/hindupuravinash/the-gan-zoo

GAN的各种变种。

## GAN的理论解释

顾险峰教授对GAN提出了自己的理论解释。

>顾险峰，1989年考入清华大学计算机科学与技术系。1992年获得清华大学特等奖学金。后于美国哈佛大学获得计算机博士学位，师从国际著名微分几何大师丘成桐先生。目前为美国纽约州立大学石溪分校计算机系终身教授。

参考：

https://mp.weixin.qq.com/s/7O0AKIUVYK7HRyvdRbUVkg

虚构的对抗，GAN with the wind

https://mp.weixin.qq.com/s/trvMOTXNs7L6fSmTkZXwsA

看穿机器学习（W-GAN模型）的黑箱

https://mp.weixin.qq.com/s/thcxsBVttSIEzVNLQlAVCA

看穿机器学习的黑箱（II）

https://mp.weixin.qq.com/s/Jx0o17CwlIVcRV22PXk4wQ

看穿机器学习的黑箱（III）

## 参考

https://mp.weixin.qq.com/s/xa3F3kCprE6DEQclas4umg

GAN的数学原理

http://www.jianshu.com/p/e2d2d7cbbe49

50行代码实现GAN

https://mp.weixin.qq.com/s/YnOF9CCUFvtaiTY8HXYOuw

深入浅出：GAN原理与应用入门介绍

http://blog.csdn.net/u011534057/article/category/6396518

GAN系列blog

https://mp.weixin.qq.com/s/4CypEZscTfmUzOk-p_rZog

生成对抗网络初学入门：一文读懂GAN的基本原理

http://mp.weixin.qq.com/s/bzwG0QxnP2drqS4RwcZlBg

微软详解：到底什么是生成式对抗网络GAN？

https://mp.weixin.qq.com/s/GobKiuxgZv0-ufSRBpTcIA

Ian Goodfellow ICCV2017演讲：解读GAN的原理与应用

https://mp.weixin.qq.com/s/TIgRVbnZYtrGUCDNLcL1uw

GAN的入门与实践

https://mp.weixin.qq.com/s/oCDlhzjOYTIhsr5JuoRCJQ

IRGAN：大一统信息检索模型的博弈竞争

https://mp.weixin.qq.com/s/4Daw-2aRmzcCMtxdvB3uYQ

IRGAN ：生成对抗网络在搜狗图片搜索排序中的应用

https://mp.weixin.qq.com/s/QacQCrjh3KmrQSMp-G_rEg

贝叶斯生成对抗网络

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

https://zhuanlan.zhihu.com/p/27199954

用GAN去除动作片中的马赛克和衣服

https://zhuanlan.zhihu.com/p/27012520

从头开始GAN

https://mp.weixin.qq.com/s/Qzlg1MzRT3josy2RJpQSVg

Image to Image Translation Using GAN

https://mp.weixin.qq.com/s/AswdyjPeKbX7yhAPloP2og

基于对抗学习的生成式对话模型

https://mp.weixin.qq.com/s/uyn41vKKoptXPZXBP2vVDQ

生成对抗网络（GAN）之MNIST数据生成

https://mp.weixin.qq.com/s/sxa0BfXtylHXzjq0YBn-Kg

伯克利图像迁移cycleGAN，猫狗互换效果感人

https://mp.weixin.qq.com/s/aMfPBl6E5SxckQdSAGTkBg

Pytorch教程：Facebook发布的LR-GAN如何生成图像？

https://zhuanlan.zhihu.com/p/28342644

CycleGAN的原理与实验详解

https://mp.weixin.qq.com/s/YXWTslQXIKVihBb2Bgtafg

GAN在信息检索领域的应用

http://mp.weixin.qq.com/s/21CN4hAA6p7ZjWsO1sT2rA

一文看懂生成式对抗网络GANs：介绍指南及前景展望

https://mp.weixin.qq.com/s/YLys6L9WT7eCC-xGr1j0Iw

带多分类判别器的GAN模型

https://mp.weixin.qq.com/s/0tTLotV-8w2j3VdkH-qjCQ

让机器告诉你故事的结局应该是什么：利用GAN进行故事型常识阅读理解

https://mp.weixin.qq.com/s/lqQeCpLQVqSdJPWx0oxs2g

例解生成对抗网络

https://mp.weixin.qq.com/s/fMtuJbWG_d9zyCZ0oYyX_w

经得住考验的“假图片”：用TensorFlow为神经网络生成对抗样本

https://zhuanlan.zhihu.com/p/28488946

AI可能真的要代替插画师了……

https://mp.weixin.qq.com/s/OXN8Y5truLeslX8-UWgqmg

宅男的福音：用GAN自动生成二次元萌妹子

https://mp.weixin.qq.com/s/LAS0KgPiVekGdQXbqlw1cQ

深度学习的三大生成模型：VAE、GAN、GAN的变种

https://mp.weixin.qq.com/s/N7YU-YeXiVX7gSB-mzYgnw

生成式对抗网络GAN的研究进展与展望

https://mp.weixin.qq.com/s/gDzti2DISq_cwGbP5T7ICQ

聊聊对抗自编码器

https://mp.weixin.qq.com/s/3Aq1HXpBzgNdcB130tCKbQ

GAN网络图像翻译机：图像复原、模糊变清晰、素描变彩图

https://mp.weixin.qq.com/s/mPtv1fQd0NBgdY2b_ALNTQ

机器之心GitHub项目：GAN完整理论推导与实现，Perfect！

https://mp.weixin.qq.com/s/NFqTpSXtFdP43MBeb3Ovrw

GAN眼中的图像翻译

http://blog.csdn.net/amds123/article/details/70199708

生成对抗网络GAN最近在NLP领域有哪些应用？

https://mp.weixin.qq.com/s/uUSq3irEIcBM35JCYGDPfw

生成对抗网络综述：从架构到训练技巧，看这篇论文就够了

https://mp.weixin.qq.com/s/nuT6Glyx0-tU7WJyoPji9w

GANs正在多个层面有所突破

https://zhuanlan.zhihu.com/p/30532830

眼见已不为实，迄今最真实的GAN：Progressive Growing of GANs

https://mp.weixin.qq.com/s/d_W0O7LNqlBuZV87Ou9uqw

训练GAN的16个技巧

https://mp.weixin.qq.com/s/6AtZZ434HehQSf_YgbylTw

用100元的支票骗到100万：看看对抗性攻击是怎么为非作歹的

https://mp.weixin.qq.com/s/df51nYaA-uz7vd4WbBuE8g

GAN货：生成对抗网络知识资料全集

https://mp.weixin.qq.com/s/-j4p7nUF-rCGk6yK0nccvw

机器人也会画漫画

https://mp.weixin.qq.com/s/ouLWl623r_YaZdIdpqSWcw

深度卷积对抗生成网络(DCGAN)实战

https://mp.weixin.qq.com/s/jG7r7THhVAnJTYgDeWX2Eg

百度提出使用GAN构建语音识别新框架

https://mp.weixin.qq.com/s/PkZ069S8ysY_JCQx1nzfGg

NVIDIA新作解读：用GAN生成前所未有的高清图像（附PyTorch复现）

https://mp.weixin.qq.com/s/cI4xZOw6eL0w9sz9Q2mSCw

Ian Goodfellow盛赞：一个GAN生成ImageNet全部1000类物体

https://mp.weixin.qq.com/s/LHCEh4BPZ_qbSKaar19nNg

十种主流GANs，我该如何选择？

https://mp.weixin.qq.com/s/rPDvLnG4MBDRUMCWs2fjcQ

最新StarGAN对抗生成网络实现多领域图像变换

https://mp.weixin.qq.com/s/h7lrJYQ_RqJDako8UoYK-A

六种改进均未超越原版：谷歌新研究对GAN现状提出质疑

https://mp.weixin.qq.com/s/dwEHorYSJuX9JapIYLHiXg

BicycleGAN：图像转换多样化，大幅提升pix2pix生成图像效果

https://mp.weixin.qq.com/s/VldzlYg5AfDRho8bsROL_g

HashGAN:基于注意力机制的深度对抗哈希模型提升跨模态检索效果

https://mp.weixin.qq.com/s/W4WVHMIMKPNvt5lcCe1v4g

半监督学习需要Bad GAN，清华特奖学霸与苹果AI总监提出

https://mp.weixin.qq.com/s/_PlISSOaowgvVW5msa7GlQ

条件GAN高分辨率图像合成与语义编辑pix2pixHD

https://mp.weixin.qq.com/s/PoSA6JXYE_OexEoJYzaX4A

利用条件GANs的pix2pix进化版：高分辨率图像合成和语义操作

https://mp.weixin.qq.com/s/RAlQVWMBYeddG2Mvu2bF4w

生成对抗网络（GANs）最新家谱：为你揭秘GANs的前世今生

https://mp.weixin.qq.com/s/FNQSLPHQSHSkDipREE702A

利用分布鲁棒优化方法应对对抗样本干扰

https://mp.weixin.qq.com/s/uJlgx9Bq-XI49l8wwmdIsw

用GAN让晴天下大雨，小猫变狮子，黑夜转白天

https://mp.weixin.qq.com/s/hjsFBVE3_IiKTZSesa44ug

GAN系列学习(1)——前生今世

https://mp.weixin.qq.com/s/T--R4c0QfyPS2vrGdyUlOw

UIUC学者构建欺骗检测器的对抗样本！

https://mp.weixin.qq.com/s/Q_1IUS-65ZAFt9w0RlZUpw

谷歌开源TFGAN：轻量级生成对抗网络工具库

https://mp.weixin.qq.com/s/BCA7MmYnivuGbwyjHqDQUw

手把手教你实现GAN半监督学习

# fine-tuning

fine-tuning和迁移学习虽然是两个不同的概念。但局限到CNN的训练领域，基本可以将fine-tuning看作是一种迁移学习的方法。

举个例子，假设今天老板给你一个新的数据集，让你做一下图片分类，这个数据集是关于Flowers的。问题是，数据集中flower的类别很少，数据集中的数据也不多，你发现从零训练开始训练CNN的效果很差，很容易过拟合。怎么办呢，于是你想到了使用Transfer Learning，用别人已经训练好的Imagenet的模型来做。

由于ImageNet数以百万计带标签的训练集数据，使得如CaffeNet之类的预训练的模型具有非常强大的泛化能力，这些预训练的模型的中间层包含非常多一般性的视觉元素，我们只需要对他的后几层进行微调，再应用到我们的数据上，通常就可以得到非常好的结果。最重要的是，**在目标任务上达到很高performance所需要的数据的量相对很少**。

虽然从理论角度尚无法完全解释fine-tuning的原理，但是还是可以给出一些直观的解释。我们知道，CNN越靠近输入端，其抽取的图像特征越原始。比如最初的一层通常只能抽取一些线条之类的元素。越上层，其特征越抽象。

而现实的图像无论多么复杂，总是由简单特征拼凑而成的。因此，无论最终的分类结果差异如何巨大，其底层的图像特征却几乎一致。

![](/images/article/trans_learn.png)

fine-tuning也是图像目标检测、语义分割的基础。

参考：

https://zhuanlan.zhihu.com/p/22624331

fine-tuning:利用已有模型训练其他数据集

http://www.cnblogs.com/louyihang-loves-baiyan/p/5038758.html

Caffe fine-tuning微调网络

http://blog.csdn.net/sinat_26917383/article/details/54999868

caffe中fine-tuning模型三重天（函数详解、框架简述）+微调技巧

http://yongyuan.name/blog/layer-selection-and-finetune-for-cbir.html

图像检索：layer选择与fine-tuning性能提升验证

h1ttps://www.zhihu.com/question/49534423

迁移学习与fine-tuning有什么区别？

