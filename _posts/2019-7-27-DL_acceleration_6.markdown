---
layout: post
title:  深度加速（六）——知识蒸馏
category: DL acceleration 
---

# 知识蒸馏

知识蒸馏是另一大类的模型压缩方法。

Geoffrey Hinton的论文：

《Distilling the Knowledge in a Neural Network》

![](/images/img2/Distilling.jpeg)

老师网络可以被固定（正如在精炼过程中）或联合优化，甚至同时训练多个不同大小的学生网络。

![](/images/img2/Distilling_2.jpeg)

上图是另一篇论文的图：

《Object detection at 200 Frames Per Second》

该论文的中文版：

https://mp.weixin.qq.com/s/OCG1TiHl2dsuS24uacQ-MA

又快又准确，新目标检测器速度可达每秒200帧

论文：

《Articulatory and Spectrum Features Integration using Generalized Distillation Framework》

参考：

https://github.com/dkozlov/awesome-knowledge-distillation

知识蒸馏从入门到精通

https://zhuanlan.zhihu.com/p/24894102

《Distilling the Knowledge in a Neural Network》阅读笔记

https://luofanghao.github.io/2016/07/20/%E8%AE%BA%E6%96%87%E7%AC%94%E8%AE%B0%20%E3%80%8ADistilling%20the%20Knowledge%20in%20a%20Neural%20Network%E3%80%8B/

论文笔记《Distilling the Knowledge in a Neural Network》

http://blog.csdn.net/zhongshaoyy/article/details/53582048

蒸馏神经网络

https://mp.weixin.qq.com/s/QZ7PGvi27LiDOJaxici7Pw

数据蒸馏Dataset Distillation

https://mp.weixin.qq.com/s/mFuxCl0Mzv5hmDFewWZkrw

FAIR&MIT提出知识蒸馏新方法：数据集蒸馏

https://mp.weixin.qq.com/s/MDgqSwVEClNqNpaWuGTEpg

微软亚研院提出用于语义分割的结构化知识蒸馏

https://zhuanlan.zhihu.com/p/24337627

深度压缩之蒸馏模型

https://blog.csdn.net/xbinworld/article/details/83063726

知识蒸馏（Distilling the Knowledge in a Neural Network），在线蒸馏

https://mp.weixin.qq.com/s/ekKg46bQlWrlg9Hon01M5g

Hinton胶囊网络后最新研究：用“在线蒸馏”训练大规模分布式神经网络

https://mp.weixin.qq.com/s/SqxooZqSeD3wA4EFK5D3Kg

再生神经网络：利用知识蒸馏收敛到更优的模型

https://zhuanlan.zhihu.com/p/51563760

知识蒸馏（Knowledge Distillation）最新进展（一）

https://zhuanlan.zhihu.com/p/53864403

知识蒸馏（Knowledge Distillation）最新进展（二）

https://mp.weixin.qq.com/s/Nkxy0SUdbwIjp5swU6tS9g

深度互学习-Deep Mutual Learning：三人行必有我师

https://mp.weixin.qq.com/s/I08kMmUohWWbYVpPDgTJsw

Startdt AI提出：使用生成对抗网络用于One-Stage目标检测的知识蒸馏方法

https://mp.weixin.qq.com/s/yFyM5OVp1YLKQBlgXeAzhw

华为诺亚方舟实验室提出无需数据网络压缩技术

https://mp.weixin.qq.com/s/a0d0b1jSm5HxHso9Lz8MSQ

小版BERT也能出奇迹：最火的预训练语言库探索小巧之路

# Integrating Learning and Planning（续）

## Model Learning

所谓Model Learning是指：从experience$$\{S_1, A_1, R_2, \dots, S_T\}$$，学习

$$S_1, A_1\to R_2,S_2$$

$$S_2, A_2\to R_3,S_3$$

$$\dots$$

$$S_{T-1}, A_{T-1}\to R_T,S_T$$

这实际上是一个监督学习的问题。其中，$$s,a\to r$$是一个回归问题(regression problem)，而$$s,a\to s'$$是一个密度估计问题(density estimation problem)。

选择一个损失函数，比如均方差，KL散度等，优化参数$$\eta$$来最小化经验损失(empirical loss)。所有监督学习相关的算法都可以用来解决上述两个问题。

根据使用的算法不同，可以有如下多种模型：查表式(Table lookup Model)、线性期望模型(Linear Expectation Model)、线性高斯模型(Linear Gaussian Model)、高斯决策模型(Gaussian Process Model)、和深信度神经网络模型(Deep Belief Network Model)等。

这里主要以查表模型来解释模型的构建。

## Table Lookup Model

查表模型适用于MDP的P，R都为已知的情况。我们通过visit得到各状态行为的转移概率和奖励，把这些数据存入表中，使用时直接检索。状态转移概率和奖励计算方法如下：

$$\hat{P}^a_{s,s'}=\frac{1}{N(s,a)}\sum_{t=1}^T 1(S_t,A_t,S_{t+1}=s,a,s')$$

$$\hat{R}^a_{s}=\frac{1}{N(s,a)}\sum_{t=1}^T 1(S_t,A_t=s,a)R_t$$

其中的$$N(s,a)$$表示state action pair$$<s,a>$$在visit中出现的次数。

这里也可以采用如下变种：

- 在每个time-step：t，记录experience tuple：$$<S_t, A_t, R_{t+1}, S_{t+1}>$$。

- 从模型采样构建虚拟经历时，从tuples中随机选择符合$$<s,a,\cdot,\cdot>$$的一个tuple。

这种方法，不再以Episode为最小学习单位，而是以时间步（time-step）为单位，一次学习一个状态转换。

## Planning with a Model

规划的过程相当于求解一个MDP的过程，即给定一个模型$$M_\eta = <P_\eta, R_\eta>$$，求解MDP$$<S, A, P_\eta, R_\eta>$$，找到基于该模型的最优价值函数或最优策略，也就是在给定的状态s下确定最优的行为a。

我们可以使用之前介绍过的价值迭代，策略迭代方法；也可以使用树搜索方法（Tree Search）。

## Sample-Based Planning

Sample-Based Planning使用Model生成采样数据，即一个time-step的虚拟经历。

有了这些虚拟采样，随后使用model-free RL方法来学习得到价值或策略函数。

## Planning with an Inaccurate Model

由于实际经历的不足或者一些无法避免的缺陷，我们通过实际经历学习得到的模型不可能是完美的模型，即：$$<P_\eta, R_\eta> = <P,R>$$

而model-based RL的性能取决于Model的准确度，如果Model不准确的话，往往只能得到一些次优解。

使用近似的模型解决MDP问题与使用价值函数或策略函数的近似表达来解决MDP问题并不冲突，他们是从不同的角度来近似求解MDP问题，有时候构建一个模型来近似求解MDP比构建一个近似的价值函数或策略函数要更加方便，这是model-based RL的可取之处。

同时由于个体经历的更新，模型处在一个动态变化的过程，当利用早期经历构建的模型后来被认为存在很大错误，不适用于当下经历的时候，需要放弃模型，转而使用model-free RL。

此外，可以在模型中，增加一些不确定性，以跳出次优解。

model-based RL适用于连续变量的状态和行为空间。

## Dyna-Q Algorithm

当构建了一个环境的模型后，个体可以有两种经历来源：

- 实际经历(real experience)。

$$S'\sim P_{s,s'}^a, R=R_s^a$$

- 模拟经历(simulated experience)。

$$S'\sim P_\eta(S' | S,A), R=R_\eta(R | S,A)$$

对于Model-Free RL来说，所有的知识都来源于real experience。

对于Model-Based RL来说，Model来源于real experience，而planning相关的知识来源于simulated experience。

于是我们很自然的想到，是否可以把real experience和simulated experience结合起来呢？

![](/images/img3/Dyna.png)

上图是Dyna算法的流程图。它从实际经历中学习得到模型，然后联合使用实际经历和模拟经历，一边学习，一边规划更新价值和（或）策略函数。

Dyna-Q算法流程如下：

>Initialize $$Q(s, a)$$ and $$Model(s, a)$$ for all $$s \in S$$ and $$a \in A(s)$$   
>Do forever:   
>>(a) $$S \leftarrow$$ current (nonterminal) state   
>>(b) $$A \leftarrow \epsilon-greedy(S, Q)$$   
>>(c) Execute action A; observe resultant reward, R, and state S'   
>>(d) $$Q(S, A) \leftarrow Q(S, A)+\alpha[R+\gamma \max_a Q(S', a)-Q(S, A)]$$   
>>(e) $$Model(S, A) \leftarrow R,S'$$(assuming deterministic environment)   
>>(f) Repeat n times:   
>>>$$S \leftarrow$$ random previously observed state   
>>>$$A \leftarrow$$ random action previously taken in S   
>>>$$R, S' \leftarrow Model(S, A)$$   
>>>$$Q(S, A) \leftarrow Q(S, A)+\alpha[R+\gamma \max_a Q(S', a)-Q(S, A)]$$

这个算法赋予了个体在与实际环境进行交互式时有一段时间用来思考的能力。其中的步骤：a,b,c,d和e都是从实际经历中学习，d过程是学习价值函数，e过程是学习模型。

在f步，给以个体一定时间（或次数）的思考。在思考环节，个体将使用模型，在之前观测过的状态空间中随机采样一个状态，同时从这个状态下曾经使用过的行为中随机选择一个行为，将两者带入模型得到新的状态和奖励，依据这个来再次更新行为价值函数。

![](/images/img3/Dyna_2.png)

上图是细化后的Dyna框架图。从上面的分析可以看出，Dyna与其说是一个算法，倒不如说是一个算法框架，两者的区别在于：框架的每个组成部分，可以采用不同的算法，只要各部分之间的关系不变，就还算是同一个框架。

![](/images/img3/Dyna_3.png)

上图比较了在Dyna算法中一次实际经历期间采取不同步数的规划时的个体表现。横坐标是Episode序号，纵坐标是特定Episode所花费的步数。可以看出，当直接使用实际经历时，个体在早期完成一个Episode需要花费较多步数，而使用5步或50步思考步数时，个体完成一个Episode与环境发生的实际交互的步数要少很多。不过到了后期，基本上就没什么差别了。

可以看出**Dyna算法可以有效提升算法的收敛速度，但不会提高最终的性能**。

![](/images/img3/Dyna_4.png)

上面的例子模拟了某时刻（虚线所示）环境发生了改变，变得更加困难，此时使用Dyna-Q算法在经历过一段时间的平台期后又找到了最优解决方案。在平台期，模型持续的给以个体原先的策略，也就是错误的策略，但个体通过与实际的交互仍然能够找到最优方案。

上二个例子表明，Dyn-Q算法赋予了个体一定的应对环境变化的能力，当环境发生一定改变时，个体一方面可以利用模型，一方面也可以通过与实际环境的交互来重新构建模型。

>上2个示例图中的Q+算法与Q的差别体现在：Q+算法鼓励探索，给以探索额外的奖励。

## Simulation-Based Search

Planning主要有两个用途：

- 改善policy或value function。这种方式通常叫做background planning。

- 直接根据当前状态选择动作。这种方式通常叫做decision-time planning。

background planning

首先回顾下前向搜索(Forward Search)算法