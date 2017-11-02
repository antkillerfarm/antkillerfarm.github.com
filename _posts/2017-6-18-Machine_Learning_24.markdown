---
layout: post
title:  机器学习（二十四）——Tri-training, Beam Search, NLP机器翻译常用评价度量, 数据不平衡问题
category: ML 
---

# Tri-training（续）

### 纯半监督学习和推断学习

纯半监督学习和推断学习，都属于广义上的半监督学习。和主动学习不同，半监督学习无需专家提供的外部信息。

但标记数据总归不能无中生有，半监督学习的实现有赖于若干假设，其中主要有聚类假设和流形假设两种。其本质都是“相似的样本拥有相似的输出”。

纯半监督学习假定未标记数据为训练样本集，而推断学习则认为未标记数据为测试样本集。

虽然多数情况下，半监督学习能有效提升模型的泛化性能，然而这并不是绝对的。当半监督学习之后，模型的泛化性能反而下降时，我们首先需要检查数据是否满足算法所依赖的假设。

参考：

http://www.cnblogs.com/chaosimple/p/3147974.html

半监督学习

## 协同训练算法

协同训练（co-training）算法是一种多学习器的半监督学习算法。它是多视图（multi-view）学习的代表。

以电影为例，它拥有多个属性集：图像、声音、字幕等。每个属性集就构成了一个视图。

协同训练假设不同的视图具有“相容性”。所谓相容性是指，如果一个电影从图像判断，像是动作片，那么从声音判断，也有很大可能是动作片。

协同训练的算法流程：

1.在每个视图上，基于有标记数据，分别训练出一个分类器。

2.每个分类器挑选自己最有把握的未标记数据，赋予伪标记。

3.其他分类器利用伪标记，进一步训练模型，最终达到相互促进的目的。

理论上，如果不同视图之间具有完全相容性，则模型准确度可达到任意上限。而实际中，虽然很难满足完全相容性假设，但该算法仍能有效提升模型的准确度。

原始的协同训练算法的主要局限在于，构造视图在工程上是件很有难度的事情。后续的一些改进算法针对这点，在应用范围上做了不少改进。如基于不同学习算法、不同的数据采样、不同的参数设置的协同训练算法。

理论研究表明，只要弱学习器之间具有显著分歧（或差异），即可通过相互提供伪标记的方式提升泛化性能。因此，协同训练算法又被称为**基于分歧的算法**。

## Tri-training

2005年，周志华提出了Tri-training算法。该算法的流程如下：

1.首先对有标记示例集进行可重复取样(bootstrap sampling)以获得三个有标记训练集，然后从每个训练集产生一个分类器。

2.在协同训练过程中,各分类器所获得的新标记示例都由其余两个分类器协作提供，具体来说，如果两个分类器对同一个未标记示例的预测相同，则该示例就被认为具有较高的标记置信度，并在标记后被加入第三个分类器的有标记训练集。

参考：

http://www.cnblogs.com/liqizhou/archive/2012/05/11/2496162.html

Tri-training, 协同训练算法

## Co-Forest & Co-Trade

Co-Forest & Co-Trade是周志华在Tri-training基础上的改进算法。

参考：

http://lamda.nju.edu.cn/huangsj/dm11/files/gaoy.pdf

半监督学习中的几种协同训练算法

# Beam Search

Beam Search（集束搜索）是一种启发式图搜索算法，通常用在图的解空间比较大的情况下，为了减少搜索所占用的空间和时间，在每一步深度扩展的时候，剪掉一些质量比较差的结点，保留下一些质量较高的结点。

这样减少了空间消耗，并提高了时间效率，但缺点就是有可能存在潜在的最佳方案被丢弃，因此Beam Search算法是不完全的，一般用于解空间较大的系统中。

![](/images/article/beam_search.png)

上图是一个Beam Search的剪枝示意图。

Beam Search主要用于机器翻译、语音识别等系统。这类系统虽然从理论来说，也就是个多分类系统，然而由于分类数等于词汇数，简单的套用softmax之类的多分类方案，明显是计算量过于巨大了。

PS：中文验证码识别估计也可以采用该技术。

参见：

http://people.csail.mit.edu/srush/optbeam.pdf

Optimal Beam Search for Machine Translation

http://www.cnblogs.com/xxey/p/4277181.html

Beam Search（集束搜索/束搜索）

http://blog.csdn.net/girlhpp/article/details/19400731

束搜索算法（Andrew Jungwirth 初稿）BEAM Search

# NLP机器翻译常用评价度量

机器翻译的评价指标主要有：BLEU、NIST、Rouge、METEOR等。

参考：

http://blog.csdn.net/joshuaxx316/article/details/58696552

BLEU，ROUGE，METEOR，ROUGE-浅述自然语言处理机器翻译常用评价度量

http://blog.csdn.net/guolindonggld/article/details/56966200

机器翻译评价指标之BLEU

http://blog.csdn.net/han_xiaoyang/article/details/10118517

机器翻译评估标准介绍和计算方法

http://blog.csdn.net/lcj369387335/article/details/69845385

自动文档摘要评价方法---Edmundson和ROUGE

# 模型驱动 vs 数据驱动

最近阅读了这篇文章，深有感慨：

https://mp.weixin.qq.com/s/N7DE0kvf8THhJQwroHj4vA

成不了AI高手？因为你根本不懂数据！听听这位老教授多年心血练就的最实用统计学

>注：吴喜之教授是我国著名的统计学家，退休前在中国人民大学统计学院任统计学教授。吴教授上世纪六十年代就读于北京大学数学力学系，八十年代出国深造，在美国北卡罗来纳大学获得统计学博士学位，是改革开放之后第一批留美并获得统计学博士学位的中国学者。多年来吴教授在国内外数十所高校讲授统计学课程，在国内统计学界享有盛誉。其知名的学生有李舰和刘思喆。

>李舰，从2003年开始，一直把R当作随身武器奋战在统计学和数据分析的第一线，是Rweibo、Rwordseg、tmcn等高质量R包的作者，在业界积累了大量的经验，目前供职于Mango Solutions（中国），任数据总监。

>刘思喆，2012至2016年就职于京东商城，推荐系统平台部高级经理，主要负责和推荐系统离线、在线相关的用户行为、商品特征的建模，以及数据监控平台。因工作业绩，在《京东技术解密》一书中获“数据达人”称号。

# 数据不平衡问题

https://mp.weixin.qq.com/s/e0jXXCIhbaZz7xaCZl-YmA

如何处理不均衡数据？

https://mp.weixin.qq.com/s/2j_6hdq-MhybO_B0S7DRCA

如何解决机器学习中数据不平衡问题

https://mp.weixin.qq.com/s/gEq7opXLukWD5MVhw_buGA

七招教你处理非平衡数据

http://blog.csdn.net/u013709270/article/details/72967462

机器学习中的数据不平衡解决方案大全

# 强化学习

## 概述

强化学习是一个多学科交叉的领域。它的主要组成以及和其他学科的关系如下图所示：

![](/images/article/RL_2.png)

![](/images/article/RL.png)

上图是Reinforcement Learning和其他类型算法的关系图。

不像监督学习，对于每一个样本，都有一个确定的标签与之对应，而强化学习没有标签，只有一个时间延迟的奖励，而且游戏中我们往往牺牲当前的奖励来获取将来更大的奖励。这就是**信用分配问题（Credit Assignment Problem）**，即当前的动作要为将来获得更多的奖励负责。

而且在我们找到一个策略，让游戏获得不错的奖励时，我们是选择继续坚持当前的策略，还是探索新的策略以求更多的奖励？这就是**探索与开发（Explore-exploit Dilemma）**的问题。

![](/images/article/reinforcement_learning.png)

上图是强化学习的基本流程图。从控制论的角度来说，这是一个反馈控制系统，和经典的Kalman filters系统非常类似。因此，目前强化学习的主要用途，也多数和系统控制相关，例如机器人和自动驾驶。

在推荐系统领域，由于有用户的反馈信息，亦可使用相关强化学习算法。

## MDP

强化学习任务通常用马尔可夫决策过程（Markov Decision Process）来描述：

$$<\mathcal{S},\mathcal{A},\mathcal{P},\mathcal{R},\gamma>$$

这个五元组依次代表：states、actions、state transition probability matrix、reward function、discount factor。

>MDP中有两个对象：Agent和Environment。   
>1.Environment处于一个特定的状态（State）（如打砖块游戏中挡板的位置、各个砖块的状态等）。   
>2.Agent可以通过执行特定的动作（Actions）（如向左向右移动挡板）来改变Environment的状态。   
>3.Environment状态改变之后会返回一个观察（Observation）给Agent，同时还会得到一个奖励（Reward）（可以为负，就是惩罚）。   
>4.Agent根据返回的信息采取新的动作，如此反复下去。Agent如何选择动作叫做策略（Policy）。MDP的任务就是找到一个策略，来最大化奖励。

注意State和Observation区别：State是Environment的私有表达，我们往往不会直接得到。

在MDP中，当前状态State包含了所有历史信息，即将来只和现在有关，与过去无关，因为现在状态包含了所有历史信息。只有满足这样条件的状态才叫做马尔科夫状态（Markov state）。当然这只是理想状况，现实往往不会那么简单。

正是因为State太过于复杂，我们往往可以需要一个对Environment的观察来间接获得信息，因此就有了Observation。不过Observation是可以等于State的，此时叫做Full Observability。

状态、动作、状态转移概率组成了MDP，一个MDP周期（episode）由一个有限的状态、动作、奖励队列组成：

$$s_0,a_0,r_1,s_1,a_1,r_2,s_2,\dots,s_{n-1},a_{n-1},r_n,s_n$$

这里$$s_i$$代表状态，$$a_i$$代表行动，$$r_{i+1}$$是执行动作后的奖励。最终状态为$$s_n$$。

## 折扣未来奖励（Discounted Future Reward）

为了获得更多的奖励，我们往往不能只看当前奖励，更要看将来的奖励。

给定一个MDP周期，总的奖励显然为：

$$R=r_1+r_2+\dots+r_n$$

那么，从当前时间t开始，总的将来的奖励为：

$$R_t=r_t+r_{t+1}+\dots+r_n$$

这个过程又称为Markov Reward Process：

$$<\mathcal{S},\mathcal{P},\mathcal{R},\gamma>$$

这个四元组依次代表：states、state transition probability matrix、reward function、discount factor

