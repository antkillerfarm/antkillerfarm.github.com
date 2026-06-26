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

# 豪门盛宴+

![](/images/img5/powerful_family.jpg)

https://zhuanlan.zhihu.com/p/569778610

七匹狼公子和特步公主的豪门联姻说明了什么？

---

美股七姐妹：苹果、微软、谷歌、亚马逊、英伟达、Meta和特斯拉

---

https://mp.weixin.qq.com/s/AAF4mmxhkD0upb-0SkK4dA

Gucci家族谋杀案：现实中的豪门恩怨远比电影更精彩

---

好利来的创始老板，人送外号当代李渊。

极度昏庸，祸害事业，吃喝享乐，不思进取，包养女明星，搞出私生子，一度想抛妻弃子，放弃圆满家庭和两个嫡子。

被亲儿子发动背刺夺权，手下人众叛亲离，无奈交权。婚也没离成，女明星也没上位，鸡飞蛋打。

老爸三战都失败的北京市场兄弟俩轻松拿下，并且通过网络营销把基业起码扩大了10倍，跟李渊的基业被儿子扩大也差不多。

---

夏威夷华人糖业大王陈芳（Chun Afong，1825-1906），1849年从广东香山梅溪村被卖为"猪仔"（契约劳工）到夏威夷，在甘蔗种植园做工。陈芳的孙女婿邝友良（Hiram Fong，1906-2004），1959年成为美国首位华裔联邦参议员。

---

Tatra的历史

1850年，创始人伊格纳茨·舒斯塔拉（Ignác Šustala）在摩拉维亚的科佩尔尼采（Kopřivnice，当时属奥匈帝国）创立Schustala & Company，主营马车和四轮马车制造。

1902年，传奇设计师汉斯·莱德温卡（Hans Ledwinka，1878-1967）加入公司，主导设计了多款经典车型。被誉为“现代越野卡车底盘设计之父”。

T87豪华轿车：采用后置V8风冷发动机，流线型设计，时速可达160公里。因其高速过弯时容易失控，被戏称为"捷克秘密武器"——多名德军高级军官因此车祸丧生。希特勒本人也曾使用太脱拉轿车。

---

著名男同波利尼亚克亲王在1892年因为投资不善和开销太大喜提破产。他和他的男情人蒙特斯基乌商量如何解决，蒙特斯基乌又与他的表妹伊丽莎白·格雷弗勒讨论，他表妹给他推荐了缝纫机大亨艾萨克·辛格的女儿温纳雷塔·辛格。他俩一个喜欢男的一个喜欢女的，而且都喜欢赞助艺术家，简直是天作之合，于是二人在一年后成婚，两个人过上了一边赞助艺术家一边开后宫的快乐生活——温纳雷塔喜欢同时养好几个女情人。

---

1892年2月16日，美国“镀金时代”的社交女王卡洛琳·阿斯特夫人（Mrs. Caroline Astor）和她的社交助手麦卡利斯特在《纽约时报》公开发表了一份名单。

麦卡利斯特声称：“在纽约上流社会，真正称得上重要、在宴会厅里能泰然自若的，只有大约400人。如果超出这个数字，就会碰上那些在舞会上自己不自在、也让别人不自在的人。”

因此，这份名单被称为“400人名册”（The Four Hundred）。

新兴富豪无法仅凭财富跻身“上流社会”，转而将女儿送往欧洲寻找真正的跨国贵族联姻，从而催生了大量的“美元公主”。

17岁时，康斯薇露·范德比尔特（Consuelo Vanderbilt）秘密与一位英俊的美国青年温斯罗普·鲁瑟福德订婚，阿尔瓦得知此事后大发雷霆，将女儿软禁在房间里，甚至威胁要开枪杀死那个青年。

当时欧美社会流行的是女儿出嫁要给嫁妆，因为女儿日后不会继承家里的其他财产，嫁妆相当于一次性把女儿的那部分随着婚姻送了出去。

温斯顿•丘吉尔的母亲珍妮·杰罗姆，她在1874年和伦道夫·丘吉尔勋爵认识三天宣布订婚，伦道夫勋爵的父母原本不赞同，直到听说珍妮的父亲会给一大笔嫁妆。

简•奥斯丁宇宙——《爱玛》《傲慢与偏见》《曼斯菲尔德庄园》《诺桑觉寺》讲的都是青年男女如何各怀鬼胎试图找到心仪配偶的故事。贝内特姐妹们（以及她们的同位体们）之所以要找有钱的男人，不是因为爱慕虚荣，而是因为她们的父亲贝内特先生攒的钱太少，作为没有嫁妆也没有兄弟的年轻女子，她们只能试一试以美貌觅得合适的夫婿。

奥斯丁宇宙里就有很多姑娘嫁不出去，奥斯丁自己靠兄弟供养。

欧洲各国的殖民地普遍处于（白人）男多（白人）女少的窘境，许多在殖民地身份地位相当高的官员、军官与非常有钱的商人连白人妻子都找不到，罔论找到身份好长得美的白人少女成婚了。在这种没得挑的情况下，只要外貌、身份还过得去的白人少女都能在殖民地找到比自己地位高得多的配偶，至于嫁妆更没人挑了。

https://www.zhihu.com/question/2051395328994235603

为什么《泰坦尼克号》要把Rose设定为贵族身份？
