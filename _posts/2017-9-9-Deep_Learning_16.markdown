---
layout: post
title:  深度学习（十六）——SENet, 李飞飞, 深度ISP
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
