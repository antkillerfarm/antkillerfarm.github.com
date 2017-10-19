---
layout: post
title:  机器学习（二十八）——机器学习语录, Keras, LightGBM
category: theory 
---

# 机器学习语录

这里收录一些网上的只言片语式的心得，以区别于一般的教程。

>首先要考虑你的数据维度是线性相关的还是非线性相关的，数据是稀疏的还是稠密的，正例反例比例是多少，数据量是否充足。数据是否具有可分类性。是否需要降维。是否有噪音，是否有异常点等，然后去选择分类策略。通常包括数据采集，预处理，分类训练，预测，后处理等过程。

# 角度值的特征化

角度值是数据分析中常见的值，然而它不是线性的，比如0度和359度之间只相差1度，然而数值上却差了359度，因此无法将角度值直接代入线性回归等模型。因为后者的loss函数是用线性的欧氏距离定义的，角度显然不满足要求。

既然角度在一维上不是线性的，那么二维呢？没错，可以采用复数坐标(x,y)来表示角度，这样角度就是线性的了。

# CRF

条件随机场(Conditional Random Field)由Lafferty等人于2001年提出，结合了最大熵模型和隐马尔可夫模型的特点，是一种无向图模型，近年来在分词、词性标注和命名实体识别等序列标注任务中取得了很好的效果。

https://mp.weixin.qq.com/s/heYpeVLrZBRjXtMQou2BAA

如何轻松愉快的理解条件随机场（CRF）？

# 异常点检测

http://chuansong.me/n/377440751130

异常点检测算法（一）

http://jiangshuxia.9.blog.163.com/blog/static/3487586020083662621887/

异常(Outlier)检测算法综述

http://www.cnblogs.com/fengfenggirl/p/iForest.html

异常检测算法--Isolation Forest

https://mp.weixin.qq.com/s/xsuLIMPJVCThBGMRlz09Hg

Isolation Forest算法原理详解

# 自适应滤波器

《自适应滤波器原理》，Simon Haykin著。

## 基本估计

三种基本的信息处理运算：

**滤波（Filter）**：利用$$[0,t]$$的数据，来估计t时刻信息的运算过程。

**平滑（Smoothing）**：利用$$[0,t]$$的数据，来估计$$t'(t'<t)$$时刻信息的运算过程。

**预测（Prediction）**：利用$$[0,t]$$的数据，来估计$$t+\tau(\tau>0)$$时刻信息的运算过程。

可见，滤波和预测是实时运算，而平滑是非实时运算。

# Keras

Keras是深度学习的前端框架的集大成者，其后端可支持tensorflow、cntk、theano等。

所谓DL前端框架一般只提供对于DL的高层抽象和封装，至于具体的运算则由具体的后端来实现。

官网：

https://keras.io/

代码：

https://github.com/fchollet/keras/

安装：

`sudo pip install keras`

参考：

https://mp.weixin.qq.com/s/tyWUxJndljOSZT1-aYPgeg

Keras入门必看教程

https://mp.weixin.qq.com/s/57j-YxA4ODMy0RybI8n7uQ

教你在R中使用Keras和TensorFlow构建深度学习模型

https://mp.weixin.qq.com/s/H-oAETObn0SLUxK8--xudg

Keras+OpenAI强化学习实践：行为-评判模型

https://mp.weixin.qq.com/s/2lcOfU3X27YYqbWS341jcg

Keras+OpenAI强化学习实践：深度Q网络

http://mp.weixin.qq.com/s/kvQlzafLAn_8YgBmib3P0g

手把手教你在Amazon EC2上安装Keras

http://www.jianshu.com/p/20585e3b6d02

Keras TensorFlow教程：如何从零开发一个复杂深度学习模型

http://mp.weixin.qq.com/s/sQKhopzS4EOcYwjrM8E7xQ

十分钟搞定Keras序列到序列学习

http://mp.weixin.qq.com/s/F2gBP23LCEF72QDlugbBZQ

详解如何使用Keras实现Wassertein GAN

https://mp.weixin.qq.com/s/PR5gQYEse9KxhSkEVglRWg

用Keras实现seq2seq学习

https://mp.weixin.qq.com/s/0Rdet35LHAXQJuo-r_THXg

如何为LSTM重新构建输入数据（Keras）

# LightGBM

LightGBM是微软推出的boosting框架。

代码：

https://github.com/Microsoft/LightGBM

文档：

https://lightgbm.readthedocs.io/en/latest/

参考：

http://www.msra.cn/zh-cn/news/features/lightgbm-20170105

微软亚洲研究院：LightGBM介绍

https://zhuanlan.zhihu.com/p/25308051

比XGBOOST更快--LightGBM介绍

https://www.zhihu.com/question/51644470

如何看待微软新开源的LightGBM?

http://www.cnblogs.com/rocketfan/p/6005353.html

LightGBM中GBDT的实现

https://zhuanlan.zhihu.com/p/28768447

一个例子读懂LightGBM的模型文件

https://zhuanlan.zhihu.com/p/27916208

LightGBM调参指南(带贝叶斯优化代码)

