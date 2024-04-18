---
layout: post
title:  深度学习（三十八）——RNN进阶, 显著性检测
category: DL 
---

* toc
{:toc}

# RNN进阶

## IndRNN

https://mp.weixin.qq.com/s/cAqpclkkeVrTiifz07HC1g

新型循环神经网络IndRNN：可构建更长更深的RNN

https://mp.weixin.qq.com/s/7-K-nZTijoYCaprRNYXxFg

新型RNN：将层内神经元相互独立以提高长程记忆

## ODE

https://mp.weixin.qq.com/s/4CrPGKnR7RLN-2ROG5X4uw

ODE网络：一场颠覆RNN的革命即将到来

https://mp.weixin.qq.com/s/0vju0Q_DcIWwdEo9EEE3iQ

NeurIPS18最佳论文NeuralODE，现在有了TensorFlow实现

https://mp.weixin.qq.com/s/i6VEYjbac4QP3s51meN1VA

Hinton向量学院推出神经ODE：超越ResNet 4大性能优势

https://mp.weixin.qq.com/s/ZEIsyV-0aTvYn6K8GyANPA

硬核NeruIPS 2018最佳论文，一个神经了的常微分方程

https://mp.weixin.qq.com/s/uAiRTeYkZKy9q3d9v3dR5A

神经微分方程--钢琴和小提琴

## 参考

https://mp.weixin.qq.com/s/0TLaC8ACXAFEK5aMNK9O-Q

简单循环单元SRU：像CNN一样快速训练RNN

https://zhuanlan.zhihu.com/p/27104240

CW-RNN收益率时间序列回归

https://mp.weixin.qq.com/s/SeR_zNZTu4t7kqB6ltNrmQ

从循环到卷积，探索序列建模的奥秘

https://mp.weixin.qq.com/s/_q69BV1r46S9X5wnLuFPSw

关于序列建模，是时候抛弃RNN和LSTM了

https://mp.weixin.qq.com/s/m5GRNp6qDfVfC0mkQ4m4Yw

神经语言模型如何利用上下文信息：长距离上下文的词序并不重要

https://mp.weixin.qq.com/s/kuoUnt2Vhz9NhfnNqMFAhQ

DeepMind提出关系RNN：构建关系推理模块，强化学习利器

https://mp.weixin.qq.com/s/wfOzCxe3L2t11VguYLGC9Q

上海交大搞出SRNN，比普通RNN也就快135倍

https://mp.weixin.qq.com/s/f0sv7c-H5o5L_wy2sUonUQ

CNN取代RNN？当序列建模不再需要循环网络

https://mp.weixin.qq.com/s/h3fF6Zvr1rSzSMpqdu8B0A

电子科大提出BT-RNN：替代全连接操作而大幅度提升LSTM效率

https://mp.weixin.qq.com/s/OgN4rVDKH5WABIaRY7CHog

如何让RNN神经元拥有基础通用的注意力能力

https://mp.weixin.qq.com/s/KBLCrupGIuPa5nVrxcS5WQ

新研究将GRU简化成单门架构，或更适用于语音识别

https://mp.weixin.qq.com/s/kQozftKd_n_kYIF7KKCc8g

短视频那么多，快手如何利用GRU实现各种炫酷的语音应用

https://mp.weixin.qq.com/s/xwuM2Vj8G7UyuEyzTyO13A

将CNN与RNN组合使用，天才还是错乱？

https://mp.weixin.qq.com/s/c7XkzjLH1n5EtqdQik618g

Dropout在RNN中的应用综述

https://mp.weixin.qq.com/s/K6LK47_GCTeZJPAW0-Xp4Q

多伦多大学提出可逆RNN：内存大降，性能不减！

https://mp.weixin.qq.com/s/lvaWx7J4HFTvYxy7-B9vYg

周志华等提出RNN可解释性方法，看看RNN内部都干了些什么

https://mp.weixin.qq.com/s/YbdiEHb8ld1pp1ehgBzTOQ

将未来信息作为正则项，Twin Networks加强RNN对长期依赖的建模能力

https://mp.weixin.qq.com/s/ty8RyPREo_EA7O8vA2pQuQ

AI编曲震撼人心，RNN生成流行音乐

https://mp.weixin.qq.com/s/vIL-bKHZK-6eXZYWxrc9vw

这种有序神经元，像你熟知的循环神经网络吗？

https://mp.weixin.qq.com/s/GGK9T0DeyIdD5ahHy5uvfg

LightRNN：存储和计算高效的RNN

https://mp.weixin.qq.com/s/JGZpKSF5HPCMCD061jwq9A

Bengio等人提出新型循环架构，大幅提升模型泛化性能

https://mp.weixin.qq.com/s/GN0m5nWuV6VDYsTk0XLoDA

pytorch中如何处理RNN输入变长序列padding

https://mp.weixin.qq.com/s/bts9mdIrGIjO8UCUxSV-xg

Transformer的潜在竞争对手QRNN论文解读，训练更快的RNN

# 显著性检测

视觉显著性检测(Visual Saliency Detection)指通过智能算法模拟人的视觉特点，提取图像中的显著区域(即人类感兴趣的区域)。

https://blog.csdn.net/dawnlooo

一个显著性检测的专栏

https://mp.weixin.qq.com/s/Mi62oqtXUT5If_Dj4KmVYA

计算机视觉如何知道你想看什么？个人显著性检测

https://mp.weixin.qq.com/s/47TcGoasB9E_Et2zwl3OCw

全局对比度的图像显著性检测算法

https://zhuanlan.zhihu.com/p/65307842

快、好、实现简单并且开源的显著性检测方法

https://mp.weixin.qq.com/s/tmp1HXU7cLerLr0DY9NluQ

杂乱环境下的显著性物体： 将显著性物体检测推向新高度

https://mp.weixin.qq.com/s/urgkUcu2ZWQMGPZdArWzYg

PoolNet：基于池化技术的显著性目标检测

https://zhuanlan.zhihu.com/p/71538356

BASNet，一种能关注边缘的显著性检测算法

https://mp.weixin.qq.com/s/ntSH2aS4YHqrLaTAfWFLsQ

可选择性与不变性：关注边界的显著性目标检测

https://mp.weixin.qq.com/s/0T1QhiT_20BrerNcTjKreQ

南开提出边缘引导的显著目标检测算法EGNet，刷新主流数据集所有评价指标

https://mp.weixin.qq.com/s/p4lHnte3FYu6XtD3PnSeKw

光场显著性检测研究综述

https://mp.weixin.qq.com/s/8QrNvb-1zmrTWo5zThpyvg

U²-Net：使用显著性物体检测来生成真实的铅笔肖像画

# 两弹一星+

在闵嗣鹤的努力下，北京大学数论方向的人才培养卓有成效，其中潘承洞、潘承彪先生受益于闵嗣鹤的教育，而后他们又培养出张益唐、刘建亚等数论方向的杰出学者。

1969年，闵嗣鹤参加研制当时急需的海上勘探设备——海洋重力仪，这是北京大学数学力学系与北京地质仪器厂的合作科研项目。由于西方禁运，资料缺乏，工作困难很大。设计该设备的理论关键是滤波问题，要在理论上保证所设计的重力仪能成功地从五万倍强噪声背景中提取出有用的微弱信号。他先是完成了“数字滤波的若干分析问题”等研究，最后提出了“契贝谢夫权系数的数字滤波方法”，从理论上解决了设计中的关键问题。由此设计制造出的仪器定名为ZY－I型海洋重力仪，历经5年海上实验于1975年通过国家鉴定，其性能远远优于日本依据3次平均法设计制造的“东京a－1号”，成为我国大面积海洋普查的先进工具。

https://www.zhihu.com/question/564799818

数学家张益唐攻克Landau-Siegel零点猜想相关论文发布，如何评价这一研究的成果及意义？

---

武钢说的百亿矿山是在2012两会上吹牛用的，要么入了点股，如澳大利亚威拉拉铁矿只是入了10%的股，要么是小矿山，如拿大魁北克省的Bloom lake矿，全部投产完成只有年千万吨产量。要么是一毛不拔之地，如澳大利亚的艾尔半岛铁矿，加拿大的Lac Otelnuk矿区、Rainy Lake矿区和Hayot Lake矿区都还在前期可行性研究和资源勘探工作。

一直到2010年才拿下利比里亚邦矿65%股权。储量13亿吨，但是品味低，仅有35%，投了巨额资金下去，到2014年才采出第一批矿，但是铁矿石在2013年底开始暴跌，又无心弄二期了，远没有购买澳大利亚高品质的矿来的合算。

就算拿到高品质矿也可能底裤赔光，中信泰富的2006年3月底购买的两个澳大利亚铁矿，品质是高，但是在一毛不拔之地，造了公路造管道，造了管道造港口，造了港口造发电厂，造了发电厂又造海水淡化厂，生生造了一座城出来。又加上中冶这个坑货，预算投资42亿美元，最后弄的超百亿美元。废了九牛二虎之力好不容易出矿了，铁矿石暴跌，赔大发了，真是继续开采巨亏，不开采前提投资又打水漂。

中冶2008年收购的澳大利亚兰伯特角铁矿资源19.15亿吨，花了249亿，发现是个磁铁矿，不具备开采价值。鞍钢投资的卡拉拉铁矿也是个磁铁矿。都是眼睛都不长的。

中铝也是坑货，2008次贷危机，必和必拓欠下340亿美元债务，眼看要倒闭了，中铝打着入股名义借给它160亿，必和必拓拿到钱后还了外债，熬过寒冬马上翻脸不认人，不让入股了，中铝仅拿到1亿违约金。

中铝收购必和必拓时合同约定违约金为合同金额的百分之一，如此优惠条件外国人不违约都不好意思，最后一句“交学费”就皆大欢喜啦。

宝钢也是一样，2009年度铁矿石议价，由商务部指定中钢协统一谈判，关键时候宝钢来个背刺，跳过中钢协与三大矿达成涨价65%协议，此后价格联盟全线雪崩。

央企对外投资十投九亏，反正糟蹋的不是自己的钱，中饱私囊，澳洲华人亿万富翁倒是暴涨了。

相反，地方和民企对外投资做的更好。

湖南华菱在中铝与FMG谈判失败后，自动找到FMG谈判，最后入股成为第二大股东。此后FMG迅速成长，成为全球第四大铁矿企业，几乎全部的铁矿石都出口中国，华菱不仅获得稳定的铁矿石来源，也获得几十倍的投资回报。

青山几乎垄断印尼镍矿开采，开设了4大工业园区，带着几十小弟进入印尼开采各种矿产。

---

Kyligence CEO韩卿：

我毕业后加入的第一家公司叫做：浙大中控（中控集团），一家提供自动化与信息化技术和产品的高科技公司。自动化，非常抽象，说的简单点，化工厂，啤酒厂，水泥厂等流程行业，各个环节的控制不可能靠人工去监控和调节各种参数、温度等，必须有一整套的自动化系统来管理和控制，确保稳定的工艺，及更重要的安全性等。然而，这样一套系统，以前都是被霍尼韦尔等国外巨头垄断，每个化工厂都需要购买，且没有谈判价格的余地。今天，依然记得很清楚，入职培训的时候，负责技术的金总讲过的那些故事。中控在那个年代，做出的第一代产品推销极难，只能在浙江的小厂不断实验和验证，不断的获取客户的信任，不断改进产品和技术。艰苦的几年后终于获得了行业的认可，当有能力做到领先水平的时候，国外巨头却不断降价来封杀来竞争。不过竞争的结果，中控在国内今天是自动化控制行业的龙头，承担863计划，有博士后工作站，不得不说，如果没有中控，作为基础行业的化工等行业在这块的技术极大可能还将被垄断在国外巨头手中。

![](/images/img2/China_USA.jpg)
