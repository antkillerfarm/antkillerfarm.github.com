---
layout: post
title:  深度学习（四十三）——图像变换, Recursive NN, Spiking Neuron Networks, 模型压缩与加速（2）
category: DL 
---

# 图像变换

## 语义分割逆变换

![](/images/img2/SCAN.png)

参考：

https://mp.weixin.qq.com/s/tusm3CYqKKep-BJQ0Vp6Ww

腾讯AI lab & 复旦大学合作提出无监督高分辨率的图像到图像转换方法SCAN

https://mp.weixin.qq.com/s/X96oI-duHwyCFAR79inlig

真实到可怕！英伟达MIT造出马良的神笔

## 图像修复

https://mp.weixin.qq.com/s/33VKfq-jFHfn9GBQzFYq0Q

震撼！英伟达用深度学习做图像修复，毫无ps痕迹

https://zhuanlan.zhihu.com/p/50620348

基于内容感知生成模型的图像修复，论文翻译、解读

## 参考

https://mp.weixin.qq.com/s/YbRo7TJ3j4dloSmKX40uAA

Image-to-Image的论文汇总

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

https://mp.weixin.qq.com/s?__biz=MzIzNjc1NzUzMw==&mid=2247504381&idx=2&sn=465027cc2a8f75a7f6ddf400830f61e0

这个品质超高的漫画自动上色AI，让你DIY出喜欢的配色

https://mp.weixin.qq.com/s/3Aq1HXpBzgNdcB130tCKbQ

GAN网络图像翻译机：图像复原、模糊变清晰、素描变彩图

https://mp.weixin.qq.com/s/djkjAfUO_DefTP2drzY_iQ

在《绝地求生》中玩《堡垒之夜》！ 深度学习帮你转换画风

https://mp.weixin.qq.com/s/X8osUSPROJqGVTvw0gieDQ

T2T：利用StackGAN和ProGAN从文本生成人脸

https://mp.weixin.qq.com/s/moDVf7h8Q2S1SL0IuxXQtQ

算法音乐往事：二次元女神“初音未来”诞生记

https://mp.weixin.qq.com/s/4UH-XWyIxYHq_ErRfkzzsQ

与神经网络相比，你对P图一无所知

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

https://mp.weixin.qq.com/s?__biz=MzI3MTA0MTk1MA==&mid=2652026913&idx=2&sn=fc38fa94211c277d616f4f37aa6d8b5d

pix2pix 3D版：几笔线条生成超炫猫咪霹雳舞！

https://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650748774&idx=4&sn=efb794e4e418493f17138e7fa7eb84ab

这种两阶段深度着色模型，为黑白照披上了彩衣

https://mp.weixin.qq.com/s/3lrCRiq5zuX9yWxiQRQE9A

CartoonGAN不靠手绘，也能成为漫画达人

https://mp.weixin.qq.com/s/BWBrso7O1O3Rfxa4QWZH4g

分分钟学会基于深度学习的图像真实风格迁移！

https://mp.weixin.qq.com/s?__biz=MzIzNjc1NzUzMw==&mid=2247505181&idx=1&sn=2e817ec2ae918fd85c0ebcf85e810997

惊！史上最佳GAN现身，超真实AI假照片，行家们都沸腾了

https://mp.weixin.qq.com/s/b3EVdPGY2jxliwdbbB1kcQ

“史上最强GAN图像生成器”BigGAN的demo出了！

https://mp.weixin.qq.com/s/kSyXd5dgdEcqupDeouhObQ

BigGAN论文解读

https://mp.weixin.qq.com/s/MPdGF2-cJbNcvaEx-2BnFg

把2D公路变成3D飞车游戏，MIT、清华打破图像编辑的次元壁

https://mp.weixin.qq.com/s/h6sktVd1ywCCncUb9iwyFQ

中科院自动化所：高清真实图像生成领域及GAN研究在人脸识别领域的进展

https://mp.weixin.qq.com/s/9LvP3AH1PJhjrokTGg3blA

心中无码：这是一个能自动脑补漫画空缺部分的AI项目

https://mp.weixin.qq.com/s/cBTnX1lNHJhCMDX926aujA

想怎么GAN就怎么GAN，一键拯救发际线

https://mp.weixin.qq.com/s/HinUtoYNq_e8HYBqPKozsQ

GAN：艺术家眼里生成作品的创作利器

https://mp.weixin.qq.com/s/Cyy4FgVZyVN-4KvG-9pHMA

品质超高！超火的漫画线稿上色AI最新版来了

https://mp.weixin.qq.com/s/lp769k6wsyh-991ACf5upw

计算机也会ps图片：TL-GAN

https://mp.weixin.qq.com/s/OmSb1dl9smyZt_uASOcofQ

人人都是画家：朱俊彦&周博磊等人的GAN画笔帮你开启艺术生涯

https://mp.weixin.qq.com/s/7xGtUHfk_FNpDTksLxKaHw

杨超越的声音+高晓松的脸~如此酸爽的技术，你值得拥有！

https://mp.weixin.qq.com/s/y9h6xkK9Keq4UrdpGJS-4Q

这些假脸实在太逼真了！英伟达造出新一代GAN，生成壁纸级高清大图毫无破绽

# Recursive NN

http://blog.csdn.net/qq_26609915/article/details/52119512

递归神经网络（recursive NN）结合自编码（Autoencode）实现句子建模

http://blog.csdn.net/mengmengz07/article/details/51348554

recursive neural network梳理

# Spiking Neuron Networks

除了基于BP算法的NN之外，Spiking Neuron Networks也是一大类NN。Spiking NN和人脑结构更相似，功耗也更小，但是相关训练和数据量化的算法尚不成熟，属于潜力股。

![](/images/img2/BrainChip_Fig2.gif)

参考：

https://homepages.cwi.nl/~sbohte/publication/paugam_moisy_bohte_SNNChapter.pdf

Computing with Spiking Neuron Networks

https://mp.weixin.qq.com/s/6dpKSaLFVo-ge4gtbG8GQg

简述脉冲神经网络SNN：下一代神经网络

https://mp.weixin.qq.com/s/0n50YO1jIv_mxqe0EeS6kw

综述AI未来：神经科学启发的类脑计算

https://mp.weixin.qq.com/s/5KA7jtlRmnXxijGQhU1k4A

DeepMind哈萨比斯狂推的神经科学，入门需要看什么书？

https://mp.weixin.qq.com/s/TWdeHVCgEf54STvdA1QUPg

DeepMind哈萨比斯长文：伟大的AI离不开神经科学

https://mp.weixin.qq.com/s/8ibcyvyBLYArAMhQElqRzg

Cell研究揭示生物神经元强大新特性，是时候设计更复杂的神经网络了！

https://mp.weixin.qq.com/s/cb6JBlb11xW0Xw0RWI4vFA

浙大&川大提出脉冲版ResNet：继承ResNet优势，实现当前最佳

# 模型压缩与加速

https://mp.weixin.qq.com/s/nEMvoiqImd0RxrskIH7c9A

仅17 KB、一万个权重的微型风格迁移网络！

https://mp.weixin.qq.com/s/pc8fJx5StxnX9it2AVU5NA

基于手机系统的实时目标检测

https://mp.weixin.qq.com/s/6wzmyhIvUVeAN4Xjfhb1Yw

论文解读：Channel pruning for Accelerating Very Deep Neural Networks

https://mp.weixin.qq.com/s/-X7NYTzOzljzOaQL7_jOkw

惊呆了！速度高达15000fps的人脸检测算法！

https://mp.weixin.qq.com/s/6eyEMW9dVBR5cZrHxn8iqA

腾讯AI Lab详解3大热点：模型压缩、自动机器学习及最优化算法
