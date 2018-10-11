---
layout: post
title:  机器学习（八）——规则化和模型选择
category: ML 
---

## $$\mathcal{H}$$为有限集的情况（续）

如果我们定义$$h^*=\arg\min_{h\in \mathcal{H}}\varepsilon(h)$$，则根据一致收敛性质$$\lvert\varepsilon(h_i)-\hat\varepsilon(h_i)\rvert\le \gamma$$可得：

$$\varepsilon(\hat h)\le \hat\varepsilon(\hat h)+\gamma$$

因为$$\hat h$$已经是$$\hat \varepsilon(h)$$中最小的一个，因此$$\hat\varepsilon(\hat h)\le \hat\varepsilon(h)$$对所有都是成立的，其中当然包括$$h^*$$，即$$\hat\varepsilon(\hat h)\le \hat\varepsilon(h^*)$$。因此，上式可改为：

$$\varepsilon(\hat h)\le \hat\varepsilon(h^*)+\gamma$$

根据一致收敛性质，我们还可以得出$$ \hat\varepsilon(h^*)\le \varepsilon(h^*)+\gamma$$，因此，上式继续变形为：

$$\varepsilon(\hat h)\le \varepsilon(h^*)+2\gamma$$

这个公式表明，作为ERM结果的$$\hat h$$，比最好的h，至多只差$$2\gamma$$。

我们将之前的结果合到一起，可得如下定理：

令$$\lvert\mathcal{H}\rvert=k$$，并固定$$m,\delta$$的取值，且一致收敛的概率至少为$$1-\delta$$，则：

$$\varepsilon(\hat h)\le \left(\min_{h\in \mathcal{H}}\varepsilon(h)\right)+2\sqrt{\frac{1}{2m}\log\frac{2k}{\delta}}$$

假设我们需要从预测函数类$$\mathcal{H}$$切换到一个更大的预测函数类$$\mathcal{H'}\supseteq\mathcal{H}$$，则上面公式的第一项只会变得更小，也就是说偏差会变小，但由于k的增加，第二项会变大，也就是方差会变大。

同理，可得以下针对m的定理：

令$$\lvert\mathcal{H}\rvert=k$$，并固定$$\delta,\gamma$$的取值，为了保证$$\varepsilon(\hat h)\le \left(\min_{h\in \mathcal{H}}\varepsilon(h)\right)+2\gamma$$的概率至少为$$1-\delta$$，则：

$$m\ge \frac{1}{2\gamma^2}\log\frac{2k}{\delta}=O\left(\frac{1}{\gamma^2}\log\frac{k}{\delta}\right)$$

## $$\mathcal{H}$$为无限集的情况

某些预测函数的参数是实数，它实际上包含了无穷多个数。针对这样的情况，我们可以参照IEEE浮点数的规则，进行离散采样。

IEEE浮点数由64bit的二进制数构成，因此d个实数参数组成的$$\mathcal{H}$$，可组成$$k=2^{64d}$$个不同的预测函数，因此：

$$m\ge O\left(\frac{1}{\gamma^2}\log\frac{2^{64d}}{\delta}\right)=O\left(\frac{d}{\gamma^2}\log\frac{1}{\delta}\right)=O_{\gamma,\delta}(d)$$

这里的下标$$\gamma,\delta$$表示一些依赖于它们的常量。从上式可以看出需要的训练样本的数量和预测模型的参数个数成线性关系。

以上就是和ERM相关的算法的理论知识，至于其他非ERM算法理论仍在研究之中。

下面是$$\mathcal{H}$$参数化的问题。一个线性分类器可以写为：

$$h_\theta(x)=1\{\theta_0+\theta_1x_1+\dots+\theta_nx_n\ge 0\}$$

这种形式有$$n+1$$个参数。

但它也可以写为：

$$h_{u,v}(x)=1\{(u_0^2-v_0^2)+(u_0^2-v_0^2)x_1+\dots+(u_n^2-v_n^2)x_n\ge 0\}$$

这种形式有$$2n+2$$个参数。

显然这两种形式在数学上是等价的，但参数的个数却不同。为此我们引入Vapnik-Chervonenkis维度（简称VC维度），用以刻画参数的个数。

![](/images/article/VC.png)

如上图所示，3个样本点有以上几种分布方式。毫无疑问，它们都能被$$h_\theta(x)=1\{\theta_0+\theta_1x_1+\theta_2x_2\ge 0\}$$所分割，且它们的训练误差为0。但如果是4个点的话，就不能无训练误差的分割了。我们将这种最大分割的个数称作VC维度，这里$$VC(\mathcal{H})=3$$。

需要注意的是，VC维度表征的是模型的最大切割能力，而不是针对所有的情况都成立。比如下图所示的三个点，就不可以被$$h_\theta(x)=1\{\theta_0+\theta_1x_1+\theta_2x_2\ge 0\}$$所分割，但这并不影响$$h_\theta(x)$$的VC维度值。

![](/images/article/VC_2.png)

如果模型能切割任意大小的样本集，则$$VC(\mathcal{H})=\infty$$。

我们将VC维度值替换$$\mathcal{H}$$为有限集时的$$\lvert\mathcal{H}\rvert$$，可得以下相关结论：

令$$VC(\mathcal{H})=d$$，则：

$$\lvert\varepsilon(h)-\hat\varepsilon(h)\rvert\le O\left(\sqrt{\frac{d}{m}\log\frac{m}{d}+\frac{1}{m}\log\frac{1}{\delta}}\right)$$

$$\varepsilon(\hat h)\le \varepsilon(h^*)+O\left(\sqrt{\frac{d}{m}\log\frac{m}{d}+\frac{1}{m}\log\frac{1}{\delta}}\right)$$

$$m=O_{\gamma,\delta}(d)$$

以上公式的其他条件，与$$\mathcal{H}$$为有限集时相同，这里不再赘述。

# 规则化和模型选择

对于多项回归模型$$h_\theta(x)=g(\theta_0+\theta_1x_1+\dots+\theta_kx_k)$$来说，如何选择合适的k值呢？

或者，我们是选择局部权重回归（locally weighted regression），还是SVM呢？

我们定义算法模型的集合为$$\mathcal{M}=\{M_1,\dots,M_d\}$$。其中的$$M_i$$为不同的算法模型，比如SVM、神经网络等等。

## 交叉验证

回想之前讨论的过拟合和ERM算法，如果我们针对多项回归模型使用ERM算法，几乎必然会选择高方差的高维多项回归模型，因为它的训练误差最小。但这显然不是个好选择。

因此，我们改进算法如下：

>1.从全部的训练数据S中随机选择70%的样例作为训练集$$S_{train}$$，剩余的30%作为测试集$$S_{CV}$$。   
>2.在$$S_{train}$$上训练每一个$$M_i$$，得到预测函数$$h_i$$。   
>3.在$$S_{CV}$$上测试每一个$$h_i$$，得到相应的经验误差$$\hat\varepsilon_{S_{CV}}(h_i)$$。   
>4.选择具有最小$$\hat\varepsilon_{S_{CV}}(h_i)$$的$$h_i$$，作为最佳模型。

这种方法被称为hold-out交叉验证（cross validation），或者称为简单（simple）交叉验证。

由于$$S_{train}$$和$$S_{CV}$$是随机选取的，因此我们可以认为这里的经验误差$$\hat\varepsilon_{S_{CV}}(h_i)$$是$$h_i$$的泛化误差的一个很好的估计值。测试集一般占所有样本数的1/4~1/3，这里的30%是一个典型值。

还可以对模型作改进，当选出最佳的模型$$M_i$$后，再在全部数据S上做一次训练，显然训练数据越多，模型参数越准确。

简单交叉验证方法的缺点在于得到的最佳模型是在70%的训练数据上选出来的，不代表在全部训练数据上是最佳的。还有当训练数据本来就很少时，再分出测试集后，训练数据就太少了。

我们对简单交叉验证方法再做一次改进，如下：

>1.将全部训练集S分成k个不相交的子集，假设S中的训练样例个数为m，那么每一个子集有m/k个训练样例，相应的子集称作$$\{S_1,\dots,S_k\}$$。   
>2.每次从模型集合$$\mathcal{M}$$中拿出来一个$$M_i$$，然后在S中选择出k-1个子集$$S_1\cup\dots\cup S_{j-1}\cup S_{j+1}\cup\dots\cup S_k$$，在这个集合上训练$$M_i$$得到预测函数$$h_{ij}$$。在$$S_j$$上测试$$h_{ij}$$，得到相应的经验误差$$\hat\varepsilon_{S_j}(h_{ij})$$。   
>3.使用$$\frac{1}{k}\sum_{j=1}^k\hat\varepsilon_{S_j}(h_{ij})$$作为$$M_i$$泛化误差的估计值。   
>4.选出泛化误差估计值最小的$$M_i$$，在S上重新训练，得到最终的预测函数$$h_i$$。

这个方法被称为k-折叠（k-fold）交叉验证。一般来说k取值为10，这样训练数据稀疏时，基本上也能进行训练，缺点是训练和测试次数过多。

更极端的，如果$$k=m$$，则该方法又被称为leave-one-out交叉验证。

## 特征选择

特征选择（Feature Selection）严格来说也是模型选择中的一种。

假设我们想对维度为n的样本点进行回归，如果，n远远大于训练样例数m，且你认为其中只有很少的特征起关键作用的话，就可以对整个特征集进行特征选择，以减少特征的数量。

对于n个特征的$$\mathcal{M}$$来说，根据特征是否包含在最终结果中，可以写出$$2^n$$个不同的$$M_i$$。直接使用上面的交叉验证方法，计算量过大。这时可以采用如下启发式算法：

>1.初始化特征集$$\mathcal{F}=\emptyset$$。   
>2.Repeat {   
>>(a)for 特征i=1 to n, {   
>>>如果$$i\notin\mathcal{F}$$，则$$\mathcal{F}_i=\mathcal{F}\cup\{i\}$$。   
>>>在$$\mathcal{F}_i$$上使用交叉验证方法评估它的泛化误差。   
>>   
>>}   
>>(b)将第(a)步中最优的$$\mathcal{F}_i$$设为新的$$\mathcal{F}$$。   
>
>}   
>3.选择并输出搜索过程中得到的最优子集。

这个算法被称为前向搜索（forward search）。其外部循环的终止条件为$$\lvert\mathcal{F}\rvert$$达到n或者事先设定的门限值。

前向搜索属于wrapper model特征选择方法的一种。 Wrapper这里指不断地使用不同的特征子集来测试学习的算法。

除了前向搜索之外，还有后向搜索（backward search）算法。它和前者的区别在于，它的初始集合为全集，然后每次删除一个特征，并评价，直到$$\lvert\mathcal{F}\rvert$$达到阈值或者为空，然后选择最佳的$$\mathcal{F}$$即可。

可以看出无论前向搜索，还是后向搜索，其算法复杂度都是$$O(n^2)$$。

## KL散度

KL散度（Kullback–Leibler divergence）是两个随机分布间距离的度量。其定义如下：

$$D_{KL}(P\|Q)=\sum_iP(i)\log\frac{P(i)}{Q(i)}$$

其中，P和Q是离散概率分布，$$P(i)$$和$$Q(i)$$是相应分布的概率密度函数。如果P和Q是连续随机变量的话，将上式中的累加符号换成积分符号即可。

**KL散度越小，两个分布之间的匹配就越好。**

但KL散度并不是真正的度量（metric）。它既不满足三角不等式(两边之和$$\ge$$第三边)，也不满足对称性（即$$D_{KL}(P\|Q)\neq D_{KL}(Q\|P)$$）。

>注：Solomon Kullback，1907～1994，美国数学家和密码学家。乔治·华盛顿大学博士。NSA首任首席科学家。二战期间，参与破解德国的Enigma机器。

>Richard Leibler，1914～2003，美国数学家和密码学家。伊利诺伊大学博士。NSA高级主管，入选NSA名人堂。

参考：

https://mp.weixin.qq.com/s/e7YQfG578vWxApD-4gsWIg

如何理解KL散度的不对称性

https://mp.weixin.qq.com/s/PPz5iKY5Kb0vGqlfHvCJHA

初学机器学习：直观解读KL散度的数学概念

## 过滤特征选择方法

过滤特征选择（Filter feature selection）方法，是另一种启发式的特征选择算法，计算量比较小。它的思路是计算特征$$x_i$$和类别标签y之间的相关度的评分$$S(i)$$。

可以使用$$x_i$$和y之间的互信息量（mutual information），作为评分依据。

$$MI(x_i,y)=\sum_{x_i\in X_i}\sum_{y\in Y}p(x_i,y)\log\frac{p(x_i,y)}{p(x_i)p(y)}$$

其中，$$p(x_i,y)$$是$$x_i$$和y的联合概率密度，$$p(x_i)$$和$$p(y)$$是相应的边缘概率密度。

和KL散度类似，如果x和y是连续随机变量的话，将上式中的累加符号换成积分符号即可。

MI也可以用KL散度来表示：

$$MI(x_i,y)=KL(p(x_i,y)\|p(x_i)p(y))$$

过滤特征选择方法的算法复杂度为$$O(n)$$。

最后一个问题，选择多少个特征合适呢？按照$$S(i)$$从高到低的顺序，依次选择1到n个特征进行交叉验证，直到效果达到预期为止。
