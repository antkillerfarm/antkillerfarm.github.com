---
layout: post
title:  深度学习（十六）——Style Transfer（2）, 人脸检测/识别（1）
category: DL 
---

* toc
{:toc}

# Style Transfer

## Cost Function（续）

首先，需要选择合适的层数l来计算$$J_{content}(C,G)$$。

如前所述，CNN的每个隐藏层分别提取原始图片的不同深度特征，由简单到复杂。如果l太小，则G与C在像素上会非常接近，没有迁移效果；如果l太深，则G上某个区域将直接会出现C中的物体。因此，l既不能太浅也不能太深，一般选择网络中间层。

若C和G在l层的激活函数输出$$a^{[l](C)}$$与$$a^{[l](G)}$$，则相应的$$J_{content}(C,G)$$的表达式为：

$$J_{content}(C,G)=\frac12||a^{[l](C)}-a^{[l](G)}||^2$$

接下来，我们定义图片的风格矩阵（style matrix）为：

$$G_{kk'}^{[l]}=\sum_{i=1}^{n_H^{[l]}}\sum_{j=1}^{n_W^{[l]}}a_{ijk}^{[l]}a_{ijk'}^{[l]}$$

风格矩阵$$G_{kk'}^{[l]}$$计算第l层隐藏层不同通道对应的所有激活函数输出和。若两个通道之间相似性高，则对应的$$G_{kk'}^{[l]}$$较大。从数学的角度来说，这里的风格矩阵实际上就是两个tensor的**互相关矩阵**，也就是上面提到的Gram矩阵。

Gram矩阵描述的是全局特征的自相关，如果输出图与风格图的这种自相关相近，那么差不多是我们所理解的”风格”。当然，其实也可以用很多其他的统计信息进行描绘风格。比如有用直方图的, 甚至还可以直接简化成”均值+方差”的。

风格矩阵$$G_{kk'}^{[l][S]}$$表征了风格图片S第l层隐藏层的“风格”。相应地，生成图片G也有$$G_{kk'}^{[l][G]}$$。那么，$$G_{kk'}^{[l][S]}$$与$$G_{kk'}^{[l][G]}$$越相近，则表示G的风格越接近S。这样，我们就可以定义出$$J^{[l]}_{style}(S,G)$$的表达式：

$$J^{[l]}_{style}(S,G)=\frac{1}{(2n_H^{[l]}n_W^{[l]}n_C^{[l]})}\sum_{k=1}^{n_C^{[l]}}\sum_{k'=1}^{n_C^{[l]}}||G_{kk'}^{[l][S]}-G_{kk'}^{[l][G]}||^2$$

为了提取的“风格”更多，也可以使用多层隐藏层，然后相加，表达式为：

$$J_{style}(S,G)=\sum_l\lambda^{[l]}\cdot J^{[l]}_{style}(S,G)$$

## 实现细节

![](/images/img2/style_transfer.png)

这是原始论文的插图，其符号表示和本文有所差异。其中的A、F、P各层的output，都是使用预训练好的Alex-Net生成的。

可以看出A和P，在整个迭代过程中，只需要进行一次Alex-Net的前向计算，因此可以事先计算好。

为了在迭代过程中，不修改Alex-Net的权重，而只修改F，我们可以使用`tf.constant`来创建Alex-Net的各个参数，进而建立Alex-Net。这样在backward的时候，梯度就只会修正到`tf.Variable`，也就是F上。

## 缺点

Gatys的方法虽然是里程碑式的进步，但仍然有如下缺点：

1.渲染速度缓慢。渲染一张图片需要从随机噪声开始，反复迭代若干次，才能得到最终的结果。

2.需要根据风格的不同，调整不同的超参数。换句话说，就是一个Style Transfer的模型就只能用于转换一种Style，通用型不强。

因此，之后的研究主要集中在对这两方面的改进上。针对前者的改进可称作**fast style transfer**，而后者可称作**Universal Style Transfer**。

此外，**不是所有的style transfer都是DL方法，很多新特效往往还是用传统的滤镜实现的**。比如最近比较火的“新海诚风格”。

参考：

https://blog.csdn.net/Trent1985

一个滤镜/美颜方面的blog

https://www.zhihu.com/question/29594460

新海诚风格的画面是手绘的还是Photoshop就可以达到的？后期过程是怎样的？

## Texture Networks: Feed-forward Synthesis of Textures and Stylized Images

这篇论文属于fast style transfer类的改进。它是Skolkovo Institute of Science and Technology & Yandex的Dmitry Ulyanov的作品。

Dmitry Ulyanov的个人主页：

https://dmitryulyanov.github.io

>Skolkovo位于莫斯科郊外，相当于俄国的硅谷。

代码：

https://github.com/DmitryUlyanov/texture_nets

![](/images/img2/Texture_Networks.png)

上图是该论文提出的网络结构，其中包括了两个部分：

1.descriptor network（以下简称D网络）。这部分的使用方法和Gatys方法基本一致：风格图片过一下预训练好的Alex-Net来生成纹理特征P。

2.论文的主要创新点在于generator network（以下简称G网络）。

1）假设有一批图片A，通过G网络得到了风格图片G(A)。

2）G(a)和a做比较得到content loss，G(A)和P做比较得到style loss。

3）迭代优化上述两个loss，得到训练好的G网络。

4）Inference时，直接将图片a输入G网络，就得到了风格变换后的图片G(a)。

## Perceptual Losses for Real-Time Style Transfer and Super-Resolution

这篇论文是李飞飞组的Justin Johnson的作品。

Justin Johnson的个人主页：

https://cs.stanford.edu/people/jcjohns/

代码：

https://github.com/OlavHN/fast-neural-style

https://github.com/lengstrom/fast-style-transfer/

![](/images/img2/RTST.png)

可以看出该论文的方法和Texture Networks基本一致，差别仅在于generator network和descriptor network的结构，略有不同而已。这里不再赘述。

参考：

https://blog.csdn.net/Hungryof/article/details/61195783

超越fast style transfer---任意风格图和内容图0.1秒出结果

https://zhuanlan.zhihu.com/p/35798776

快速风格迁移（fast-style-transfer）

## 其他

原版的neural style是用Gram矩阵来进行匹配风格，但是也有用其他的。例如：

MRF loss（ombining markov random fields and convolutional neural networks for image synthesis.）

Adversarial loss(C. Li and M. Wand. Precomputed real-time texture synthesis with markovian generative adversarial networks. In ECCV,2016)

梯度直方图（P. Wilmot, E. Risser, and C. Barnes. Stable and controllable neural texture synthesis and style transfer using histogram losses. arXiv preprint arXiv:1701.08893 , 2017）

## ResNet + Style transfer

Style transfer的CNN bone也是有讲究的，除了AlexNet之外，还可以使用VGG，但是它在ResNet上效果不佳。直到最近的论文才对这一现象有所解释。

参考：

https://mp.weixin.qq.com/s/Pub83W6nMM02lEIrsBe49Q

谁说只有VGG才能做风格迁移，ResNet也可以！

## Style transfer的其他用法

使用VGG抽取的所谓纹理信息，也可以用于图像增强领域。具体的做法是将纹理信息作为Loss的一部分。（注意不是Loss的全部，否则就只是Style transfer了）。

## 文本风格迁移

不光图像、语音有风格迁移，文本也有。

下面是一个金庸风格的例子

```text
Input：谢谢
Output（金庸）：多谢之至

Input：再见
Output（金庸）：别过!

Input：请问您贵性？
Output（金庸）：请教阁下尊姓大名?
```

https://mp.weixin.qq.com/s/7nvqifY_J34mWEUM5IUsLQ

NLP文本风格迁移

## 参考

https://zhuanlan.zhihu.com/p/26746283

图像风格迁移(Neural Style)简史

https://mp.weixin.qq.com/s/64H2dDcaTcKdKaOnwdsoEg

基于深度学习的艺术风格化研究

https://blog.csdn.net/red_stone1/article/details/79055467

人脸识别与神经风格迁移

https://blog.csdn.net/cicibabe/article/details/70885715

卷积神经网络图像风格转移

https://blog.csdn.net/stdcoutzyx/article/details/53771471

图像风格转换(Image style transfer)

https://blog.csdn.net/u011534057/article/details/78935202

风格迁移学习笔记(1):Multimodal Transfer: A Hierarchical Deep Convolutional Neural Network for Fast

https://blog.csdn.net/u011534057/article/details/78935230

风格迁移学习笔记(2):Universal Style Transfer via Feature Transforms

https://mp.weixin.qq.com/s/l3hQCQWh5NgihzTs2A049w

风格迁移原理及tensorflow实现

https://mp.weixin.qq.com/s/5Omfj-fYRDt9j2VZH1XXkQ

如何用Keras打造出“风格迁移”的AI艺术作品

https://mp.weixin.qq.com/s/4q-9QsXD04mD-f2ke7ql8A

tensorflow风格迁移网络训练与使用

https://blog.csdn.net/hungryof/article/details/53981959

谈谈图像的Style Transfer（一）

https://blog.csdn.net/Hungryof/article/details/71512406

谈谈图像的style transfer（二）

https://mp.weixin.qq.com/s/8Fz6Q-6VgJsAko0K7HDsow

一个模型搞定所有风格转换，直接在浏览器实现（demo+代码）

https://github.com/cysmith/neural-style-tf

TensorFlow (Python API) implementation of Neural Style.这个项目实现了两张图片的画风融合，非常牛。

https://github.com/jinfagang/pytorch_style_transfer

这个和上面的一样，不过是用pytorch实现的。

https://mp.weixin.qq.com/s/g1hpuzH36j_rbYR23Mwx0w

开源图像风格迁移，快看看大画家的潜力股

https://mp.weixin.qq.com/s/OzancX-44Si13ZtZiONnpQ

基于感知损失的实时风格迁移与超分辨率重建

https://mp.weixin.qq.com/s/yr9fNyzpOBt7dLsnh2T2xg

使用TFLite在移动设备上优化与部署风格转化模型

https://mp.weixin.qq.com/s/9AEYcY04lAl3dCRK9LPBeQ

人脸风格化核心技术与数据集总结

https://mp.weixin.qq.com/s/ylfeWiOrftCB823_gjQNmA

图像风格迁移也有框架了：使用Python编写，与PyTorch完美兼容，外行也能用

# 人脸检测/识别

## Cascade CNN

论文：

《A Convolutional Neural Network Cascade for Face Detection》

![](/images/img2/CascadeCNN.jpg)

这篇可以说是对经典的Viola-Jones方法的深度卷积网络实现，可以明显看出是3阶级联（12-net、24-net、48-net）。

前2阶的网络都非常简单，只有第3阶才比较复杂。这不是重点，重点是我们要从上图中学习多尺度特征组合。

以第2阶段的24-net为例，首先把上一阶段剩下的窗口resize为24*24大小，然后送入网络，得到全连接层的特征。同时，将之前12-net的全连接层特征取出与之拼接在一起。最后对组合后的特征进行softmax分类。

除了分类网络之外，Cascade CNN还包含了3个修正bounding box的CNN网络，分别叫做12-calibration-net，24-calibration-net和48-calibration-net。他们的结构与12-net等类似。

网络结构方面也就这样了，该论文最牛之处在于给出了这类级联网络的训练方法。

![](/images/img2/CascadeCNN_2.jpg)

1.按照一般的方法组织正负样本训练第一阶段的12-net和12-calibration-net网络；

2.利用上述的1层网络在AFLW数据集上作人脸检测，在保证99%的召回率的基础上确定判别阈值T1。

3.将在AFLW上判为人脸的非人脸窗口作为负样本，将所有真实人脸作为正样本，训练第二阶段的24-net和24-calibration-net网络；

4.重复2和3，完成最后阶段的训练。

参考：

http://blog.csdn.net/shuzfan/article/details/50358809

人脸检测——CascadeCNN
