---
layout: post
title:  机器学习（十六）——Adaboost, EMD
category: ML 
---

* toc
{:toc}

# Adaboost

Adaboost是Yoav Freund和Robert Schapire于1997年提出的算法。两人后来因为该算法被授予Gödel Prize（2003）。

>Yoav Freund，UCSC博士，UCSD教授。

>Robert Elias Schapire，MIT博士。先后供职于Princeton University、AT&T Labs和Microsoft Research。

>Gödel Prize，由欧洲计算机学会（EATCS）与美国计算机学会基础理论专业组织（ACM SIGACT）于1993年共同设立，颁给理论计算机领域最杰出的学术论文。其名称取自Kurt Gödel。

>Kurt Friedrich Gödel，1906～1978，奥地利逻辑学家，数学家，哲学家，后加入美国藉。维也纳大学博士（1930）。在逻辑学方面，他是继Aristotle、Gottlob Frege之后最伟大的逻辑学家。在数学方面，他以哥德尔不完备定理著称，和Bertrand Russell、 David Hilbert、Georg Cantor齐名。

Adaboost既可用于分类问题，也可用于回归问题。这里仅针对二分类问题进行讨论。

假设我们有数据集$$\{(x_1, y_1), \ldots, (x_N, y_N)\}$$，其中$$y_i \in \{-1, 1\}$$，还有一系列弱分类器$$\{k_1, \ldots, k_L\}$$。

由于Boost算法是个串行算法，每次迭代就会加入一个弱分类器。这样m-1次迭代之后的分类器如下所示：

$$C_{(m-1)}(x_i) = \alpha_1k_1(x_i) + \cdots + \alpha_{m-1}k_{m-1}(x_i)$$

而m次迭代之后的分类器则为：

$$C_{m}(x_i) = C_{(m-1)}(x_i) + \alpha_m k_m(x_i)$$

如何选择新加入的弱分类器$$k_m$$和对应的权重$$\alpha_m$$呢？我们可以定义误差E如下所示：

$$E = \sum_{i=1}^N e^{-y_i C_m(x_i)}$$

令$$w_i^{(1)} = 1,w_i^{(m)} = e^{-y_i C_{m-1}(x_i)}$$，则：

$$E = \sum_{i=1}^N w_i^{(m)}e^{-y_i\alpha_m k_m(x_i)}$$

因为$$k_m$$分类正确时，$$y_i k_m(x_i) = 1$$，分类错误时，$$y_i k_m(x_i) = -1$$。所以：

$$E = \sum_{y_i = k_m(x_i)} w_i^{(m)}e^{-\alpha_m} + \sum_{y_i \neq k_m(x_i)} w_i^{(m)}e^{\alpha_m}\\= \sum_{i=1}^N w_i^{(m)}e^{-\alpha_m} + \sum_{y_i \neq k_m(x_i)} w_i^{(m)}(e^{\alpha_m}-e^{-\alpha_m})$$

可以看出和$$k_m$$相关的实际上只有上式的右半部分。显然，使得$$\sum_{y_i \neq k_m(x_i)} w_i^{(m)}$$最小的$$k_m$$，也会令E最小，这也就是我们选择加入的$$k_m$$。

对E求导，得：

$$\frac{d E}{d \alpha_m} = \frac{d (\sum_{y_i = k_m(x_i)} w_i^{(m)}e^{-\alpha_m} + \sum_{y_i \neq k_m(x_i)} w_i^{(m)}e^{\alpha_m}) }{d \alpha_m}$$

令导数为0，可得：

$$\alpha_m = \frac{1}{2}\ln\left(\frac{\sum_{y_i = k_m(x_i)} w_i^{(m)}}{\sum_{y_i \neq k_m(x_i)} w_i^{(m)}}\right)$$

令$$\epsilon_m = \sum_{y_i \neq k_m(x_i)} w_i^{(m)} / \sum_{i=1}^N w_i^{(m)}$$，则：

$$\alpha_m = \frac{1}{2}\ln\left( \frac{1 - \epsilon_m}{\epsilon_m}\right)$$

参考：

https://mp.weixin.qq.com/s/G06VDc6iTwmNGsH4IfSeJQ

Adaboost从原理到实现

https://mp.weixin.qq.com/s/PZ-1fkNvdJmv_8zLbvoW1g

Adaboost算法原理小结

https://mp.weixin.qq.com/s/KoOUgwXLOfJfOjWhbFX52Q

如果Boosting你懂，那Adaboost你懂么？

https://mp.weixin.qq.com/s/Joz2FpGgBY0tC8lpoFz8Mw

AdaBoost元算法如何提高分类性能——机器学习实战

https://mp.weixin.qq.com/s/MLEVUKse5usmKIWJF-yfOQ

通俗易懂讲解自适应提升算法AdaBoost

https://mp.weixin.qq.com/s/VuDAdeVsoZsTokh3n_wWFw

一文详解机器学习中最好用的提升方法：Boosting与AdaBoost

https://mp.weixin.qq.com/s/Jnh7yIOmzbTvWk77zh2-lA

周志华：Boosting学习理论的探索——一个跨越30年的故事

# EMD

推土机距离（Earth mover's distance）是两个概率分布之间的距离度量的一种方式。如果将区间D的概率分布比作沙堆P，那么$$P_r$$和$$P_\theta$$之间的EMD距离，就是推土机将$$P_r$$改造为$$P_\theta$$所需要的工作量。

![](/images/article/earth_move.png)

EMD的计算公式为：

$$EMD(P_r,P_\theta) = \frac{\sum_{i=1}^m \sum_{j=1}^n f_{i,j}d_{i,j}}{\sum_{i=1}^m \sum_{j=1}^n f_{i,j}}$$

其中，f表示土方量，d表示运输距离。

然而，搬运土方的方案并不唯一，有的聪明，有的愚笨。因此，两个分布的EMD距离，通常指的是，所有方案的EMD距离的最小值。

![](/images/img3/EMD.png)

一般来说，可用上面这样的矩阵来可视化并计算EMD距离。

这个问题实际是线性规划中的运输问题，可以用匈牙利算法迭代求解。最终求得的最小值就是EMD。

最优方案也被称为“最优传输”，相关的研究被称作“最优传输理论”。法国数学家蒙日最早研究过该类问题。

>Gaspard Monge, Comte de Péluse，1746～1818，法国数学家。微分几何之父，巴黎综合理工大学（École Polytechnique）创始人、校长。海军部长。   
>革命形势在1794年已经开始恶化，蒙日的好友、化学家拉瓦锡就是在那时被声称“革命不需要科学”的群众，送上了断头台。
两年后的现在，50岁的蒙日又被革命群众认定为“不够激进”。他不得不从巴黎逃离，路途中还担心自己的安危——狂热的革命群众随时可能把他抓回去，并送上断头台。   
>一封意外的来信打消了蒙日的恐惧。写信人是法兰西共和国意大利方面军总司令拿破仑，27岁的总司令在信中表示，除了乐意向蒙日“伸出感激和友谊之手”，还想向他致谢。原来在4年前，他们见过面。当时蒙日担任法国海军部长，拿破仑尚是“不得宠的年轻炮兵军官”。在部长那里，拿破仑受到了“热诚的欢迎”。尽管蒙日根本记不起这件事，拿破仑则依旧“珍藏着这段记忆”。

EMD可以是多维分布之间的距离。一维的EMD也被称为Match distance。

EMD有时也称作Wasserstein距离。

>Leonid Vaseršteĭn，俄罗斯数学家，Moscow State University硕博，现居美国，Penn State University教授。Wasserstein是他名字的德文拼法，并为英文文献所沿用。他在去美国之前，曾在德国住过一段时间。

由于最优传输问题的计算比较复杂，因此在DL时代，我们通常使用神经网络来计算EMD距离，例如WGAN。

在文本处理中，有一个和EMD类似的编辑距离（Edit distance），也叫做Levenshtein distance。它是指两个字串之间，由一个转成另一个所需的最少编辑操作次数。许可的编辑操作包括将一个字符替换成另一个字符，插入一个字符，删除一个字符。一般来说，编辑距离越小，两个串的相似度越大。

>注：严格来说，Edit distance是一系列字符串相似距离的统称。除了Levenshtein distance之外，还包括Hamming distance等。

>Vladimir Levenshtein，1935年生，俄罗斯数学家，毕业于莫斯科州立大学。2006年获得IEEE Richard W. Hamming Medal。

参考：

https://vincentherrmann.github.io/blog/wasserstein/

Wasserstein GAN and the Kantorovich-Rubinstein Duality

http://chaofan.io/archives/earth-movers-distance-%e6%8e%a8%e5%9c%9f%e6%9c%ba%e8%b7%9d%e7%a6%bb

Earth Mover's Distance——推土机距离

https://mp.weixin.qq.com/s/rvPLYa1NFg_LRvb8Y8-aCQ

Wasserstein距离在生成模型中的应用

https://mp.weixin.qq.com/s/2xOrSyyWSbp8rBVbFoNrxQ

Wasserstein is all you need：构建无监督表示的统一框架

https://mp.weixin.qq.com/s/g4F50zLNs_aC1WUuCMNdXg

一文详解Wasserstein距离

https://mp.weixin.qq.com/s/NXDJ4uCpdX-YcWiKAsjJLQ

传说中的推土机距离基础，最优传输理论了解一下

https://mp.weixin.qq.com/s/5sNXmQbINIWMGjX5TYAPYw

最优传输理论你理解了，传说中的推土机距离重新了解一下

https://mp.weixin.qq.com/s/iwZrWYbppwStJlXESUufZQ

想要算一算Wasserstein距离？这里有一份PyTorch实战

https://mp.weixin.qq.com/s/itQNrNsdjAgPl5R48V-HtQ

计算最优传输（Computational Optimal Transport）

https://zhuanlan.zhihu.com/p/72803739

Word Mover's Distance-文档距离优化方案

https://mp.weixin.qq.com/s/Zi9v_sbxPkHWUWwNhMMMAg

编辑距离

https://zhuanlan.zhihu.com/p/270675634

点云距离度量：完全解析EMD距离(Earth Mover's Distance)

https://zhuanlan.zhihu.com/p/358895758

统计距离（STATISTICAL DISTANCE）

# TensorFlow参考+

https://mp.weixin.qq.com/s/eX3LWYiSH-KObH_7F_3QCA

TensorFlow 1.11.0发布，一键多GPU

https://mp.weixin.qq.com/s/316VVXLQfeIsKNk4ld-VRw

TensorFlow语义分割套件开源了ECCV18旷视科技BiSeNet实时分割算法

https://mp.weixin.qq.com/s/XI1J4ardEWKP4UQ4IXZGTQ

TensorFlow Hub,给您带来全新的Web体验

http://www.jianshu.com/p/1da012a83b74

利用TensorFlow实现排序和搜索算法

https://mp.weixin.qq.com/s/oEqMjOTj8xpd3sg60ZUhqA

TensorFlow的c++实践及各种坑

https://mp.weixin.qq.com/s/-5RCRl9ztQ2dQmX00QvfvQ

在Python和TensorFlow上构建Word2Vec词嵌入模型

https://mp.weixin.qq.com/s/Nyjp0mZxcn04vLKjJXLSaw

如何用TensorFlow在安卓设备上实现深度学习推断

https://mp.weixin.qq.com/s/kEowgNPVS1nAGBPbzkatlQ

如何构建高可读性和高可重用的TensorFlow模型

https://mp.weixin.qq.com/s/O_IN39FBVPeD5fRYBsPuZQ

用TensorFlow开发问答系统

https://mp.weixin.qq.com/s/8Hrq_z8s_5ms6Q_6OOaU-g

如何使用TensorFlow和自编码器模型生成手写数字

https://mp.weixin.qq.com/s/nnjyR4XGVZQ1zXCIPzTNlg

基于TensorFlow的变分自编码器实现

https://mp.weixin.qq.com/s/iMgesGmdb7Jq4muCxb-nFA

Tensorflow实战：Discuz验证码识别

https://mp.weixin.qq.com/s/4aJUGBpPG_6Oc5EqOmM0Iw

作为TensorFlow的底层语言，你会用C++构建深度神经网络吗？

https://github.com/yahoo/TensorFlowOnSpark

TensorFlow On Spark

https://mp.weixin.qq.com/s/7er3wNV_IhxhFDOIwNMpww

深度强化学习入门：用TensorFlow构建你的第一个游戏AI

https://mp.weixin.qq.com/s/UbBJYOmWtUXPFliRMyzDrg

最新TensorFlow专业深度学习实战书籍和代码《Pro Deep Learning with TensorFlow》

https://mp.weixin.qq.com/s/zeZs48XbYJGhvOoIysZ8QA

Docker Compose+GPU+TensorFlow所产生的奇妙火花

https://mp.weixin.qq.com/s/sOggiB57D-ekWOsbL6TY_A

TensorFlow中那些鲜为人知却又极其实用的知识

https://mp.weixin.qq.com/s/gW_KX6eF9XEsSUO1UzJ3WQ

基于LSTM的情感分析

https://mp.weixin.qq.com/s/KZhL477ApHgQfmM2xFrYJw

Tensorlang：基于TensorFlow的可微编程语言

https://mp.weixin.qq.com/s/_9NJ6QLQArUAD1DKb0KRfA

如何使用TensorFlow mobile部署模型到移动设备

https://mp.weixin.qq.com/s/e_TzQxFLAonLMyYAhte6Cg

face-api.js：在浏览器中进行人脸识别的JavaScript接口

https://zhuanlan.zhihu.com/p/347599203

TFRT的开源代码分析
