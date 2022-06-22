---
layout: post
title:  线性代数（四）——病态矩阵
category: math 
---

* toc
{:toc}

# 病态矩阵

## 矩阵的条件数（续）

然而这个定义，在病态矩阵的条件下，并不能直接用于数值计算。因为浮点数所引入的微小的量化误差，也会导致求逆结果的很大误差。所以通常情况下，一般使用矩阵的特征值或奇异值来计算条件数。

假设A是2阶方阵，它有两个单位特征向量$$x_1,x_2$$和相应的特征值$$\lambda_1,\lambda_2$$。

由之前的讨论可知，$$x_1,x_2$$是相互正交的。因此，向量b能够被$$x_1,x_2$$的线性组合所表示，即：

$$b=mx_1+nx_2=\frac{m}{\lambda_1}\lambda_1x_1+\frac{n}{\lambda_2}\lambda_2x_2=A(\frac{m}{\lambda_1}x_1+\frac{n}{\lambda_2}x_2)$$

从这里可以看出，b在$$x_1,x_2$$上的扰动，所带来的影响，和特征值$$\lambda_1,\lambda_2$$有很密切的关系。奇异值实际上也有类似的特点。

因此，一般情况下，条件数也可以由最大奇异值与最小奇异值之间的比值，或者最大特征值和最小特征值之间的比值来表示。这里的最大和最小，都是针对绝对值而言的。

参见：

https://en.wikipedia.org/wiki/Condition_number

## 矩阵规则化

病态矩阵处理方法有很多，这里只介绍矩阵规则化（regularization）方法。

机器学习领域，经常用到各种损失函数（loss function）。这里我们用：

$$\min_f \sum_{i=1}^nV(f(\hat x_i),\hat y_i)$$

表示损失函数。

当样本数远小于特征向量维数时，损失函数所表示的矩阵是一个稀疏矩阵，而且往往还是一个病态矩阵。这时，就需要引入规则化因子用以改善损失函数的稳定性：

$$\min_f \sum_{i=1}^nV(f(\hat x_i),\hat y_i)+\lambda R(f)$$

其中的$$\lambda$$表示规则化因子的权重。

>注：稀疏矩阵并不一定是病态矩阵，比如单位阵就不是病态的。但是从系统论的角度，高维空间中样本量的稀疏，的确会带来很大的不确定性。可类比下围棋，棋子过于稀疏的地方，只能称作势力范围，而不能称作实地。

函数V（又叫做Fit measure）和R（又叫做Entropy measure），在不同的算法中，有不同的取值。

比如，在Ridge regression问题中：

$$\text{Fit measure}:\|Y-X\beta\|_2,\text{Entropy measure}:\|\beta\|_2$$

Ridge regression问题中规则化方法，又被称为$$L_2$$ regularization，或Tikhonov regularization。

>注：Andrey Nikolayevich Tikhonov，1906~1993，苏联数学家和地球物理学家。大地电磁学的发明人之一。苏联科学院院士。著有《Solutions of Ill-posed problems》一书。

更多的V和R取值参见：

https://en.wikipedia.org/wiki/Regularization_(mathematics)

从形式上来看，对比之前提到的拉格朗日函数，我们可以发现规则化因子，实际上就是给损失函数增加了一个约束条件。它的好处是增加了解向量的稳定度，缺点是增加了数值解和真实解之间的误差。

为了更便于理解规则化，这里以二维向量空间为例，给出了规则化因子对损失函数的约束效应。

![](/images/article/L1_vs_L2.png)

上图中的圆圈是损失函数的等高线，坐标原点是规则化因子的约束中心，左图的方形和右图的圆形是$$l_p$$ ball。图中的黑点是等高线和$$l_p$$ ball的交点，实际上也就是这个带约束的优化问题的解。

可以看出$$L_1$$ regularization的解一般出现在坐标轴上，因而其他坐标上的值就是0，因此，$$L_1$$ regularization会导致矩阵的稀疏。

$$L_1$$ regularization又被称为Lasso（least absolute shrinkage and selection operator） regression。

从贝叶斯统计的角度来看，规则化相当于增加了一个先验分布，比如$$L_2$$ regularization对应的贝叶斯先验分布是Gaussian distribution，而$$L_1$$ regularization对应的贝叶斯先验分布是Laplace distribution。

除此之外，还有混合使用规则化的情况，比如：

$$\min_f \sum_{i=1}^nV(f(\hat x_i),\hat y_i)+\lambda \|\beta\|_1 + \eta  \|\beta\|_2$$

这种方法也被称为弹性网络回归（ElasticNet Regression）。

规则化同时也提供了一种衡量特征重要度的方法：**loss函数的值，如果显著小于规则项，则说明该特征不太重要。**

参考：

https://en.wikipedia.org/wiki/Tikhonov_regularization

http://www.mit.edu/~cuongng/Site/Publication_files/Tikhonov06.pdf

http://blog.csdn.net/zouxy09/article/details/24971995

机器学习中的范数规则化之（一）L0、L1与L2范数

https://mp.weixin.qq.com/s/pZko9gM5sbFhMl8P8TCFww

机器学习损失函数、L1-L2正则化的前世今生

https://mp.weixin.qq.com/s/pNG8u8V7zp6fLFF9TomEug

史上最全面的正则化技术总结与分析

https://mp.weixin.qq.com/s/PMisvVy4EwEF-5xEY5LrwA

从损失函数的角度详解常见机器学习算法

https://mp.weixin.qq.com/s/MRabAUZrfgD2t2GhnLI43Q

开发者必读：计算机科学中的线性代数

https://mp.weixin.qq.com/s/ctLe1UbvWqBJ8jh-ppU3rA

机器学习中的五种回归模型及其优缺点

https://mp.weixin.qq.com/s/zzCRiSSyfTAFYSGMnr8zfg

机器学习中L1和L2正则化的直观解释

https://mp.weixin.qq.com/s/xwYldlEjJ9Co9uo8o0mlKQ

深度学习之DNN的多种正则化方式

https://mp.weixin.qq.com/s/-axtm6ZBm8yYneiA3mvQrw

SIGIR 2018大会最佳短论文：利用对抗学习的跨域正则化

https://mp.weixin.qq.com/s/FtWA1rff13e7_FM0lJKCVg

Petuum提出新型正则化方法：非重叠促进型变量选择

https://zhuanlan.zhihu.com/p/50142573

L1正则化引起稀疏解的多种解释

https://blog.csdn.net/daunxx/article/details/51596877

Lasso Regression

https://mp.weixin.qq.com/s/kFEwvgBC4-Y05vL3lmRbXQ

正则的一些intuition，一分钟发明新正则

https://mp.weixin.qq.com/s/oZZGBZLRoJVsY7M-CP5MRg

矩阵L2,1/L2,p范数扫盲

https://mp.weixin.qq.com/s/poSfjftItmHERs3MDgvjpw

机器学习防止模型过拟合的方法知识汇总

https://mp.weixin.qq.com/s/YPtGqUXnRQ2qswpBLsbM7g

Lasso和Ridge回归中的超参数调整技巧

# Krylov subspace

Krylov subspace是一类针对大矩阵的近似计算的方法，由Aleksey Nikolaevich Krylov于1931年提出。

>Aleksey Nikolaevich Krylov，1863～1945，俄罗斯海军工程师、数学家、作家。

参考：

https://blog.csdn.net/lizhengjiang/article/details/18794275

krylov子空间迭代法

https://www.zhihu.com/question/23309010

如何使用Krylov方法求解矩阵的运算尤其是逆？

# 海军

不要说两艘巡洋舰打一艘战列舰，就算是四五艘巡洋舰打一艘战列舰，除非能够冲到鱼雷距离，否则都是被战列舰点名的命。但一万多吨的巡洋舰显然不能学驱逐舰扭来扭去，因此“除非能够冲到鱼雷距离”这句话可以直接删掉。

相比之下，轻巡洋舰学派提倡的“轻巡洋舰”或者说现实中的超级驱逐舰，她们靠着高闪避，高雷击，多用途和快速建造成为了二战性价比最高的水面舰艇。比她小的被她欺压，比她大的被她围殴。

如果是一个超级驱逐舰中队遭遇一条落单的战列舰，则战局可能变得非常微妙。因为太平洋战争的实战表明，舰炮未必能在超级驱逐舰进入鱼雷射程前阻止其前进。

CVSSDD三分天下，这是二战对海战的最终结论。至今这个结论仍未过时。

CV：航空母舰

SS：潜艇

DD：驱逐舰

https://www.zhihu.com/question/414077387

二战时期，在理想状况下，两艘落单巡洋舰能不能硬刚一艘落单战列舰？

---

1949年10月，时任四野第十二兵团司令员的萧劲光接毛泽东急电，日夜兼程赶到北京中南海。毛泽东让他去当海军司令员。“我是个‘旱鸭子’，还晕船，哪能当海军司令？”

1950年3月17日，海军司令员萧劲光乘渔船视察刘公岛。

2019.12.17 山东号航空母舰入列。

我国先后获得四艘航空母舰，分别是从澳大利亚购买的墨尔本号，从韩国购买的明斯克号，从乌克兰购买的瓦良格号，从俄罗斯购买的基辅号。

![](/images/img3/Carrier.jpg)

![](/images/img3/Carrier_2.jpg)

如果你有一艘航母，你就破坏了你所在地区的稳定。

如果你有两艘，你就会对全球秩序构成威胁。

如果你有一打，你就是全球维和人员。

CTOL（conventional take-off and landing）

STOVL（Short Take Off Vertical Landing）

052D/055 导弹驱逐舰

075 两栖攻击舰

HMS是His/Her Majesty's Ship的缩写，意为国王/女王陛下的船舰，是某些君主国家对其海军船舰的正式或非正式船舰称号。

作为我国第二款隐身战斗机，也是被称为“海飞丝”的四代隐身舰载机。

https://zhuanlan.zhihu.com/p/196773456

中国的底牌：万一战争爆发，我们能否顶住？

https://www.guancha.cn/ShiYang/2016_10_26_378389.shtml

施洋：“辽宁号”会像“库兹涅佐夫”一样冒黑烟么

https://mp.weixin.qq.com/s/n1ys7Np2vwPiAHjCnHbUSw

现代级福州舰升级，详细数据忆当年：导弹亚音速高炮靠手摇

https://zhuanlan.zhihu.com/p/427871314

珠联璧合：当003型航母加上歼-31，2025年的中国海军究竟有多强？

https://zhuanlan.zhihu.com/p/438944213

海军20艘056全部转隶海警部队，中国周边想“挑事儿”的要当心了！

---

巡洋舰以上：由国务院特别命名（航母）。

巡洋舰：以行政省（区）或直辖市命名（当然国内没有巡洋舰，国内最后一艘巡洋舰是“重庆”号）。

驱逐舰：以“大、中城市”命名。

护卫舰：以“中、小城市”命名。

综合补给舰：以“湖泊”命名。

弹道导弹核潜艇/攻击型核潜艇：以“长征”加序号命名。

常规鱼雷潜艇：以“长城”加序号命名。

扫雷舰/猎潜艇：以“州”或“县”命名。

船坞登陆舰、坦克登陆舰：以“山”命名。

步兵登陆舰:以“河”命名辅助舰船以所在海区和性质的名称+序号命名。

技术验证船/训练船以人员名称命名（世昌号国防动员舰）。

---

镇海号水上飞机母舰俘获三江号，也是我国航母或水上飞机母舰至今为止取得的惟一一次战果。

https://www.zhihu.com/answer/1516015740

海军史上有什么开挂一样的史实？

https://mp.weixin.qq.com/s/JQg1idg_mPnkForuuJQSoA

写在辽宁号之前，说一说中国海军早期的航空舰船
