---
layout: post
title:  深度学习（三十）——GAN进阶
category: DL 
---

# GAN进阶

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

https://mp.weixin.qq.com/s/E28lA-fcAQ6Sp6Qv64H3TQ

GAN in NLP

https://mp.weixin.qq.com/s/7-oHa-8Q8ThcctaVOZFfew

Facebook创意生成网络CAN，比GAN更有创造力

https://mp.weixin.qq.com/s/aSQ2-QxbToGF0ROyjxw2yw

萌物生成器：如何使用四种GAN制造猫图

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

https://mp.weixin.qq.com/s/Qzlg1MzRT3josy2RJpQSVg

Image to Image Translation Using GAN

https://mp.weixin.qq.com/s/AswdyjPeKbX7yhAPloP2og

基于对抗学习的生成式对话模型

https://mp.weixin.qq.com/s/sxa0BfXtylHXzjq0YBn-Kg

伯克利图像迁移cycleGAN，猫狗互换效果感人

https://mp.weixin.qq.com/s/YXWTslQXIKVihBb2Bgtafg

GAN在信息检索领域的应用

https://mp.weixin.qq.com/s/0tTLotV-8w2j3VdkH-qjCQ

让机器告诉你故事的结局应该是什么：利用GAN进行故事型常识阅读理解

https://zhuanlan.zhihu.com/p/28488946

AI可能真的要代替插画师了……

https://mp.weixin.qq.com/s/OXN8Y5truLeslX8-UWgqmg

宅男的福音：用GAN自动生成二次元萌妹子

https://mp.weixin.qq.com/s/NFqTpSXtFdP43MBeb3Ovrw

GAN眼中的图像翻译

http://blog.csdn.net/amds123/article/details/70199708

生成对抗网络GAN最近在NLP领域有哪些应用？

https://mp.weixin.qq.com/s/FNQSLPHQSHSkDipREE702A

利用分布鲁棒优化方法应对对抗样本干扰

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

https://mp.weixin.qq.com/s/T--R4c0QfyPS2vrGdyUlOw

UIUC学者构建欺骗检测器的对抗样本！

https://mp.weixin.qq.com/s/jG7r7THhVAnJTYgDeWX2Eg

百度提出使用GAN构建语音识别新框架

https://mp.weixin.qq.com/s/I7axl00VZeCHJpaaITCO2Q

对抗学习GAN提升跨模态检索效果

https://mp.weixin.qq.com/s/OZWih_xfqYdeP-x9MawogA

GAN做图像翻译的一点总结

https://zhuanlan.zhihu.com/p/32103958

GAN调研：多极扩展（跨域和条件的GAN扩展模型调研）

https://mp.weixin.qq.com/s/RZ6UbTgfn30UFaUYNtApRg

如何用Masking GAN让100,000人笑起来？

https://mp.weixin.qq.com/s/OPn4_jmJ7pUg8xa0FZvyiw

超火的漫画线稿上色AI出新版了！无监督训练，效果更美好

https://mp.weixin.qq.com/s/wrxKwY2SvJZc8bRuZ_wF_A

用生成对抗网络给雪人上色，探索人工智能时代的美学

https://mp.weixin.qq.com/s/lPeySqEr3pzPdZqnp1Eh8A

CMU提出对抗生成网络：可实现对人脸识别模型的神经网络攻击

https://mp.weixin.qq.com/s/Dz5c32SnM8-Pdjb9oWxZ_Q

想实现DCGAN？从制作一张门票谈起！

https://mp.weixin.qq.com/s/f5dGUJw3bv8WppPZVzqyAA

反GAN传统，Petuum自动驾驶新研究提出从复杂真实图像生成简单虚拟表征以预测驾驶指令

https://mp.weixin.qq.com/s/SvIC6DXUKH93jU9R-7fcyw

CNN对抗补丁之谜

https://mp.weixin.qq.com/s/V1XX8WWstR00m_OimMrDDw

微软亚洲研究院论文解读：GAN在网络特征学习中的应用

https://mp.weixin.qq.com/s/YNkx03D2Ok2xyMpPI0XXKg

Petuum新研究提出形义感知型Grad-GAN：可基于虚拟游戏生成更具真实感的城市场景

http://mp.weixin.qq.com/s/RlpI3hCKjcJ8VHSbBXf84g

用生成对抗网络解决NLP问题：谷歌大脑提出MaskGAN

https://zhuanlan.zhihu.com/p/32135185

从一篇ICLR'2017被拒论文谈起：行走在GAN的Latent Space

https://mp.weixin.qq.com/s/9N_3JkNEPdXQgFn0S7MqnA

走进深度生成模型：变分自动编码器（VAE）和生成对抗网络（GAN）

https://mp.weixin.qq.com/s/SPAbCZloiHp2mtGu0IDWDA

DeepMind高级研究员：重新理解GAN，最新算法、技巧及应用


