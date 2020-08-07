---
layout: post
title:  机器学习（八）——规则化和模型选择
category: ML 
---

* toc
{:toc}

## 学习理论的预备知识（续）

对于线性分类$$h_\theta(x)=1\{\theta^Tx\ge 0\}$$来说，寻找合适的参数$$\theta$$，还有另一种方法，即最小化训练误差，并令：

$$\hat\theta=\arg\min_\theta\hat\varepsilon(h_\theta)$$

我们将这个过程称为经验风险最小化（empirical risk minimization，ERM）。其最终的预测函数为$$\hat h=h_{\hat\theta}$$。

ERM是一类基本的学习算法，也是本节关注的焦点。

我们定义预测函数类（hypothesis class ）$$\mathcal{H}$$，用以表示解决某类学习问题的所有可能的分类器的集合。（实际上也就是参数$$\theta$$所有可能取值的集合。）则ERM算法可表示为：

$$\hat h=\arg\min_{h\in \mathcal{H}}\hat\varepsilon(h)$$

参考：

http://www.jianshu.com/p/f1433317bf48

你真的理解机器学习中偏差-方差之间的权衡吗？

https://mp.weixin.qq.com/s/nRaM87Wao4XdHwltV1nh-A

思考VC维与PAC：如何理解深度神经网络中的泛化理论？

https://mp.weixin.qq.com/s/ty-V1JOzx24lLB8C0zHpaw

机器学习碎碎念：霍夫丁不等式

## $$\mathcal{H}$$为有限集的情况

根据之前的讨论，我们做如下定义：

$$Z=1\{h_i(x)\neq y\}$$

$$Z_j=1\{h_i(x^{(j)})\neq y^{(j)}\}$$

$$\hat\varepsilon(h_i)=\frac{1}{m}\sum_{j=1}^mZ_j$$

其中，$$h_i\in \mathcal{H}$$。

由Hoeffding不等式可知：

$$P(\lvert\varepsilon(h_i)-\hat\varepsilon(h_i)\rvert>\gamma)\le 2\exp(-2\gamma^2m)$$

这个公式表明：对于特定的$$h_i$$，在m很大的情况下，训练误差有很大的概率接近于泛化误差。

如果我们用$$A_i$$表示事件$$\lvert\varepsilon(h_i)-\hat\varepsilon(h_i)\rvert>\gamma$$，则上式可改为$$P(A_i)\le 2\exp(-2\gamma^2m)$$。

$$P(\exists h\in\mathcal{H}.\lvert\varepsilon(h_i)-\hat\varepsilon(h_i)\rvert>\gamma)=P(A_1\cup \dots\cup A_k)$$

$$\le \sum_{i=1}^kP(A_i)\le \sum_{i=1}^k2\exp(-2\gamma^2m)=2k\exp(-2\gamma^2m)$$

$$\begin{align}
&1-P(\exists h\in\mathcal{H}.\lvert\varepsilon(h_i)-\hat\varepsilon(h_i)\rvert>\gamma)=P(\lnot\exists h\in\mathcal{H}.\lvert\varepsilon(h_i)-\hat\varepsilon(h_i)\rvert>\gamma)
\\&=P(\forall h\in\mathcal{H}.\lvert\varepsilon(h_i)-\hat\varepsilon(h_i)\rvert\le\gamma)\ge 1-2k\exp(-2\gamma^2m)
\end{align}$$

上面的结果表明，对于所有的$$h\in \mathcal{H}$$，实际上也有一个收敛性质。这个性质被称为一致收敛（uniform convergence）。

上式变形可得：

$$m\ge \frac{1}{2\gamma^2}\log\frac{2k}{\delta}$$

其中，$$\delta=2k\exp(-2\gamma^2m)$$。

上式表明，在固定$$\gamma$$和$$\delta$$的情况下，至少需要多少训练样本，才能保证对于所有的$$h\in \mathcal{H}$$，$$P(\lvert\varepsilon(h_i)-\hat\varepsilon(h_i)\rvert\le \gamma)$$至少为$$1-\delta$$。

这里的m也被称为算法的样本复杂度（sample complexity），它表征达到一定性能所需要的训练样本的数量。

同样的，我们固定m和$$\delta$$，可得：

$$\lvert\varepsilon(h)-\hat\varepsilon(h)\rvert\le \sqrt{\frac{1}{2m}\log\frac{2k}{\delta}}$$

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

![](/images/img3/k-fold.png)

更极端的，如果$$k=m$$，则该方法又被称为leave-one-out交叉验证。

参考：

https://mp.weixin.qq.com/s/OmSxnVL6pYYzB9_jDV4Lqg

模型评估方法基础总结

https://mp.weixin.qq.com/s/lrNvC8EWcq6cT16FU8nUbQ

5种常用的交叉验证技术，保证评估模型的稳定性
