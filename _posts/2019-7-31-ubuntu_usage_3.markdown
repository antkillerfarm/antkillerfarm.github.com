---
layout: post
title:  Ubuntu使用技巧（三）
category: linux 
---

# Ubuntu 18.04使用手记

又是两年过去了，这次是Ubuntu 18.04（2018.4.26发布）。

这次的变化还是有点大，Ubuntu舍弃了自己开发的Unity，转回Gnome，连带着好多软件的界面都出现了一定的调整。这个适应过程，要长于之前的几次升级。

>前些年由于Unity界面乏善可陈，Ubuntu的版本升级被吐槽为换壁纸。这次算是换主题吧。

由于这个改变是2017.4做出的，有了1年的过渡期，因此拿到手的Ubuntu 18.04的成品度还是蛮高的。

输入法比原来好，但有些软件存在兼容问题。

内核：4.15

LibreOffice：6.0

Emacs：25.2

# 桌面主题

用腻了系统自带的桌面主题之后，我打算换个新鲜一些的桌面主题，比如Mac OS X风格的。

1.安装主题修改工具

`sudo apt-get install unity-tweak-tool`

2.安装Mac OS X主题

{% highlight bash %}
sudo add-apt-repository ppa:noobslab/themes
sudo apt-get update
sudo apt-get install mac-ithemes-v3 mac-icons-v3
{% endhighlight %}

3.Cairo Dock

做完上面两步之后，基本的Mac OS X风格已经有了，但Mac最经典的Dock启动器还没有。这里介绍一下Cairo Dock。

安装方法：

`sudo apt-get install cairo-dock`

Cairo Dock不仅具有类似Mac OS X的风格，还有其他的风格可供选择下载。比如我使用的是Chrome风格。

4.其他主题

http://www.ubuntuthemes.org/

这个网站收集了很多桌面主题，但是需要注册，因为有些主题是收费的。

# 发行版乱战

Linux以发行版众多闻名于世。最近发现了以下网站，或可对各个发行版进行一个简单的比较。

http://distrowatch.com/

下面对几个主要的参数，进行一下点评：

## Office

主要是3个流派：

1.StarOffice->OpenOffice.org->LibreOffice。最初由Sun主导，后来改为Google主导。

2.KOffice->Calligra Office。KDE项目的成果。

3.GOffice。Gnome项目的成果，和前两个相比，GOffice的组件比较独立，没有什么协同能力。

# 常用英语缩写

FYI：for your information

IFF：if and only if

eta：estimated time of arrival

w/o：without

N.B.:nota bene 注意,留心

# Integrating Learning and Planning

## Simulation-Based Search（续）

这种算法以当前状态$$S_t$$作为根节点构建了一个搜索树（Search Tree），使用MDP模型进行前向搜索。前向搜索不需要解决整个MDP，而仅仅需要构建一个从当前状态开始与眼前的未来相关的次级（子）MDP。这相当于一个登山运动员的登山问题，在某一个时刻，他只需要关注当前位置（状态）时应该采取什么样的行为才能更有利于登山（或者下撤），而不需要考虑第二天中饭该吃什么，那是登顶并安全撤退后才要考虑的事情。

从中可以看出，background planning相当于是**广度优先遍历**，虽然考虑的比较全面，但决策的深度较浅，而decision-time planning虽然只考虑当前状态的情况，但决策的深度较深（通常是一个完整的episode），相当于是**深度优先遍历**。

sample-based planning+Forward search就得到了Simulation-Based Search。

首先，从Model中采样，形成从当前状态开始到终止状态的一系列Episode：

$$\{s_t^k, A_t^k, R_{t+1}^k, \dots, S_T^k\}_{k=1}^K\sim M_v$$

有了这些Episode资源，我们可以使用Model-Free的强化学习方法，如果我们使用Monte-Carlo control，那么这个算法可以称作蒙特卡罗搜索(Monte-Carlo Search)，如果使用Sarsa方法，则称为TD搜索(TD Search)。

## Monte-Carlo Search

我们首先看一下Simple Monte-Carlo Search的步骤：

- 给定一个模型M和一个模拟的策略$$\pi$$。

- 针对行为空间里的每一个行为$$a\in A$$：

从这个当前状态$$s_t$$开始模拟出K个Episodes：

$$\{s_t^k, a, R_{t+1}^k, S_{t+1}^k, A_{t+1}^k,\dots, S_T^k\}_{k=1}^K\sim M_v,\pi$$

使用平均收获（蒙特卡罗评估）来评估当前行为a的行为价值：

$$Q(s_t, a)=\frac{1}{K}\sum_{k=1}^KG_t\xrightarrow[]{P} q_\pi(s_t, a)$$

- 选择一个有最大Q值的行为作为实际要采取的行为：

$$a_t = \mathop{\arg\max}_{a\in A}Q(s_t, a)$$

Simple Monte-Carlo Search的主要问题在于模拟出的Episodes的质量依赖于Model的质量，如果Model不准确，就会导致一系列的偏差，最终会导致结果的不准确。

我们可以采用优化领域常用的Evaluate/Improve方法来改进Monte-Carlo Search，于是就得到了Monte-Carlo Tree Search（MCTS）方法。

这里先看一下MCTS的Evaluation阶段的流程：

- 给定一个模型M。

- 从当前状态$$s_t$$开始，使用当前的模拟策略$$\pi$$,模拟生成K个Episodes：

$$\{s_t^k, a, R_{t+1}^k, S_{t+1}^k, A_{t+1}^k,\dots, S_T^k\}_{k=1}^K\sim M_v,\pi$$

- 构建一个包括所有个体经历过的状态和行为的**搜索树**。

- 对于搜索树种每一个状态行为对$$(s,a)$$，计算从该$$(s,a)$$开始的所有完整Episode收获的平均值，以此来估算该状态行为对的价值$$Q(s,a)$$:

$$Q(s_t, a)=\frac{1}{N(s,a)}\sum_{k=1}^K \sum_{u=t}^T 1(S_u,A_u=s,a) G_u\xrightarrow[]{P} q_\pi(s, a)$$

- 选择一个有最大Q值的行为作为实际要采取的行为：

$$a_t = \mathop{\arg\max}_{a\in A}Q(s_t, a)$$

MCTS的一个特点是在构建搜索树的过程中，更新了搜索树内状态行为对的价值，积累了丰富的信息，利用这些信息可以更新模拟策略，使得模拟策略得到改进。换句话说，MCTS的策略不是一成不变的。

MCTS的Simulation也是有讲究的。

首先，改进策略时要分情况对待：

- 树内（in-tree）确定性策略（Tree Policy）：对于在搜索树中存在的状态行为对，策略的更新倾向于最大化Q值，这部分策略随着模拟的进行是可以得到持续改进的。

- 树外（out-of-tree）默认策略（Default Policy）：对于搜索树中不包括的状态，可以使用固定的随机策略。

随着不断地重复模拟，状态行为对的价值将得到持续地得到评估。同时基于$$\epsilon-greedy(Q)$$的搜索策略使得搜索树不断的扩展，策略也将不断得到改善。

下面我们结合实例（下围棋）和示意图，来实际了解MCTS的运作过程。

![](/images/img3/MCTS.png)

**第一次迭代**：五角形表示的状态是个体第一次访问的状态，也是第一次被录入搜索树的状态。我们构建搜索树：将当前状态录入搜索树中。使用基于蒙特卡罗树搜索的策略（两个阶段），由于当前搜索树中只有当前状态，全程使用的应该是一个搜索第二阶段的默认随机策略，基于该策略产生一个直到终止状态的完整Episode。图中那些菱形表示中间状态和方格表示的终止状态，在此次迭代过程中并不录入搜索树。终止状态方框内的数字1表示（黑方）在博弈中取得了胜利。此时我们就可以更新搜索树种五角形的状态价值，以分数1/1表示从当前五角形状态开始模拟了一个Episode，其中获胜了1个Episode。

![](/images/img3/MCTS_2.png)

**第二次迭代**：当前状态仍然是树内的圆形图标指示的状态，从该状态开始决定下一步动作。根据目前已经访问过的状态构建搜索树，依据模拟策略产生一个行为模拟进入白色五角形表示的状态，并将该状态录入搜索树，随后继续该次模拟的对弈直到Episode结束，结果显示黑方失败，因此我们可以更新新加入搜索树的五角形节点的价值为0/1，而搜索树种的圆形节点代表的当前状态其价值估计为1/2，表示进行了2次模拟对弈，赢得了1次，输了1次。

经过前两次的迭代，当位于当前状态（黑色圆形节点）时，当前策略会认为选择某行为进入上图中白色五角形节点状态对黑方不利，策略将得到更新：当前状态时会个体会尝试选择其它行为。

![](/images/img3/MCTS_3.png)

**第三次迭代**：假设选择了一个行为进入白色五角形节点状态，将该节点录入搜索树，模拟一次完整的Episode，结果显示黑方获胜，此时更新新录入节点的状态价值为1/1，同时更新其上级节点的状态价值，这里需要更新当前状态的节点价值为2/3，表明在当前状态下已经模拟了3次对弈，黑方获胜2次。

随着迭代次数的增加，在搜索树里录入的节点开始增多，树内每一个节点代表的状态其价值数据也越来越丰富。在搜索树内依据$$\epsilon$$-greedy策略会使得当个体出于当前状态（圆形节点）时更容易做出到达图中五角形节点代表的状态的行为。

![](/images/img3/MCTS_4.png)

**第四次迭代**：当个体位于当前（圆形节点）状态时，树内策略使其更容易进入左侧的蓝色圆形节点代表的状态，此时录入一个新的节点（五角形节点），模拟完Episode提示黑方失败，更新该节点以及其父节点的状态价值。

![](/images/img3/MCTS_5.png)

**第五次迭代**：更新后的策略使得个体在当前状态时仍然有较大几率进入其左侧圆形节点表示的状态，在该节点，个体避免了进入刚才失败的那次节点，录入了一个新节点，基于模拟策略完成一个完整Episode，黑方获得了胜利，同样的更新搜索树内相关节点代表的状态价值。

如此反复，随着迭代次数的增加，当个体处于当前状态时，其搜索树将越来越深，那些能够引导个体获胜的搜索树内的节点将会被充分的探索，其节点代表的状态价值也越来越有说服力；同时个体也忽略了那些结果不好的一些节点（上图中当前状态右下方价值估计为0/1的节点）。需要注意的是，仍然要对这部分节点进行一定程度的探索，以确保这些节点不会被完全忽视。

MCTS的优点：

- 蒙特卡罗树搜索是具有高度选择性的（Highly selective)、导致越好结果的行为越被优先选择（best-first）的一种搜索方法。

- 它可以动态的评估各状态的价值，这种动态更新价值的方法与动态规划不同，后者聚焦于整个状态空间，而蒙特卡罗树搜索是立足于当前状态，动态更新与该状态相关的状态价值。

- 使用采样避免了维度灾难；同样由于它仅依靠采样，因此适用于那些“黑盒”模型（black-box models）。

- MCTS是可以高效计算的、不受时间限制以及可以并行处理的。
