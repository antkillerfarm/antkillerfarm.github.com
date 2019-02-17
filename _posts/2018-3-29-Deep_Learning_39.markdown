---
layout: post
title:  深度学习（三十九）——GAN进阶（1）
category: DL 
---

# GAN进阶

## GAN的发展

最早的GAN出现在2014年6月，但直到2015年底，也只有5个变种，发展并不迅速。

2016年，GAN开始发力，年底时已有52个变种。2017年6月底，更达到142个变种。

![](/images/article/GAN_structure.png)

上图的源地址：

https://github.com/hwalsuklee/tensorflow-generative-model-collections

《Generating Reasonable and Diversified Story Ending Using Sequence to Sequence Model with Adversarial Training》，这篇论文提出的Seq2Seq+RL+GAN的组合模型，也算是让我开眼界了。

![](/images/article/Seq2Seq_RL_GAN.png)

参考：

https://github.com/hindupuravinash/the-gan-zoo

GAN的各种变种。

https://zhuanlan.zhihu.com/p/34016536

历史最全GAN网络及其各种变体整理

https://mp.weixin.qq.com/s/rE_RYklz8G_Etg5Gstlhew

十款神奇的GAN，总有一个适合你！

https://mp.weixin.qq.com/s/IjlIT-3FVY7IfYzNDtkkgg

谷歌大脑发布GAN全景图：看百家争鸣的生成对抗网络

https://mp.weixin.qq.com/s/Y4Ags-yupq__gQZmparhsg

COLING 2018⽤对抗增强的端到端模型⽣成合理且多样的故事结尾

https://mp.weixin.qq.com/s/QRmy8f88eJcdp1xCxCI8bg

历数GAN的5大基本结构

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

https://mp.weixin.qq.com/s/ecqPzcSa75U9n4BcvnNJ_Q

GAN模式崩溃的理论解释

## GAN的评估指标

尽管可用的GAN模型非常多，但对它们的评估仍然主要是定性评估，通常需要借助人工检验生成图像的视觉保真度来进行。此类评估非常耗时，且主观性较强、具备一定误导性。鉴于定性评估的内在缺陷，恰当的定量评估指标对于GAN的发展和更好模型的设计至关重要。

参考：

https://mp.weixin.qq.com/s/EPwsQ_005CYNlCK_82SYWQ

六种GAN评估指标的综合评估实验，迈向定量评估GAN的重要一步

## 参考

https://blog.csdn.net/liuxiao214/article/category/6940697

某GAN专栏

https://mp.weixin.qq.com/s/oCDlhzjOYTIhsr5JuoRCJQ

IRGAN：大一统信息检索模型的博弈竞争

https://mp.weixin.qq.com/s/4Daw-2aRmzcCMtxdvB3uYQ

IRGAN ：生成对抗网络在搜狗图片搜索排序中的应用

https://zhuanlan.zhihu.com/p/28342644

CycleGAN的原理与实验详解

https://zhuanlan.zhihu.com/p/30532830

眼见已不为实，迄今最真实的GAN：Progressive Growing of GANs

https://mp.weixin.qq.com/s/ouLWl623r_YaZdIdpqSWcw

深度卷积对抗生成网络(DCGAN)实战

https://mp.weixin.qq.com/s/PkZ069S8ysY_JCQx1nzfGg

NVIDIA新作解读：用GAN生成前所未有的高清图像（附PyTorch复现）

https://mp.weixin.qq.com/s/Q_1IUS-65ZAFt9w0RlZUpw

谷歌开源TFGAN：轻量级生成对抗网络工具库

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

https://mp.weixin.qq.com/s/h7lrJYQ_RqJDako8UoYK-A

六种改进均未超越原版：谷歌新研究对GAN现状提出质疑

https://mp.weixin.qq.com/s/dwEHorYSJuX9JapIYLHiXg

BicycleGAN：图像转换多样化，大幅提升pix2pix生成图像效果

https://mp.weixin.qq.com/s/W4WVHMIMKPNvt5lcCe1v4g

半监督学习需要Bad GAN，清华特奖学霸与苹果AI总监提出

https://mp.weixin.qq.com/s/_PlISSOaowgvVW5msa7GlQ

条件GAN高分辨率图像合成与语义编辑pix2pixHD

https://mp.weixin.qq.com/s/PoSA6JXYE_OexEoJYzaX4A

利用条件GANs的pix2pix进化版：高分辨率图像合成和语义操作

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

https://mp.weixin.qq.com/s/EI57G3ph9WCrT7kudOAjvQ

Petuum提出对偶运动生成对抗网络：可合成逼真的视频未来帧和流

https://mp.weixin.qq.com/s/M-YeUqvcSVHj_MbCS_c-6Q

无平行文本照样破解密码，CipherGAN有望提升机器翻译水平

https://mp.weixin.qq.com/s/uJEGXNvBVJfsxYJUQCz4DQ

DA-GAN技术：计算机帮你创造奇妙“新物种”

https://mp.weixin.qq.com/s/b8g_wNqi4dD5x9whj9ZN4A

争议、流派，有关GAN的一切：Ian Goodfellow Q&A

https://mp.weixin.qq.com/s/9N-eOyRseMc6zVH1_VBE4w

斯坦福联合谷歌使用图卷积和GAN从场景图中生成图像

https://mp.weixin.qq.com/s/V8v9eeP4HFH-TfIKUio6Iw

上交大推出新型无监督生成模型算法CoT，性能比GAN稳定！

https://mp.weixin.qq.com/s/Gjb-PkMqUIIV81Na6Za_BQ

超全的GAN PyTorch+Keras实现集合

https://mp.weixin.qq.com/s/Cb3C6Q5aWSjsNFs5hqD6kw

阿里提出应用LocalizedGAN进行半监督训练

https://mp.weixin.qq.com/s/Y8D0gr1ybQ48H0bDpPvD8w

二次元萌妹高清舞姿随心变，换装只需一瞬间：又是GAN立功了

https://mp.weixin.qq.com/s/H_EcHi9BRur214rktocGbg

怎样用GAN生成各种胖吉猫？谷歌大脑程序员教你撩妹神技

https://mp.weixin.qq.com/s/FDyN6fblDFGOSNRFs5OYqA

什么都能GAN，无监督神经网络翻译新方法

https://mp.weixin.qq.com/s/DYSnAwP9xt-p0ihsEtKm1Q

TensorFlow实现StarGAN代码全部开源，1天训练完

https://mp.weixin.qq.com/s/se_2Ci4eE5fxiiG4LZrv8w

怎样用可交互对抗网络增强人类创造力

https://mp.weixin.qq.com/s/29Ror1CKFubB0n38cihMqQ

六种GAN评估指标的综合评估实验，迈向定量评估GAN的重要一步

https://mp.weixin.qq.com/s/0I8jLO3srXC3fUdoMC585w

一些fancy的GAN应用

https://mp.weixin.qq.com/s/2cY3NCtMYiRlLayB32wl-w

下一个GAN？OpenAI提出可逆生成模型Glow

https://mp.weixin.qq.com/s/1cUM8_AUZ40JWGI37HsqFw

换脸效果媲美GAN！一文解析OpenAI最新流生成模型Glow

https://mp.weixin.qq.com/s/kD_163rjGRp4rXOdIPCvRA

亲历IJCAI 2018，为什么北京大学SentiGAN能获杰出论文？

https://mp.weixin.qq.com/s/v23rSjIKyVEAGCN8rEtsfg

伯克利新论文：合成GAN（Compositional GAN）

https://mp.weixin.qq.com/s/ddR7dwwBZnu3knKmfDWjCg

斯坦福大学PH.D Aditya Grover：115页Slides带你领略深度生成模型（Deep Generative Model）全貌

https://mp.weixin.qq.com/s/QjEShm__Rq0n7n_sdc6GNg

Ryan P.Adams教授：深度概率生成模型—156页普林斯顿教程带你回顾深度生成模型最新发展脉络

https://mp.weixin.qq.com/s/DSHuCNQWKN3pLozZ8i16hQ

卡成PPT不开心？GAN也能生成流畅的连续表情了

https://mp.weixin.qq.com/s/5S6TsyT6dwXP3cTE_KQleg

把酱油瓶放进菜篮子：UC Berkeley提出高度逼真的物体组合网络Compositional GAN

https://mp.weixin.qq.com/s/IE5h6AiAYhA1nBvsFiGUHA

GAN最新进展：8大技巧提高稳定性
