---
layout: post
title:  机器学习（二十五）——机器学习语录
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

# 异常点检测

http://chuansong.me/n/377440751130

http://jiangshuxia.9.blog.163.com/blog/static/3487586020083662621887/

http://www.cnblogs.com/fengfenggirl/p/iForest.html

# 自适应滤波器

《自适应滤波器原理》，Simon Haykin著。

>注：Simon Haykin，英国伯明翰大学博士，加拿大麦克马斯特大学教授。加拿大皇家学会会员。自适应信号处理领域的权威。

## 基本估计

三种基本的信息处理运算：

**滤波（Filter）**：利用$$[0,t]$$的数据，来估计t时刻信息的运算过程。

**平滑（Smoothing）**：利用$$[0,t]$$的数据，来估计$$t'(t'<t)$$时刻信息的运算过程。

**预测（Prediction）**：利用$$[0,t]$$的数据，来估计$$t+\tau(\tau>0)$$时刻信息的运算过程。

可见，滤波和预测是实时运算，而平滑是非实时运算。

# DL参考资源

https://mp.weixin.qq.com/s/x0r-2J_YdYgIQlRDqvGofg

CVPR 2017论文解读：用于单目图像车辆3D检测的多任务网络

https://mp.weixin.qq.com/s/fY4vF0aPexwFi2Fu4grdzg

大规模视觉理解竞赛WebVision冠军分享

https://mp.weixin.qq.com/s/cO1VlYGwdRBAbPs7IgvcAA

超越传统强化学习的价值分布方法

https://mp.weixin.qq.com/s/HlqzSdhj3Q3nX4Zl63PM0A

商汤科技23篇论文横扫CVPR

https://mp.weixin.qq.com/s/TelGG-uVQyxwQjiDGE1pqA

特征金字塔网络FPN

https://mp.weixin.qq.com/s/RpaOrngeXTKycLb3iCygZw

利用贝叶斯神经网络进行随机动力系统中的学习与策略搜索

https://mp.weixin.qq.com/s/j5YPHYEPioLiEIDc6lK3kA

在线视频衣物精确检索技术，开启刷剧败明星同款时代

https://mp.weixin.qq.com/s/6KC1vhKcyqQ0eZEXJyPqVA

密集连接卷积网络

https://mp.weixin.qq.com/s/Jty0eHlQ7gmihKkdp76XyQ

北大：一种基于新闻特征抽取和循环神经网络的股票预测方法

https://mp.weixin.qq.com/s/5DtTgc9bIrdXQkmuqRm8CA

谷歌大脑迁移学习：减少调参，直接在数据集中学习最佳图像架构

https://mp.weixin.qq.com/s/bMW2go3QefbgYMqVSzyy_g

腾讯AI Lab深度解读CVPR五大前沿

https://mp.weixin.qq.com/s/DGPI-nASWnsO9t-_AnQnbw

OpenAI 新研究：通过自适应参数噪声提升强化学习性能

https://mp.weixin.qq.com/s/cANlqQAI-A2mC9vnd3imQA

Instance-Aware图像语义分割

https://mp.weixin.qq.com/s/_dHjZQ_7_7H34PHhV_lC3w

全新强化学习算法详解，看贝叶斯神经网络如何进行策略搜索

https://www.leiphone.com/news/201707/mFuwXGvZBhoVQD5S.html

一秒分辨出杨臣刚、王大治和孙楠，这个黑产居然用AI 来"打码"

http://mp.weixin.qq.com/s/Bt6EMD4opHCnRoHKYitsUA

结合人类视觉注意力进行图像分类

https://mp.weixin.qq.com/s/M4U0RKcFqZKZAiwK5IyOFA

GAN之父Ian Goodfellow在Quora：机器学习十问十答


