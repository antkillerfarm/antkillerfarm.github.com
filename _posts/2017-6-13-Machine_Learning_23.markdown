---
layout: post
title:  机器学习（二十三）——Optimizer
category: ML 
---

# 时间序列分析+

https://mp.weixin.qq.com/s/iKM6zMSm1F2icjy79F9Hcg

季节性的分析才不简单，小心不要在随机数据中也分析出季节性

https://mp.weixin.qq.com/s/avLWHXj2JkjXOomCipj8kA

使用希尔伯特-黄变换（HHT）进行时间序列分析

https://mp.weixin.qq.com/s/p8oN4xh-FHnay2eTsk6Gng

基于高阶模糊认知图与小波变换的时间序列预测

https://mp.weixin.qq.com/s/lmJk-iIzxxPmnZa6D8i_nw

一文简述如何使用嵌套交叉验证方法处理时序数据

https://mp.weixin.qq.com/s/05WAZcklXnL_hFPLZW9t7Q

时间序列模型之相空间重构模型

https://mp.weixin.qq.com/s/rIgjtILF7EtuBS5UWCEFcQ

重大事件后，股价将何去何从？

# 金融模型

Capital asset pricing model

Fama–French three-factor model

Carhart four-factor model

# Optimizer

在《机器学习（一）》中，我们已经指出梯度下降是解决凸优化问题的一般方法。而如何更有效率的梯度下降，就是本节中Optimizer的责任了。

## 原始版本

早期的梯度下降法一般用以下公式确定学习率：

$$\eta_t=\frac{\eta_0}{\sqrt{t+1}}$$

## Momentum

Momentum是梯度下降法中一种常用的加速技术。其公式为：

$$v_t = \gamma v_{t-1} + \eta \nabla_\theta J( \theta)$$

$$\theta = \theta - v_t$$

从上式可以看出，参数的更新值$$v_t$$，不仅取决于当前梯度$$\nabla_\theta J( \theta)$$，还取决于上一刻的速度$$v_{t-1}$$。

## Nesterov accelerated gradient

该方法是Momentum的一个变种。其公式为：

$$v_t = \gamma v_{t-1} + \eta \nabla_\theta J( \theta - \gamma v_{t-1})$$

$$\theta = \theta - v_t$$

>注：Yurii Nesterov，莫斯科大学应用数学系本科（1977年），凸优化理论专家。法国鲁汶天主教大学教授。2009年获John von Neumann Theory Prize。

参考：

https://zhuanlan.zhihu.com/p/22810533

比Momentum更快：揭开Nesterov Accelerated Gradient的真面目

https://zhuanlan.zhihu.com/p/27435669

从Nesterov的角度看：我们为什么要研究凸优化？

https://www.cs.cmu.edu/~ggordon/10725-F12/slides/09-acceleration.pdf

Accelerated first-order methods

## Adagrad

Momentum算法中所有的参数$$\theta$$都使用同一个学习率，而Adagrad采用了另一种方法进行优化：为每个参数确定不同的学习率。

Adagrad的基本思想：给经常更新的参数一个较小的学习率，而给很少更新的参数一个较大的学习率。

其公式为：

$$g_{t, i} = \nabla_\theta J( \theta_i )$$

$$\theta_{t+1, i} = \theta_{t, i} - \dfrac{\eta}{\sqrt{G_{t, ii} + \epsilon}} \cdot g_{t, i}$$

其中，$$G_{t, ii}$$表示参数$$\theta_i$$梯度平方和的历史累积值，$$\epsilon$$是为了防止分母为0，而加入的平滑项，数量级一般为$$10^{-8}$$。

有趣的是，如果去掉上式中的根号，则其效果会变糟。

Adagrad的优点在于：它是一个自适应算法，初值选择显得不太重要了。

Adagrad的缺点在于：训练越往后，G越大，从而学习率越小。如果在训练完成之前，学习率变为0，就会导致提前结束训练。

## Adadelta

为了克服Adagrad的缺点，Matthew D. Zeiler于2012年提出了Adadelta算法。

>注：Matthew D. Zeiler，多伦多大学本科（2009）+纽约大学博士（2013）。Clarifai创始人和CEO。读书期间，他还创立了一家给大学生卖习题册的公司。   
>个人主页：   
>http://www.matthewzeiler.com/

该算法不再使用历史累积值，而是只取最近的w个状态，这样就不会让梯度被惩罚至0。

为了避免保存前w个状态的梯度平方和，可做如下变换：

$$E[g^2]_t = \gamma E[g^2]_{t-1} + (1 - \gamma) g^2_t$$

$$\theta_{t+1} = \theta_{t} - \dfrac{\eta}{\sqrt{E[g^2]_t + \epsilon}} g_{t}$$

上边的公式，就是Hinton在同一年提出的**RMSprop算法**。其中的$$\gamma E[g^2]_{t-1}$$即可看作是前w个状态的滤波值，也可看作是Momentum算法中动量值。

Adadelta在RMSprop的基础上更进一步：

$$RMS[g]_{t}=\sqrt{E[g^{2}]_{t}+\epsilon }$$

$$\Delta \theta_t = - \dfrac{RMS[\Delta \theta]_{t-1}}{RMS[g]_{t}} g_{t}$$

也就是说，Adadelta不仅考虑了梯度的平方和，也考虑了更新量的平方和。

## Adam

Adaptive Moment Estimation借用了卡尔曼滤波的思想，对$$g_t,g_t^2$$进行滤波：

$$m_t = \beta_1 m_{t-1} + (1 - \beta_1) g_t$$

$$v_t = \beta_2 v_{t-1} + (1 - \beta_2) g_t^2$$

估计：

$$\hat{m}_t = \dfrac{m_t}{1 - \beta^t_1}$$

$$\hat{v}_t = \dfrac{v_t}{1 - \beta^t_2}$$

更新：

$$\theta_{t+1} = \theta_{t} - \dfrac{\eta}{\sqrt{\hat{v}_t} + \epsilon} \hat{m}_t$$

## AdamW

Adam自动调整学习率，大幅提高了训练速度，也很少需要调整学习率，但是有相当多的资料报告Adam优化的最终精度略低于SGD。问题出在哪呢，其实Adam本身没有问题，问题在于目前大多数DL框架的L2 loss实现用的是weight decay的方式，而weight decay在与Adam共同使用的时候有互相耦合。

为了解决这个问题，人们又提出了AdamW。

参考：

https://mp.weixin.qq.com/s/TVJIC7IEUeWypN8Z1NGKaA

都9102年了，别再用Adam + L2 Loss了

## Nadam

http://cs229.stanford.edu/proj2015/054_report.pdf

ncorporating Nesterov Momentum into Adam

## AdaSecant

《ADASECANT: Robust Adaptive Secant Method for Stochastic Gradient》

## 二阶Optimizer

虽然二阶Optimizer的收敛效果优于一阶Optimizer，但由于计算量较大，通常用的较少。

常用的算法有BGFS和L-BFGS。

参考：

http://www.cnblogs.com/kemaswill/p/3352898.html

优化算法-BFGS

http://blog.csdn.net/acdreamers/article/details/44728041

L-BFGS算法

https://mp.weixin.qq.com/s/lGrTUYALmKOQkO70DZpbPQ

小改进，大飞跃：深度学习中的最小牛顿求解器

## 参考

http://sebastianruder.com/optimizing-gradient-descent/

An overview of gradient descent optimization algorithms

https://mp.weixin.qq.com/s/k_d02G2V4yd6HdGfw2mf1Q

从修正Adam到理解泛化：概览2017年深度学习优化算法的最新研究进展

https://mp.weixin.qq.com/s/cOCCapYrmrS_DyPkj_XRlg

常见的几种最优化方法

https://morvanzhou.github.io/tutorials/machine-learning/ML-intro/3-06-speed-up-learning/

加速神经网络训练

http://www.cnblogs.com/neopenx/p/4768388.html

自适应学习率调整：AdaDelta

https://mp.weixin.qq.com/s/VoBK-l_ieSg2UupC2ix2pA

听说你了解深度学习最常用的学习算法：Adam优化算法？

https://mp.weixin.qq.com/s/YRyqvlNe24mlFZ7GB9vDnw

一文看懂常用的梯度下降算法

https://mp.weixin.qq.com/s/qncTSBCvjMzAual5Sz9R3A

解析深度学习优化：Momentum、RMSProp 和 Adam

https://mp.weixin.qq.com/s/q7BI-YyhtmNzUfBMTKVdqQ

Hitting time analysis of SGLD！

https://mp.weixin.qq.com/s/vt7BEHbwJrAzlL2Pc-6QFg

掌握机器学习数学基础之优化（上）

https://mp.weixin.qq.com/s/6NBLLLa-S625iaehR8zDfQ

掌握机器学习数学基础之优化（下）

https://mp.weixin.qq.com/s/o10Fp2VCwoLqgzirbGL9LQ

如何估算深度神经网络的最优学习率

https://mp.weixin.qq.com/s/T4f4W0V6YNBbjWqWBF19mA

目标函数的经典优化算法介绍

https://mp.weixin.qq.com/s/R_0_E5Ieaj9KiWgg1prxeg

为什么梯度的方向与等高线切线方向垂直？

https://mp.weixin.qq.com/s/0gdGNv98DytB8KxwVu_M0A

通俗易懂讲解Deep Learning最优化方法之AdaGrad

https://mp.weixin.qq.com/s/VVHe2msyeUTGiC7f_f0FFA

一文概览深度学习中的五大正则化方法和七大优化策略

https://mp.weixin.qq.com/s/qp5tJynA2uZIgv-IzJ_lrA

从基础知识到实际应用，一文了解“机器学习非凸优化技术”

https://mp.weixin.qq.com/s/zFGQzC_uQdAwlr9BzA-CYg

深度学习需要了解的四种神经网络优化算法

https://mp.weixin.qq.com/s/rUqIfKWmEBVjajlAn2HXfg

理解深度学习中的学习率及多种选择策略

https://mp.weixin.qq.com/s/UfplwSgyWnLNiCdIrconhA

SGD的那些变种，真的比SGD强吗

https://zhuanlan.zhihu.com/p/73441350

从物理角度理解加速梯度下降

https://mp.weixin.qq.com/s/n1Ks8I3Ldgb-u-kVbGBZ5Q

机器学习中的优化方法

https://mp.weixin.qq.com/s/4XOI8Dq6fqe8rhtJjeyxeA

超级收敛：使用超大学习率超快速训练残差网络

http://mp.weixin.qq.com/s/Q5kBCNZs3a6oiznC9-2bVg

Michael Jordan新研究官方解读：如何有效地避开鞍点

https://mp.weixin.qq.com/s/idmt0F49tOCh-ghWdHLdUw

吴恩达导师Michael I.Jordan学术演讲：如何有效避开鞍点。这是Jordan半年后的另一个演讲，有些新内容。

https://mp.weixin.qq.com/s/jVjemfcLzIWOdWdxMgoxsA

超越Adam，从适应性学习率家族出发解读ICLR 2018高分论文

https://mp.weixin.qq.com/s/B9nUwPtgpsLkEyCOlSAO5A

1cycle策略：实践中的学习率设定应该是先增再降

https://mp.weixin.qq.com/s/dseeCB-CRtZnzC3d4_8pYw

AMSGrad能够取代Adam吗

https://zhuanlan.zhihu.com/p/81020717

从SGD到NadaMax，十种优化算法原理及实现

https://zhuanlan.zhihu.com/p/22252270

深度学习最全优化方法总结比较（SGD，Adagrad，Adadelta，Adam，Adamax，Nadam）