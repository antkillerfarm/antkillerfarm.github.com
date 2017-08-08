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

https://mp.weixin.qq.com/s/heYpeVLrZBRjXtMQou2BAA

如何轻松愉快的理解条件随机场（CRF）？

# 异常点检测

http://chuansong.me/n/377440751130

异常点检测算法（一）

http://jiangshuxia.9.blog.163.com/blog/static/3487586020083662621887/

异常(Outlier)检测算法综述

http://www.cnblogs.com/fengfenggirl/p/iForest.html

异常检测算法--Isolation Forest

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

https://mp.weixin.qq.com/s/-G94Mj-8972i2HtEcIZDpA

人脸识别世界杯榜单出炉，微软百万名人识别竞赛冠军分享

https://mp.weixin.qq.com/s/6_cW22DCzSw3DpUDrLXLcA

OpenAI提出强化学习近端策略优化，可替代策略梯度法

https://mp.weixin.qq.com/s/hkcVhyqiDW--F3QxucgEcA

亚马逊开源神经机器翻译框架Sockeye：基于Apache MXNet的NMT平台

https://mp.weixin.qq.com/s/TWdeHVCgEf54STvdA1QUPg

DeepMind哈萨比斯长文：伟大的AI离不开神经科学

https://mp.weixin.qq.com/s/Jdd7S6JFKRdsVe2w-W01IA

Facebook田渊栋开源游戏平台ELF，简化版《星际争霸》完美测试人工智能

https://mp.weixin.qq.com/s/GyE9qdXPGvrq12dMAf4nrQ

论文推荐：QA，增强学习，知识图谱，机器阅读理解

https://mp.weixin.qq.com/s/DQcRqWALLuPpNabCv83Vcg

自动机器学习，神经网络自主编程

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

https://mp.weixin.qq.com/s/1E9gunSpgYt5xl9Q_uQNJg

使用深度学习进行医疗影像分析：文件格式篇

https://mp.weixin.qq.com/s/PiQB2AvhtDceMJxYN8O8jA

通用卷积神经网络交错组卷积

https://mp.weixin.qq.com/s/r01vfSKb4VpFXVyokb54Bg

MIT开放图像标注工具LabelMe源代码：助力机器视觉的发展

https://mp.weixin.qq.com/s/tLqsWWhzUU6TkDbhnxxZow

Momenta详解ImageNet 2017夺冠架构SENet

https://mp.weixin.qq.com/s/30dK7nGJOWIaa1j9QElrYA

详解帝国理工集成工具TensorLayer：控制深度学习开发复杂度

http://stanford.edu/~rezab/nips2014workshop/slides/jeff.pdf

Techniques and Systems for Training Large Neural Networks Quickly

https://mp.weixin.qq.com/s/m_2LE2QN_8_a1hti6G0Cow

Netflix开源稀疏数据专用神经网络库：Vectorflow！

https://mp.weixin.qq.com/s/rlkZ82FcYMOmYoLjiCTN7g

一张图看懂人工智能知识体系

https://mp.weixin.qq.com/s/qReN6z8s45870HSMCMNatw

微软亚洲研究院CVPR 2017 Oral论文：逐层集中Attention的卷积模型

https://mp.weixin.qq.com/s/oNPrmIm1SOlA9tuyQZFr9w

让机器人学会理解语义概念：谷歌提出深度视觉新技术

https://mp.weixin.qq.com/s/HD370E4cCYvy_pdEKAvLIA

教机器学习编程

https://mp.weixin.qq.com/s/AS1VFjBFnSk19QJ28tBVWA

NIPS 2017斯坦福赛题大公开：强化学习模拟人类肌肉骨骼模型

https://mp.weixin.qq.com/s/EA5FgDeFI_w-kSfMpf5Yxw

神经网络视觉分类算法的意外弱点

https://mp.weixin.qq.com/s/CXKuSMi0Vd43BGDf5BgoqA

弱监督视频物体识别新方法：香港科技大学联合CMU提出TD-Graph LSTM

https://mp.weixin.qq.com/s/uh-Gh8BSxBi-jjG6-d7-UQ

TACOTRON一种端到端的Text-to-Speech合成模型

https://mp.weixin.qq.com/s/7NpJJn4k5oGPOB04S9Jyqw

DeepMind新论文，关联推理为什么是智能最重要的特征

https://mp.weixin.qq.com/s/V6JQuACFbDWBl-iYkDnRig

自动修复Bug正确率达78.3%，北大、微软等提出ACS技术

https://mp.weixin.qq.com/s/0Q-Kg6pNVRl3tqv8-wH-bg

Facebook开源史上最大星际争霸AI研究数据集



