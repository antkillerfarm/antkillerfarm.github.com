---
layout: post
title:  深度学习（十九）——SegNet, DeconvNet, DeepLab, ENet, GCN, Mask R-CNN
category: DL 
---

# SegNet

SegNet是Vijay Badrinarayanan于2015年提出的。

论文：

《SegNet: A Deep Convolutional Encoder-Decoder Architecture for Robust Semantic Pixel-Wise Labelling》

代码：

https://github.com/alexgkendall/caffe-segnet

除此之外，还有一个demo网站：

http://mi.eng.cam.ac.uk/projects/segnet/

>Vijay Badrinarayanan，印度人，班加罗尔大学本科（2001年）+Georgia理工硕士（2005年）+法国INRIA博士（2009年）。剑桥大学讲师。

>Alex Kendall，新西兰奥克兰大学本科（2014年）+剑桥大学博士在读。本文二作，但是代码和demo都是他写的。

>Roberto Cipolla，剑桥大学本科（1984年）+宾夕法尼亚大学硕士（1985年）+牛津大学博士（1991年）。剑桥大学教授。

![](/images/article/SegNet.png)

相比于CNN下采样阶段的结构规整，FCN上采样时的结构就显得凌乱了。因此，SegNet采用了几乎和下采样对称的上采样结构。

参考：

http://blog.csdn.net/fate_fjh/article/details/53467948

SegNet

https://mp.weixin.qq.com/s/YwmHiQ0vyFAx_dhjsmOlAQ

编解码结构SegNet

# DeconvNet

DeconvNet是韩国的Hyeonwoo Noh于2015年提出的。

论文：

《Learning Deconvolution Network for Semantic Segmentation》

代码：

https://github.com/HyeonwooNoh/DeconvNet

![](/images/article/DeconvNet.jpg)

从上图可见，DeconvNet和SegNet的结构非常类似，只不过DeconvNet在encoder和decoder之间使用了FC层作为中继。

类似这样的encoder-decoder对称结构的还有U-Net（因为它们的形状像U字形）：

![](/images/article/U_Net.jpg)

论文：

《U-Net: Convolutional Networks for Biomedical Image Segmentation》

官网：

https://lmb.informatik.uni-freiburg.de/people/ronneber/u-net/

>Olaf Ronneberger，弗莱堡大学教授，DeepMind研究员。

U-Net使用center crop和concat操作实现了不同层次特征的upsample，这和后面介绍的DenseNet十分类似。

参考：

https://mp.weixin.qq.com/s/ZNNwK1pkL4e0KeYw-UycgA

Kaggle车辆边界识别第一名解决方案：使用预训练权重轻松改进U-Net

# DeepLab

DeepLab共有4个版本（v1, v2, v3, v3+），分别对应4篇论文：

《Semantic Image Segmentation with Deep Convolutional Nets and Fully Connected CRFs》

《DeepLab: Semantic Image Segmentation with Deep Convolutional Nets, Atrous Convolution, and Fully Connected CRFs》

《Rethinking Atrous Convolution for Semantic Image Segmentation》

《Encoder-Decoder with Atrous Separable Convolution for Semantic Image Segmentation》

>Liang-Chieh(Jay) Chen，台湾国立交通大学本科（2004年）+密歇根大学硕士（2010年）+UCLA博士（2015年）。现为Google研究员。   
>个人主页：   
>http://liangchiehchen.com/

DeepLab针对FCN主要做了如下改进：

1.用Dilated convolution取代Pooling操作。因为前者能够更好的保持空间结构信息。

2.使用全连接条件随机场（Dense Conditional Random Field）替换最后的Softmax层。这里的CRF或者Softmax，也被称为语义分割网络的后端。

常见的后端还有Markov Random Field、Gaussian CRF等。这些都与概率图模型（Probabilistic Graphical Models）有关。

总之，目前的主流一般是**FCN+PGM**的模式。然而后端的计算模式和普通的NN有所差异，因此如何将后端NN化，也是当前研究的关键点。

参考：

https://mp.weixin.qq.com/s/ald9Dq_VV3PYuN6JoY3E5Q

DeepLabv3+：语义分割领域的新高峰

https://mp.weixin.qq.com/s/zLyAwtG49A-vwWa1iPziew

谷歌最新语义图像分割模型DeepLab-v3+今日开源

https://mp.weixin.qq.com/s/L1JtK9eUhVJMGWm-SjWW5w

DeepLab v1

https://zhuanlan.zhihu.com/p/34783156

读Xception和DeepLab V3+

https://mp.weixin.qq.com/s/_D9jFc1suFhmkm9v2-_PTQ

DeepLab v2及调试过程

https://mp.weixin.qq.com/s/Za_h1BWR-mQKtgArxwsjRw

语义分割网络DeepLab-v3的架构设计思想和TensorFlow实现

https://mp.weixin.qq.com/s/JidoZ9-V8JGWfB2gyO2AlA

Deeplab v2安装及调试全过程

https://mp.weixin.qq.com/s/pL1Wvd3oBRVtqG9I4O4oDg

DeepLab V3

https://zhuanlan.zhihu.com/p/54911894

语义分割之DeepLabv2

https://mp.weixin.qq.com/s/bFe4F1QGIWm-yCAx9YvWDQ

人人必须要知道的语义分割模型：DeepLabv3+

# ENet

ENet是波兰的Adam Paszke于2016年提出的。

论文：

《ENet: A Deep Neural Network Architecture for Real-Time Semantic Segmentation》

代码：

https://github.com/TimoSaemann/ENet

![](/images/article/ENet.png)

ENet的网络结构如上图所示。其中的initial和bottleneck结构分别见下图的(a)和(b)：

![](/images/article/ENet_2.png)

从大的结构来看，ENet的设计主要参考了Resnet和SqueezeNet。

ENet对Pooling操作进行了一定的修改：

1.下采样时，除了输出Pooling值之外，还输出Pooling值的位置，即所谓的Pooling Mask。

2.上采样时，利用第1步的Pooling Mask信息，获得更好的精确度。

显然这个修改在思路上和Dilated convolution是非常类似的。

参考：

http://blog.csdn.net/zijinxuxu/article/details/67638290

论文中文版blog

https://mp.weixin.qq.com/s/_Sz8u7A6_01e_Xbkvuoscg

快速道路场景分割—ENet

# Global Convolutional Network

Global Convolutional Network是孙剑团队的Chao Peng于2017年提出的。

论文：

《Large Kernel Matters -- Improve Semantic Segmentation by Global Convolutional Network》

>孙剑，西安交通大学博士（2003年）。后一直在微软亚洲研究院工作，担任首席研究员。2016年7月正式加入旷视科技担任首席科学家。

![](/images/article/GCN.png)

上图是论文的关键结构GCN，它主要用于计算超大卷积核。这里借鉴了Separable convolution的思想（将一个k x k的卷积运算，转换成1 x k + k x 1的卷积运算）。

然而正如我们在《深度学习（九）》中指出的，不是所有的卷积核都满足可分离条件。单纯采用先1 x k后k x 1，或者先k x 1后1 x k，效果都是不好的。而将两者结合起来，可以有效提高计算的精度。

![](/images/article/GCN_2.png)

这是GCN提出的另一个新结构。

![](/images/article/GCN_3.png)

上图是GCN的整体结构图。

参考：

http://blog.csdn.net/bea_tree/article/details/60977512

旷视最新：Global Convolutional Network

# 语义分割的展望

俗话说，“没有免费的午餐”（“No free lunch”）。基于深度学习的图像语义分割技术虽然可以取得相比传统方法突飞猛进的分割效果，但是其对数据标注的要求过高：不仅需要海量图像数据，同时这些图像还需提供精确到像素级别的标记信息（Semantic labels）。因此，越来越多的研究者开始将注意力转移到弱监督（Weakly-supervised）条件下的图像语义分割问题上。在这类问题中，图像仅需提供图像级别标注（如，有“人”，有“车”，无“电视”）而不需要昂贵的像素级别信息即可取得与现有方法可比的语义分割精度。

另外，示例级别（Instance level）的图像语义分割问题也同样热门。该类问题不仅需要对不同语义物体进行图像分割，同时还要求对同一语义的不同个体进行分割（例如需要对图中出现的九把椅子的像素用不同颜色分别标示出来）。

![](/images/article/Instance_level.jpg)

此外，基于视频的前景／物体分割（Video segmentation）也是今后计算机视觉语义分割领域的新热点之一，这一设定其实更加贴合自动驾驶系统的真实应用环境。

最后，目前使用的分割模型在对分割注释有限的大型概念词汇的识别方面表现欠佳。原因在于它们忽略了所有概念的固有分类和语义层次。例如，长颈鹿、斑马和马同属于有蹄类动物，这个大类描绘了它们的共同视觉特征，使得它们很容易与猫/狗区分开来。对人来说，即使你没见过斑马，但也不会把它错误的认成猫/狗。但目前的DL模型在这方面的能力还很薄弱。

参考：

https://mp.weixin.qq.com/s/palhFeMnWOZj-T2cqQN7tw

新型语义分割模型：动态结构化语义传播网络DSSPN

# Mask R-CNN

Mask R-CNN虽然挂着R-CNN的名头，但却是一个对象实例分割（不仅要分出对象的类别，连同一类对象的不同实例也要分出来）的网络。它是何恺明2017年的新作。

论文：

《Mask R-CNN》

代码：

官方Pytorch版本：

https://github.com/facebookresearch/maskrcnn-benchmark

TensorFlow版本：

https://github.com/hillox/TFMaskRCNN

MXNet版本：

https://github.com/TuSimple/mx-maskrcnn

![](/images/img2/mask_rcnn.png)

上图是Mask R-CNN的网络结构图。它实际上就是在Faster R-CNN的基础上添加了一个FCN。

![](/images/img3/Mask_R-CNN.png)

上图也是Mask R-CNN的网络结构图，但它对Faster R-CNN中与本主题无关的部分做了省略。

Mask R-CNN的要点主要有：

- **RoI Align**

RoI Align避免对RoI的边界或者块（bins）做任何量化，例如直接使用x/16代替[x/16]。

然而这就引来一个问题：如果x/16不是整数该怎么采样呢？

答案：对临近的整数采样点，使用双线性插值（bilinear interpolation）拟合，得到非整数采样点的值。

![](/images/img3/ROI_Align.png)

- **独立的类别预测**

把loss由`tf.nn.softmax_cross_entropy_with_logits`换成`tf.nn.sigmoid_cross_entropy_with_logits`。参见《深度目标检测（五）》的YOLOv3一节。没错，YOLOv3借鉴了Mask R-CNN的这一设计思路。

- **对象实例分割**

Mask R-CNN只对RoI Align后的区域进行分割，而不像U-NET等会对全景进行分割。因此，更适合抠图之类的应用。

Mask R-CNN除了可以用于实例分割以外，还可用于关键点检测。这点在原始论文和FB的代码中有体现，但是在通常的介绍中往往被忽略。

![](/images/img3/Mask_R-CNN_keypoint.png)

keypoint branch的输出结果是一个keypoint的heatmap（每个keypoint都有自己的heatmap），显然，heatmap中值最大的点就是keypoint的所在。

当然ROI区域和原图，无论是坐标，还是尺寸，都有差异，需要通过插值恢复回去。Mask R-CNN使用的是bicubic插值，该方法计算量较大，因此实际中，多采用下文所述的基于Taylor展开的方法进行插值。

《Invariant Features from Interest Point Groups》
