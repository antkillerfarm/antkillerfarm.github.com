---
layout: post
title:  深度学习（八）——fine-tuning, 李飞飞
category: theory 
---

# GAN（续）

## 如何对抗

因为$$D(Y,\Theta)$$的均值，也就是L，是度量两个分布的差异程度，这就意味着，L要能够将两个分布区分开来，即L越大越好；但是我们最终的目的，是希望通过均匀分布而生成我们指定的分布，所以$$G(X,\theta)$$则希望两个分布越来越接近，即L越小越好。

形式化的描述就是：

$$\arg \min_G \max_D V(G,D)$$

具体的做法是：

### Step1

随机初始化$$G(X,\theta)$$，固定它，然后生成一批Y，这时候我们要训练$$D(Y,\Theta)$$，既然L代表的是“与指定样本Z的差异”，那么，如果将指定样本Z代入L，结果应该是越小越好，而将Y代入L，结果应该是越大越好，所以

$$\begin{aligned}\Theta =& \mathop{\arg\min}_{\Theta} L = \mathop{\arg\min}_{\Theta} \frac{1}{N}\sum_{i=1}^N D\Big(z_i,\Theta\Big)\\ 
\Theta =& \mathop{\arg\max}_{\Theta} L = \mathop{\arg\max}_{\Theta} \frac{1}{M}\sum_{i=1}^M D\Big(y_i,\Theta\Big)\end{aligned}$$

然而有两个目标并不容易平衡，所以干脆都取同样的样本数B（一个batch），然后一起训练就好：

$$\begin{aligned}\Theta =& \mathop{\arg\min}_{\Theta} L_1\\ 
=&\mathop{\arg\min}_{\Theta} \frac{1}{B}\sum_{i=1}^B\left[D\Big(z_i,\Theta\Big)-D\Big(y_i,\Theta\Big)\right]\end{aligned}$$

### Step2

$$G(X,\theta)$$希望它生成的样本越接近真实样本越好，因此这时候把$$\Theta$$固定，只训练$$\theta$$让L越来越小：

$$\begin{aligned}\theta =& \mathop{\arg\min}_{\theta} L_2\\ 
=&\mathop{\arg\min}_{\theta} \frac{1}{B}\sum_{i=1}^B\left[D\Big(G(x_i,\theta),\Theta\Big)\right]\end{aligned}$$

## Lipschitz约束

稍微思考一下，我们就发现，问题还没完。我们目前还没有对D做约束，不难发现，无约束的话Loss基本上会直接跑到负无穷去了～

最简单的方案就是采用Lipschitz约束：

$$\| D(y,\theta) - D(y' , \theta) \| \leq C \|y-y'\|$$

也可写作：

$$\left\| \frac{\partial D(y,\Theta)}{\partial y}\right\| \leq C$$

>注：Rudolf Otto Sigismund Lipschitz，1832～1903，德国数学家，先后就读于柯尼斯堡大学和柏林大学，导师Dirichlet。波恩大学教授。

## WGAN

KL散度和JS散度由于不是距离，数学特性并不够好。因此，Martín Arjovsky于2017年1月，提出了Wasserstein GAN。

其中的一项改进就是使用Wasserstein距离替代KL散度和JS散度。Wasserstein距离的定义参看《机器学习（二十）》。

WGAN极大程度的改善了GAN训练困难的问题，成为当前GAN研究的主流。

参考：

https://zhuanlan.zhihu.com/p/25071913

令人拍案叫绝的Wasserstein GAN

## GAN的发展

最早的GAN出现在2014年6月，但直到2015年底，也只有5个变种，发展并不迅速。

2016年，GAN开始发力，年底时已有52个变种。2017年6月底，更达到142个变种。

![](/images/article/GAN_structure.png)

上图的源地址：

https://github.com/hwalsuklee/tensorflow-generative-model-collections

参考：

https://github.com/hindupuravinash/the-gan-zoo

GAN的各种变种。

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

https://mp.weixin.qq.com/s/oCDlhzjOYTIhsr5JuoRCJQ

IRGAN：大一统信息检索模型的博弈竞争

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

# fine-tuning

fine-tuning和迁移学习虽然是两个不同的概念。但局限到CNN的训练领域，基本可以将fine-tuning看作是一种迁移学习的方法。

举个例子，假设今天老板给你一个新的数据集，让你做一下图片分类，这个数据集是关于Flowers的。问题是，数据集中flower的类别很少，数据集中的数据也不多，你发现从零训练开始训练CNN的效果很差，很容易过拟合。怎么办呢，于是你想到了使用Transfer Learning，用别人已经训练好的Imagenet的模型来做。

由于ImageNet数以百万计带标签的训练集数据，使得如CaffeNet之类的预训练的模型具有非常强大的泛化能力，这些预训练的模型的中间层包含非常多一般性的视觉元素，我们只需要对他的后几层进行微调，再应用到我们的数据上，通常就可以得到非常好的结果。最重要的是，**在目标任务上达到很高performance所需要的数据的量相对很少**。

虽然从理论角度尚无法完全解释fine-tuning的原理，但是还是可以给出一些直观的解释。我们知道，CNN越靠近输入端，其抽取的图像特征越原始。比如最初的一层通常只能抽取一些线条之类的元素。越上层，其特征越抽象。

而现实的图像无论多么复杂，总是由简单特征拼凑而成的。因此，无论最终的分类结果差异如何巨大，其底层的图像特征却几乎一致。

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

# 李飞飞

## AI大佬

李飞飞是吴恩达之后的华裔AI新大佬。巧合的是，他们都是斯坦福AP+AI lab的主任，只不过吴是李的前任而已。

**李飞飞（Fei-Fei Li）**，1976年生，成都人，16岁移民美国。普林斯顿大学本科（1995～1999）+加州理工学院博士（2001～2005）。先后执教于UIUC、普林斯顿、斯坦福等学校。

个人主页：

http://vision.stanford.edu/feifeili/

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


