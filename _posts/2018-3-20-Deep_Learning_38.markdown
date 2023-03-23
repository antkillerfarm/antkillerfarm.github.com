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

# 俄乌战争++

乌克兰4100多万人，共有1037所高校大学，每3.95万人一所大学。作为对比，中国14亿人，截止22年共有2956所高校大学，每47.36万人一所大学。

苏25是第比利斯产的，毛子有没有能力重开苏25生产线都不好说。。。

“动物园”（1L260反炮兵雷达）包含两个平行项目分别是俄罗斯设计局主导的动物园-1和乌克兰设计局主导的动物园-2。最后乌克兰设计开发的动物园-2在2003年先服役，而俄罗斯的动物园-1则拖到了2008年。但动物园-1一经服役就被指出和它的前任（1RL239“山猫”）一样“完全无法被采用”。

AZK-7（自动声源炮兵校射系统）是本次战争前线很喜欢的新装备，但这本质上还是一个乌克兰生产的系统。它是80年代中期由天才的苏联工程师O.M.马尔琴科和V.B.斯马金在敖德萨市的Molniya设计局内开发，并于1988年被苏联军队采用。俄罗斯国防部将AZK-7M吹得天花乱坠，声称是本世代最优秀的系统，但所谓的改进力度也没有能颠覆原本创造者的范畴。

Zorya-Mashproekt（曙光设计局）不再为俄罗斯提供技术支持，这导致“库兹涅佐夫”号航母本来就问题重重的动力系统完全无法维护。根据进口替代计划，俄罗斯人设计了KVG-6M等多种型号动力设备以替换乌克兰的产品，但是之前乌克兰的动力系统只是不好使，而俄罗斯自制的设备是完全没法用。

去年，俄罗斯军方开始寻求从中国进口GT-25000型燃气轮机，为格里戈罗维奇级护卫舰提供动力，这基本说明俄罗斯的进口替代战略已完全失败。而GT-25000型燃气轮机的技术就是来自于乌克兰，是中船重工1993年从Zorya-Mashproekt引进然后改造的，在某种意义上说，俄罗斯人还是离不开乌克兰的技术。

莫洛佐夫设计局+马雷舍夫（又译马利雪夫）工厂：苏联哈尔科夫机械厂制造了著名的T-34坦克。当原总设计师米哈伊尔·伊里奇·科什金积劳成疾，于1940年9月26日与世长辞时，科什金的学生和挚友亚历山大·A·莫洛佐夫接任了KB-520坦克设计局总工程师的职务。

哈尔科夫机车厂在制造T-12坦克之前只制造过拖拉机，所有生产坦克必需的特种制造设备他们都没有，工人们也没有制造坦克的经验，但尽管条件艰苦且缺乏经验，T-12原型车的制造工作仍然进展很快。1929年底，T-12坦克原型车下线。

1931年，在哈尔科夫机车厂的围墙内，一群才华横溢的苏联工程师K.F. Chelpan, T.P. Chupakhin, Ya.E. Vikhman, I.Ya. Trashutin创造了V-2柴油发动机。是的，就是红极一时的T-34、KV和IS的心脏。

卫国战争期间，该局和工厂迁移至乌拉尔地区，所以它也是现俄罗斯乌拉尔坦克工厂的前身。

2019年11月，乌克兰总统泽连斯基视察哈尔科夫，发现哈尔科夫马雷舍夫机械制造厂已经停工，近十年来只生产过1辆坦克，一度以修理战车度日，而在苏联时期，该厂年产坦克近千辆。

伊夫琴科设计局：阿列克萨恩德尔·格奥尔基耶维奇·伊夫琴科。马达西奇公司只是伊夫琴科设计局的代工厂。而伊夫琴科设计局是乌克兰的国企，乌克兰就算再败家子，也知道这玩意不能卖。

作为前苏联工业制造中心的乌克兰，军工科技人员研制出温压弹根本不稀奇，没有任何技术门槛。关键是乌军创新研制出9公斤级小型温压弹，适合无人机投放，的确令人刮目相看。

南方设计局的工程师Nippo Biedimeyer，从1982年开始，研制一种改进的“奥卡”导弹，具有末端制导和更精确的打击。改进后的射程不仅增加到500公里，还旨在突破对方的防空系统。弹头的外部隐身涂层、电子干扰设备和复杂的弹道设计，让当时的美国爱国者系统束手无策。奥卡在后来苏联入侵阿富汗的行动中大显神威，一度取代了战士的位置，甚至被阿富汗游击队称为“种族灭绝”。俄军的Iskander导弹就是OCA技术的升级版。

近日，俄媒称乌克兰要复产“奥卡”。看起来俄军将太多的导弹消耗在攻击乌克兰电力基础设施上了，这让乌克兰军工业获得了一定的喘息和恢复时间。

https://www.zhihu.com/question/542861406

“库兹涅佐夫”号航母的维修工作为什么还没有完成?

https://zhuanlan.zhihu.com/p/591844298

苦痛之路——俄罗斯炮兵不能揭开的伤疤

https://www.163.com/dy/article/HPTSHMQP05560MC2.html

乌克兰研制的小型温压弹在战场显神威

https://mp.weixin.qq.com/s/tRvGxYldNoH9N59ug66ypA

从六千多辆缩水到一千辆：乌克兰坦克部队发生了什么？

https://zhuanlan.zhihu.com/p/595998197

为什么T-14阿玛塔还没有出现在俄乌前线？

https://view.inews.qq.com/k/20230103A06OPP00

俄媒称乌克兰要复产“奥卡”，“奥卡”有多可怕：当年连生产线都要被销毁

---

2014年7月，瑞典新纳粹分子、前本土防卫军狙击手米尔凯·斯基尔特加入“亚速”营。但他在2015年8月公开宣布：他在乌克兰的经历改变了自己，自己将不再相信纳粹主义，因为他在乌克兰的所见所闻让自己认识到——自己过去的纳粹思想是错误而愚蠢的。

https://mp.weixin.qq.com/s/wp8CBSc2cBJBddSws2ktzw

乌克兰正在进一步走向分裂！这次不在东部，而是西部。
