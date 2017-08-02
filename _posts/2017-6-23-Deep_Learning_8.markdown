---
layout: post
title:  深度学习（八）——模型压缩, 依存分析, CTC
category: theory 
---

# GAN

## GAN的发展

最早的GAN出现在2014年6月，但直到2015年底，也只有5个变种，发展并不迅速。

2016年，GAN开始发力，年底时已有52个变种。2017年6月底，更达到142个变种。

参考：

https://github.com/hindupuravinash/the-gan-zoo

GAN的各种变种。

## 参考

https://mp.weixin.qq.com/s/xa3F3kCprE6DEQclas4umg

GAN的数学原理

http://www.jianshu.com/p/e2d2d7cbbe49

50行代码实现GAN

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

# 模型压缩

对于AI应用端而言，由于设备普遍没有模型训练端的性能那么给力，因此如何压缩模型，节省计算的时间和空间就成为一个重要的课题。

此外，对于一些较大的模型（如VGG），即使机器再给力，单位时间内能处理的图像数量，往往也无法达到实际应用的要求。这点在自动驾驶和视频处理领域显得尤为突出。

这里首先提到的是韩松的两篇论文：

《Deep Compression: Compressing Deep Neural Networks with Pruning, Trained Quantization and Huffman Coding》

《Learning both Weights and Connections for Efficient Neural Networks》

>韩松，清华本科（2012）+Stanford博士（2017）。MIT AP（from 2018）。   
>个人主页：   
>https://stanford.edu/~songhan/

韩松也是SqueezeNet的二作。

![](/images/article/nn_compression.png)

https://www.zhihu.com/question/62068158

如何评价图森科技连发的三篇关于深度模型压缩的文章？

https://zhuanlan.zhihu.com/p/24337627

深度压缩之蒸馏模型

# 依存分析

## 概况

Dependency Parsing是NLP领域的一项重要工作。

![](/images/article/dependency_parsing.png)

依存分析的基本目标是**对一句话构建一个表达词与词之间依赖关系的语法树**，如上图所示。

## 传统方法

这里以2003年提出的Greedy transition-based parsing算法为例，介绍一下依存分析的传统做法。

![](/images/article/tbp.png)

![](/images/article/tbp_2.png)

上图演示了ROOT结点是如何一步步“吃”进词语（即Shift操作），并生成依存分析树的过程。

这里的每一步被称作**transition**。

transition中箭头左边的部分是以ROOT为栈底的**stack**，右边的部分是待处理文本的**buffer**，**A**表示依赖关系树。

stack+buffer+A构成了一个**configuration**。GTBP算法的难点，在于如何根据configuration，确定下一步的transition。这在传统做法中，通常是一堆文法规则，或者特征的分类。

## 依存分析的准确度指标

![](/images/article/dependency_accuracy.png)

依存分析的准确度指标主要有UAS和LAS两种。

上图是某句话的依存分析结果。其中Gold表示正确答案，而Parsed表示算法的计算结果。结果的第二列是依存结点，0表示ROOT；第4列是单词的词性。

Unlabeled attachment score是指依存结点是否正确。以上图中的例子为例，就是4/5=80%。

Labeled	attachment score不仅考虑依存结点是否正确，还考虑词性是否正确。用样以上图为例，则是2/5=40%。

## 深度方法

深度方法的开山之作是陈丹琦2014年的论文：

《A Fast and Accurate Dependency Parser using Neural Networks》

![](/images/article/dependency_parser_nn.png)

上图是该方案的结构图。

我们之前已经指出，在传统方法中，transition是由单词、词性和依赖关系树所确定的。只是这种确定的规则比较复杂，不易提炼出有效特征。

参照我们在CNN中的作为，特征提取这一步骤可以由神经网络来完成。因此，在这里我们将configuration的各个组成部分分别向量化，然后合成为一个长向量，作为Input layer。

这里采用以下的Cube函数作为激活函数，也是该文的一大创见：

$$h=(W_1^w x^w + W_1^t x^t + W_1^l x^l + b_1)^3$$

Output layer是一个softmax的多分类层，每个分类对应一个transition。

## SyntaxNet

2015年David Weiss在陈丹琦方案的基础上，做了一些改进。

论文：

《Structured Training for Neural Network Transition-Based Parsing》

![](/images/article/dependency_parser_nn_2.png)

## NLP的女学霸们

http://cs.stanford.edu/people/danqi/

陈丹琦，清华本科（姚班）（2012）+斯坦福博士生。

https://homes.cs.washington.edu/~luheng/

何律恒，上海交大本科（2010）+宾夕法尼亚大学硕士（2012）+华盛顿大学博士生。

# CTC

Connectionist Temporal Classification，是一种改进的RNN模型。它主要解决的是时序模型中，输入数大于输出数，输入输出如何对齐的问题。

参考：

https://www.zhihu.com/question/47642307

语音识别中的CTC方法的基本原理

https://www.zhihu.com/question/20398418

语音识别的技术原理是什么？

https://www.zhihu.com/question/55851184

基于CTC等端到端语音识别方法的出现是否标志着统治数年的HMM方法终结？

https://www.zhihu.com/question/46829056

语音识别领域的最新进展目前是什么样的水准？

https://zhuanlan.zhihu.com/p/27064536

用Wavenet做中文语音识别

https://www.zhihu.com/question/29168274

语音识别中，如何理解HMM是一个生成模型，而DNN是一个判别模型呢？

https://zhuanlan.zhihu.com/p/24979135

从声学模型算法总结2016年语音识别的重大进步

https://mp.weixin.qq.com/s/LsVhMaHrh8JgfpDra6KSPw

横向对比5大开源语音识别工具包

https://mp.weixin.qq.com/s/-NTQG7_-GqGQWrRhiGgAQQ

详述DeepMind wavenet原理及其TensorFlow实现

https://mp.weixin.qq.com/s/bFjXDQlxRbt1ia-DSfYazw

SampleRNN语音合成模型

https://zhuanlan.zhihu.com/p/21344595

端到端的OCR：验证码识别(LSTM+CTC)

https://mp.weixin.qq.com/s/zEqgDh6_fnDgXEI8MC9cmg

端对端的深度卷积神经网络在语音识别中的应用


