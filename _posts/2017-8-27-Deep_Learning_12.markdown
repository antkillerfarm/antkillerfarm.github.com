---
layout: post
title:  深度学习（十二）——Siamese network, SENet, 姿态/行为检测
category: DL 
---

* toc
{:toc}

# 花式卷积

## 3D卷积（续）

http://blog.csdn.net/zouxy09/article/details/9002508

基于3D卷积神经网络的人体行为理解

https://mp.weixin.qq.com/s/YdON6Yzddq2f_QGbQsOY8w

深度三维残差神经网络：视频理解新突破

https://mp.weixin.qq.com/s/MfDQXTSoe0GnaEFfyLJQ1w

点云处理不得劲？球卷积了解一下

https://paddlepedia.readthedocs.io/en/latest/tutorials/CNN/convolution_operator/3D_Convolution.html

3D卷积

## 参考

https://github.com/vdumoulin/conv_arithmetic

Convolution arithmetic

http://deeplearning.net/software/theano_versions/dev/tutorial/conv_arithmetic.html

Convolution arithmetic

https://www.jarvis73.com/2019/06/06/Convolution-Guide/

Convolution Arithmetic for Deep Learning

https://mp.weixin.qq.com/s/dR2nhGqpz7OdmxKPYSaaxw

如何理解空洞卷积（dilated convolution）？

https://mp.weixin.qq.com/s/CLFbhWMcat4rN8YS_7q25g

这12张图生动的告诉你，深度学习中的卷积网络是怎么一回事？

https://mp.weixin.qq.com/s/kJEeKzC9pC375EjIJpTuzg

一文全解深度学习中的卷积

http://cs.nyu.edu/~fergus/drafts/utexas2.pdf

Deconvolutional Networks

https://zhuanlan.zhihu.com/p/22245268

CNN-反卷积

http://buptldy.github.io/2016/10/29/2016-10-29-deconv/

Transposed Convolution, Fractionally Strided Convolution or Deconvolution（中文blog）

https://mp.weixin.qq.com/s/ybI8kJPRn7sH-hJbc5uqnw

CMU研究者探索新卷积方法：在实验中可媲美基准CNN

https://zhuanlan.zhihu.com/p/46633171

深度卷积神经网络中的降采样

https://mp.weixin.qq.com/s/1gBC-bp4Q4dPr0XMYPStXA

万字长文带你看尽深度学习中的各种卷积网络

https://mp.weixin.qq.com/s/qReN6z8s45870HSMCMNatw

微软亚洲研究院：逐层集中Attention的卷积模型

http://blog.csdn.net/shuzfan/article/details/77964370

不规则卷积神经网络

https://mp.weixin.qq.com/s/rXr_XBc2Psh3NSA0pj4ptQ

常建龙：深度卷积网络中的卷积算子研究进展

https://mp.weixin.qq.com/s/i8vOeAVEYX-hRAvPSe6DEA

一文看尽神经网络中不同种类的卷积层

https://mp.weixin.qq.com/s/hZc8MgHoE010hnzLU-trIA

高性能涨点的动态卷积DyNet与CondConv、DynamicConv有什么区别联系？

https://www.yuque.com/yahei/hey-yahei/condconv

CondConv：按需定制的卷积权重

https://mp.weixin.qq.com/s/eRZ3jNuceMYKE3lEj-g1aw

动态卷积：自适应调整卷积参数，显著提升模型表达能力

https://mp.weixin.qq.com/s/_GOXBYyyYnridILemNRDqA

ChannelNets: 省力又讨好的channel-wise卷积，在channel维度进行卷积滑动 

https://mp.weixin.qq.com/s/HMLKUL3_3MhWmJ8ub-Yfcg

一文速览Deep Learning中的11种卷积

# Siamese network

Siamese和Chinese有点像。Siam是古时候泰国的称呼，中文译作暹罗。Siamese也就是“暹罗”人或“泰国”人。Siamese在英语中是“孪生”、“连体”的意思，这是为什么呢？

>十九世纪泰国出生了一对连体婴儿，当时的医学技术无法使两人分离出来，于是两人顽强地生活了一生，1829年被英国商人发现，进入马戏团，在全世界各地表演，1839年他们访问美国北卡罗莱那州，后来成为“玲玲马戏团” 的台柱，最后成为美国公民。1843年4月13日跟英国一对姐妹结婚，恩生了10个小孩，昌生了12个，姐妹吵架时，兄弟就要轮流到每个老婆家住三天。1874年恩因肺病去世，另一位不久也去世，两人均于63岁离开人间。两人的肝至今仍保存在费城的马特博物馆内。从此之后“暹罗双胞胎”（Siamese twins）就成了连体人的代名词，也因为这对双胞胎让全世界都重视到这项特殊疾病。

![](/images/img3/Siamese_network.jpg)

上图摘自LeCun 1993年的论文：

《Signature Verification using a ‘Siamese’ Time Delay Neural Network》

Siamese network有两个输入（Input1 and Input2）,将两个输入feed进入两个神经网络（Network1 and Network2），这两个神经网络分别将输入映射到新的空间，形成输入在新的空间中的表示。通过Loss的计算，评价两个输入的相似度。

在上图中，两个输入分别是支票上的签名与银行预留签名。我们可以使用Siamese network来验证两者是否一致。

Siamese network也可进一步细分：

- 如果Network1和Network2的结构和参数都相同，则称为Siamese network。

- 如果两个网络不共享参数，则称为pseudo-siamese network。对于pseudo-siamese network，两边可以是不同的神经网络（如一个是lstm，一个是cnn），也可以是相同类型的神经网络。

除了Siamese network之外，类似的还有三胞胎连体——Triplet network。

![](/images/img3/Triplet_network.jpg)

输入是三个，一个正例+两个负例，或者一个负例+两个正例，训练的目标是让相同类别间的距离尽可能的小，让不同类别间的距离尽可能的大。

Siamese network由于输入是一对样本，因此它更能理解样本间的差异，使得数据量相对较小的数据集也能用深度网络训练出不错的效果。

对Siamese network的进一步发展，引出了2020年比较火的对比学习。

参考：

https://blog.csdn.net/shenziheng1/article/details/81213668

Siamese Network（原理篇）

https://www.jianshu.com/p/92d7f6eaacf5

Siamese network孪生神经网络--一个简单神奇的结构

https://blog.csdn.net/sxf1061926959/article/details/54836696

Siamese Network理解

https://vra.github.io/2016/12/13/siamese-caffe/

Caffe中的Siamese网络

https://mp.weixin.qq.com/s/rPC542OcO8B4bjxn7JRFrw

深度学习网络只能有一个输入吗

https://mp.weixin.qq.com/s/GlS2VJdX7Y_nfBOEnUt2NQ

使用Siamese神经网络进行人脸识别

https://mp.weixin.qq.com/s/lDlijjIUGmzNzcP89IzJnw

张志鹏:基于siamese网络的单目标跟踪

https://mp.weixin.qq.com/s/WYL43CEhVmsvjZDY7afMrA

孪生网络：使用双头神经网络进行元学习

https://mp.weixin.qq.com/s/bgZbIi4BvAFmVoAakciYGQ

如何训练孪生神经网络

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

https://mp.weixin.qq.com/s/ao7gOfMYDJDPsNMVV9-Dlg

后ResNet时代：SENet与SKNet

https://mp.weixin.qq.com/s/_7Iir2DZ_lROyR-BxScbnA

SANet：视觉注意力SE模块的改进，并用于语义分割

# Non-local

https://zhuanlan.zhihu.com/p/33345791

Non-local neural networks

https://zhuanlan.zhihu.com/p/109514384

医学图像分割的Non-local U-Nets

https://mp.weixin.qq.com/s/Tox7jEFNHFHZQ-KdojMIpA

GCNet：当Non-local遇见SENet

https://zhuanlan.zhihu.com/p/48198502

Non-local Neural Networks论文笔记

https://mp.weixin.qq.com/s/zHZO1pmY8PCoI9vkDOaUgw

CCNet--于"阡陌交通"处超越恺明的Non-local

https://mp.weixin.qq.com/s/6q2q9OVhOYjk4ZrhLvAdkA

Non-local Neural Networks及自注意力机制思考

https://mp.weixin.qq.com/s/v4IK4gJvZ3J03Ikrujiyhw

视觉注意力机制：Non-local模块与Self-attention的之间的关系与区别？

https://mp.weixin.qq.com/s/EElEYaDbfdxlGWL_jBEwzQ

Non-local与SENet、CBAM模块融合：GCNet、DANet

https://mp.weixin.qq.com/s/lZxamQryotfLTKpRJKaA5Q

Non-local模块如何改进？来看CCNet、ANN

https://mp.weixin.qq.com/s/2O-T6akdPjGe2rUZKoE4Kw

Self-attention机制及其应用：Non-local网络模块

https://zhuanlan.zhihu.com/p/138444916

写写non local network

# 姿态/行为检测

基于CNN的2D多人姿态估计方法，通常有2个思路（Bottom-Up Approaches和Top-Down Approaches）：

- Top-Down framework，就是先进行行人检测，得到边界框，然后在每一个边界框中检测人体关键点，连接成每个人的姿态，缺点是受人体检测框影响较大，代表算法有RMPE。

- Bottom-Up framework，就是先对整个图片进行每个人体关键点部件的检测，再将检测到的人体部位拼接成每个人的姿态，代表方法就是openpose。

## OpenPose

OpenPose是一个实时多人关键点检测的库，基于OpenCV和Caffe编写。它是CMU的Yaser Sheikh小组的作品。

>Yaser Ajmal Sheikh，巴基斯坦信德省易司哈克工程科学与技术学院本科（2001年）+中佛罗里达大学博士（2006年）。现为CMU副教授。

![](/images/article/openpose.png)

OpenPose的使用效果如上图所示。

论文：

《Realtime Multi-Person 2D Pose Estimation using Part Affinity Fields》

《Hand Keypoint Detection in Single Images using Multiview Bootstrapping》

《Convolutional pose machines》

官方代码（caffe）：

https://github.com/CMU-Perceptual-Computing-Lab/openpose

Tensorflow版本：

https://github.com/ildoonet/tf-pose-estimation

参考：

https://zhuanlan.zhihu.com/p/37526892

OpenPose：实时多人2D姿态估计

https://mp.weixin.qq.com/s?__biz=MzIwMTE1NjQxMQ==&mid=2247488741&idx=2&sn=93f05747f3a94a2cbfa2431901d2d97f

OpenPose升级，CMU提出首个单网络全人体姿态估计网络，速度大幅提高

https://mp.weixin.qq.com/s/jAmUscrMZ8EmG3th-3Yx3w

实战：基于OpenPose的卡通人物可视化
