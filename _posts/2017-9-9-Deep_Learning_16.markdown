---
layout: post
title:  深度学习（十六）——SENet, 李飞飞, 深度ISP, RNN进阶, CNN进阶
category: DL 
---

# SENet

无论是在Inception、DenseNet或者ShuffleNet里面，我们对所有通道产生的特征都是不分权重直接结合的，那为什么要认为所有通道的特征对模型的作用就是相等的呢？这是一个好问题，于是，ImageNet2017冠军SEnet就出来了。

论文：

《Squeeze-and-Excitation Networks》

代码：

https://github.com/hujie-frank/SENet

Sequeeze-and-Excitation(SE) block并不是一个完整的网络结构，而是一个子结构，可以嵌到其他分类或检测模型中。

![](/images/img2/SENet.png)

上图就是SE block的示意图。其步骤如下：

1.转换操作$$F_{tr}$$。这一步就是普通的卷积操作，将输入tensor的shape由$$W'\times H'\times C'$$变为$$W\times H\times C$$。

2.Squeeze操作。

$$z_c = F_{sq}(u_c) = \frac{1}{H\times W}\sum_{i=1}^H \sum_{j=1}^W u_c(i,j)$$

这实际上就是一个global average pooling。

3.Excitation操作。

$$s=F_{ex}(z,W) = \sigma(g(z,W)) = \sigma(W_2 \sigma(W_1 z))$$

其中，$$W_1$$的维度是$$C/r \times C$$，这个r是一个缩放参数，在文中取的是16，这个参数的目的是为了减少channel个数从而降低计算量。

$$W_2$$的维度是$$C \times C/r$$，这样s的维度就恢复到$$1 \times 1 \times C$$，正好和z一致。

4.channel-wise multiplication。

$$\tilde{x_c} = F_{scale}(u_c, s_c)=s_c \cdot u_c$$

![](/images/img2/SENet_2.png)

![](/images/img2/SENet_3.png)

上面两图演示了如何将SE block嵌入网络的办法。

参考：

https://mp.weixin.qq.com/s/tLqsWWhzUU6TkDbhnxxZow

Momenta详解ImageNet 2017夺冠架构SENet

http://blog.csdn.net/u014380165/article/details/78006626

SENet（Squeeze-and-Excitation Networks）算法笔记

# 李飞飞

## AI大佬

李飞飞是吴恩达之后的华裔AI新大佬。巧合的是，他们都是斯坦福AP+AI lab的主任，只不过吴是李的前任而已。

**李飞飞（Fei-Fei Li）**，1976年生，成都人，16岁移民美国。普林斯顿大学本科（1995～1999）+加州理工学院博士（2001～2005）。先后执教于UIUC、普林斯顿、斯坦福等学校。

个人主页：

http://vision.stanford.edu/feifeili/

她的老公Silvio Savarese，也是斯坦福的AP。

## 大佬的门徒

比如可爱的妹子**Serena Yeung**。这个妹子是斯坦福的本硕博。出身不详，但从姓名的英文拼法来看，应该是美国土生的华裔。Yeung是杨、阳、羊等姓的传统英文拼法，但显然不是大陆推行的拼音拼法。（可以对比的是Fei-Fei Li和Bruce Lee，对于同一个姓的不同拼法。）

个人主页：

http://ai.stanford.edu/~syyeung/

还有当红的“辣子鸡”：**Andrej Karpathy**，多伦多大学本科（2009）+英属不列颠哥伦比亚大学硕士（2011）+斯坦福博士（2015）。现任特斯拉AI总监。

吐槽一下：英属不列颠哥伦比亚大学其实是加拿大的一所大学。

个人主页：

http://cs.stanford.edu/people/karpathy/

Andrej Karpathy建了一个检索arxiv的网站，主要搜集了近3年来的ML/DL领域的论文。网址：

http://www.arxiv-sanity.com/

**李佳（Jia Li）**，李飞飞的开山大弟子，追随她从UIUC、普林斯顿到斯坦福。目前又追随其到Google。大约是知道自己的名字是个大路货，她的笔名叫做Li-Jia Li。

个人主页：

http://vision.stanford.edu/lijiali/

## 学神

应该说李飞飞和吴恩达都是万里挑一的超卓人物，但是和学神还是有所差距。下面是两个80后的华裔学神，他们都已经是正教授了：

**尹希**，1983年生，哈佛大学物理系教授。

**张锋**，1982年生，MIT教授，生物学家。

这两个人都是有机会挑战诺奖的人，而李和吴暂时还没有这个可能性。

## 网红

这里收录了一些非李飞飞门下的AI网红。

**Zachary Chase Lipton**，1985年生，哥伦比亚大学本科+UCSD博士，CMU的AP。他的另一身份——Jazz歌手，可比他的学术成就知名多了。

个人主页：

http://zacklipton.com/

# 深度ISP

## 数据集

### HDR+

HDR+是一个使用连拍摄影生成更好的图像的数据集。

官网：

http://hdrplusdata.org

参考：

https://zhuanlan.zhihu.com/p/34391353

机器感知Google推出HDR+连拍摄影数据集

### HDRNet

HDRNet是一个Image Enhancement方面的数据集。

官网：

https://groups.csail.mit.edu/graphics/hdrnet/

## 参考

https://mp.weixin.qq.com/s/wA85XFQXeypuoqFnmN2P4g

降噪的新时代

https://mp.weixin.qq.com/s/919VEvennHEG3iXKkMZoQQ

不止是去噪---从去噪看AI ISP的趋势

https://mp.weixin.qq.com/s/1HA6XKnWpqVd8k7IIfzB7w

利用卷积自编码器对图片进行降噪

https://zhuanlan.zhihu.com/p/39512000

Noise2Noise：图像降噪，无需干净样本

https://mp.weixin.qq.com/s/_tvOQPvybqmvLF19kHcbFg

北大开源ECCV2018深度去雨算法：RESCAN

https://mp.weixin.qq.com/s/Wdxkvlz4nLbJS_gWqHwMjw

无需额外硬件，全卷积网络让机器学习学会夜视能力

https://mp.weixin.qq.com/s/iH7gbRn4opLsWgKWoVFpBA

腾讯优图&港科大提出较大前景运动下的深度高动态范围成像

https://mp.weixin.qq.com/s/WXVZkqCGlj6ym5YrSZS3Vg

谷歌普林斯顿提出首个端到端立体双目系统深度学习方案

https://mp.weixin.qq.com/s/9yfTO2jHz69-k1MsUGIM0Q

双目立体放大！谷歌刚刚开源的这篇论文可能会成为手机双摄的新玩法

# RNN进阶

## IndRNN

https://mp.weixin.qq.com/s/cAqpclkkeVrTiifz07HC1g

新型循环神经网络IndRNN：可构建更长更深的RNN

https://mp.weixin.qq.com/s/7-K-nZTijoYCaprRNYXxFg

新型RNN：将层内神经元相互独立以提高长程记忆

## 参考

https://mp.weixin.qq.com/s/SeR_zNZTu4t7kqB6ltNrmQ

从循环到卷积，探索序列建模的奥秘

https://mp.weixin.qq.com/s/_q69BV1r46S9X5wnLuFPSw

关于序列建模，是时候抛弃RNN和LSTM了

https://mp.weixin.qq.com/s/mIuAn4G9l3AKFAswpbaQdA

时间卷积网络（TCN）将取代RNN成为NLP预测领域王者

https://mp.weixin.qq.com/s/m5GRNp6qDfVfC0mkQ4m4Yw

神经语言模型如何利用上下文信息：长距离上下文的词序并不重要

https://mp.weixin.qq.com/s/kuoUnt2Vhz9NhfnNqMFAhQ

DeepMind提出关系RNN：构建关系推理模块，强化学习利器

https://mp.weixin.qq.com/s/wfOzCxe3L2t11VguYLGC9Q

上海交大搞出SRNN，比普通RNN也就快135倍

https://mp.weixin.qq.com/s/f0sv7c-H5o5L_wy2sUonUQ

CNN取代RNN？当序列建模不再需要循环网络

https://mp.weixin.qq.com/s/h3fF6Zvr1rSzSMpqdu8B0A

电子科大提出BT-RNN：替代全连接操作而大幅度提升LSTM效率

https://mp.weixin.qq.com/s/OgN4rVDKH5WABIaRY7CHog

如何让RNN神经元拥有基础通用的注意力能力

https://mp.weixin.qq.com/s/KBLCrupGIuPa5nVrxcS5WQ

新研究将GRU简化成单门架构，或更适用于语音识别

https://mp.weixin.qq.com/s/kQozftKd_n_kYIF7KKCc8g

短视频那么多，快手如何利用GRU实现各种炫酷的语音应用

https://mp.weixin.qq.com/s/xwuM2Vj8G7UyuEyzTyO13A

将CNN与RNN组合使用，天才还是错乱？

https://mp.weixin.qq.com/s/c7XkzjLH1n5EtqdQik618g

Dropout在RNN中的应用综述

https://mp.weixin.qq.com/s/K6LK47_GCTeZJPAW0-Xp4Q

多伦多大学提出可逆RNN：内存大降，性能不减！

https://mp.weixin.qq.com/s/lvaWx7J4HFTvYxy7-B9vYg

周志华等提出RNN可解释性方法，看看RNN内部都干了些什么

https://mp.weixin.qq.com/s/YbdiEHb8ld1pp1ehgBzTOQ

将未来信息作为正则项，Twin Networks加强RNN对长期依赖的建模能力

https://mp.weixin.qq.com/s/ty8RyPREo_EA7O8vA2pQuQ

AI编曲震撼人心，RNN生成流行音乐

https://mp.weixin.qq.com/s/vIL-bKHZK-6eXZYWxrc9vw

这种有序神经元，像你熟知的循环神经网络吗？

# CNN进阶

http://mp.weixin.qq.com/s/ddTNr-967IahTZ2X1LNSEQ

盘点影响计算机视觉Top100论文：从ResNet到AlexNet

https://mp.weixin.qq.com/s/gwH9s1ggMTj2dJkad9wUuw

从VGG到NASNet，一文概览图像分类网络

https://mp.weixin.qq.com/s/hIAIbpqItS09KDOSFxaeqg

从Inception v1到Inception-ResNet，一文概览Inception家族的“奋斗史”

https://mp.weixin.qq.com/s/r143qYj8bziu_N-27RWRRw

机器学习5年大跃进，可能是个错觉

https://mp.weixin.qq.com/s/pollD4LN_GHJckzJAjwPqg

侧抑制”卷积神经网络，了解一下？

https://mp.weixin.qq.com/s/gil2K-JKzfRqdzc-abnh6A

王井东：深度融合——一种神经网络结构设计模式

https://mp.weixin.qq.com/s/7l0J5LIAawlkCIrUEGG9aA

卷积神经网络“失陷”，CoordConv来填坑

https://mp.weixin.qq.com/s/4GPLeJiQoDfBZJzF4QmuAg

Excel再现人脸识别：CNN用于计算机视觉任务不再神秘

https://mp.weixin.qq.com/s/30_AwwHS0eqfmYwTvRQ85Q

pooling去哪儿了？

https://mp.weixin.qq.com/s/yTkAhje3A0dRSyTobjsm2A

Pervasive Attention：用于序列到序列预测的2D卷积神经网络

https://zhuanlan.zhihu.com/p/31595192

Deep Image Prior：深度卷积网络先天就理解自然图像

https://mp.weixin.qq.com/s/TelGG-uVQyxwQjiDGE1pqA

特征金字塔网络FPN

https://mp.weixin.qq.com/s/6KC1vhKcyqQ0eZEXJyPqVA

密集连接卷积网络

https://mp.weixin.qq.com/s/vUK2NneOs8OR0vcJEZNbrA

田渊栋等人论文：何时卷积滤波器容易学习？

https://mp.weixin.qq.com/s/PiQB2AvhtDceMJxYN8O8jA

通用卷积神经网络交错组卷积

https://mp.weixin.qq.com/s/2w_Bwqx9C9KwenK480sVMQ

如何可视化卷积网络分类图像时关注的焦点

https://mp.weixin.qq.com/s/yQG0983RjQHIkt9oBlkDqQ

瞎谈CNN：通过优化求解输入图像

http://mp.weixin.qq.com/s/5ozeLeF6IyKTOqAxYEdaog

山东大学提出PointCNN：让CNN更好地处理不规则和无序的点云数据

https://zhuanlan.zhihu.com/p/33445638

传统不死：在CNN中学习和构建空间传播模块

https://mp.weixin.qq.com/s/mtocyqpybSEpwekS20xgmQ

使用CNN生成图像先验，实现更广泛场景的盲图像去模糊

https://mp.weixin.qq.com/s/Uc0VFMmYoOFvH0c7IExKIg

何恺明团队计算机视觉最新进展：从特征金字塔网络、Mask R-CNN到学习分割一切

https://mp.weixin.qq.com/s/W2DqCjmF3oyPLKeETAO5XA

CNN实现“读脑术”，成功解码人脑视觉活动，准确率超50%

https://mp.weixin.qq.com/s/t9ZHW8fkWtnvpM7ynJOTDw

从语义上理解卷积核行为，UCLA朱松纯等人使用决策树量化解释CNN
