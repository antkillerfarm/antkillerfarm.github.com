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

# 明+

清军以绝对优势的兵力猛攻了二十天，直到七月十六日金华才被攻破。朱大典带领家属和亲信将校来到火药局，用绳索捆在火药桶上，点燃引线，轰然一声，壮烈成仁。朱大典在明末官场上以贪婪著称，然而当民族危难之时他却破家纾难，体现了威武不能屈的气节。

朱大典贪婪之名最早是出自东林党人张岱的记述还有东林党人黄宗羲的学生邵廷采所著的《东南纪事》。而朱大典跟东林党又是长期的政敌关系，东林党人所说的朱大典贪婪的可信度有多少，是否真实客观就很值得怀疑了。

---

大清兵破浙兵大阵而入，数千人围仲揆于阵前。其亲兵者五人皆出，皆乱刃斩死。劝其降，仲揆勒甲持刀而出，众将皆上前迎战，其竟连杀十七人。众将惶恐退之，引万箭齐发，仲揆被乱箭射死。

童仲揆是万历年间的武举人，是金陵卫军户出身，曾在南京、浙江、四川等多地任官，参与过征缅和平定播州杨应龙之乱。去辽东时已经六十多岁了。

# 清+

自郴州进发长沙，太平军8天时间就跨越600里山路，一路连克4县，平均每日行军75华里，清军惊呼“无一兵一勇与之面者”。

醴陵之战，太平军9月7日寅时起程，“分由小路”进军，昼夜兼程，一路星驰，先锋曾水源于9月8日子时即一气呵成直下醴陵县城，创造了连续强行军18-20小时，跋涉170华里的军事奇迹。

---

走这条贸易线的人的后台极硬，顺治九年在登州被查的一起走私案中，船上不仅发现了弓箭这样的军事物资，甚至还发现了束发用的网巾，本来按清朝的作风，这得值好几个满门抄斩，结果只把船长打了一顿板子，这件事就过去了。当地官府报告，船上发现的网巾是小孩的玩具，而那个小孩碰巧落水淹死了。如此扯淡的理由都能敷衍清廷，可见跑走私的人后台多硬。

---

广州将军，武职从一品官。广州将军的官阶与两广总督相同，地位却比其更高，全省绿营兵要受广州将军节制。

广州将军地位高于两广总督（虽然和平时期不如两广总督实惠）。清代很特殊，为了防止汉人造反，驻防将军地位很高且必须由旗人担任，不受地方督抚干扰。广东官员有事集体上奏时，广州将军领衔。

满清更像是一个殖民政权，核心是满洲八旗。广州将军相当于两广地区八旗军队的总指挥官，正儿八经的“皇军”，两广总督节制的都是“伪军”啊。

https://www.zhihu.com/question/623203807

电影《武状元苏乞儿》里面的广州将军的级别有多高？

# 宋+

历史上的大理段氏，在五代后晋时期建立大理国，统治了云南地区318年（937年—1254年），随后投降蒙古军，成为蒙古汗国和元帝国的大理总管，又继续统治了今天云南中西部地区128年，于明初洪武十五年（1382年）被明军攻灭，作为我国云南地区统治者一共存在了446年。

https://www.zhihu.com/answer/524642062

为什么金庸先生在《天龙八部》里会把大理段氏写成一个江湖味十足的皇权，难道历史上这个政权真的很江湖么？

---

三百户为谋克，十谋克为猛安。

---

https://www.zhihu.com/question/524124918

北宋拿回了几代人心心念念的幽云十六州，为什么那里的汉人反而日子过得糟糕了？

https://www.zhihu.com/question/313988961

北宋的种家军为何没有杨家将有名？

# 秦汉+

康成文婢：东汉郑玄，博通今古，对经学颇有造诣。家中的奴婢，因耳濡目染多了，也知书达礼，动辄说话便引经文。

郑玄家奴婢皆读书。尝使一婢不称旨，将挞之，方自陈说，玄怒，使人曳著泥中。须臾，复有一婢来，问曰：“胡为乎泥中?”答曰：“薄言往诉，逢彼之怒。”

“胡为乎泥中”：出自《诗·邶风·式微》。

“薄言往诉，逢彼之怒”：出自《诗·邶风·柏舟》。

郑玄家里的奴婢都曾读书。他曾经指使一婢女不称意旨，将要鞭打她，那婢女正在自我辩解，郑玄生了气，让人把她拉到泥水中。过了一会儿，又有一婢女过来，问道：“为什么在泥中?”她回答说：“我急忙地申说，谁知反碰上他发怒。”
