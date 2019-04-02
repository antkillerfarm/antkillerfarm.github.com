---
layout: post
title:  机器学习（二十九）——在线学习
category: ML 
---

# 在线学习

我们之前讨论的算法，都是给定一个训练集S，经训练之后，得到预测函数h，然后再在新的样本集上进行预测。这种方法被称为批量学习（batch learning）。

与之相对的，还有一种边学习边预测的在线学习（online learning）算法。其步骤如下：

>1.$$i:=0$$。   
>2.输入$$x^{(i)}$$，算法预测$$y^{(i)}$$。   
>3.根据$$y^{(i)}$$的真实值，修正算法模型。这一步也被称作更新过程（update procedure）   
>4.令$$i:=i+1$$，以处理下一个数据样本。

在线学习的优点：
1.算法在学习过程中，即可预测。
2.随着数据样本的增多，预测会更加准确，即具有自我完善的的能力。

下面以感知器（perceptron）算法为例，讨论一下在线学习的误差问题。

首先回忆一下感知器算法：

$$h_\theta(x)=g(\theta^Tx),y\in\{1,-1\}$$

$$g(z)=\begin{cases}
1, & z\ge 0 \\
-1, & z<0 \\
\end{cases}$$

对于训练样本$$(x,y)$$，其更新过程为：

$$\theta:=\begin{cases}
\theta, & h_\theta(x)=y \\
\theta+yx, & otherwise \\
\end{cases}\tag{3}$$

这里给出一个和感知器算法有关的定理：

对于给定的m个样本**序列**（注意“序列”二字，在线算法对于样本的顺序是敏感的）$$x^{(i)}$$，如果所有的$$\|x^{(i)}\|\le D$$，并且存在单位向量u，使得所有样本的$$y^{(i)}(u^Tx^{(i)})\ge \gamma$$，则感知器算法在该序列上的预测错误个数最多为$$(D/\gamma)^2$$。

这个定理是Henry David Block和Albert B. J. Novikoff于1962年提出的。

>注：Henry David Block，1920～1978，美国数学家。   
>这个人的经历有点非典型。20岁本科毕业，由于专业是文学和心理学，结果找不到工作。只好回炉，又读了个土木工程的本科。   
>二战期间在Goodyear的飞机工厂（没错，就是那个卖轮胎的Goodyear）担任测试工程师。在那里碰到一个当医生的英国妹子，搞定之。   
>1946年，他老婆在Iowa State University获得了一个职位，于是他也跟着搬了过去。估计是无所事事，他经常到大学里蹭课，然后就发现自己对数学很感兴趣。   
>于是继续读书，1949年拿到博士学位。经过几年助教生涯之后，最终成为康奈尔大学应用数学教授。   
>话说，根据缩写找人真是太痛苦了，很多资料都不是你要找的人，Block又是个大路货。我最后是在他的一位同事的论文中找到他的全名的。那篇论文发表于1985年，距离他去世已经7年，但他仍是作者之一，可见人缘不错。

>Albert B. J. Novikoff，全名不详，只知道是纽约大学教授。从岁数来看应该已经退休了。

下面给出这个定理的证明过程：

由公式3可知，$$\theta$$的值只有在发生预测错误的时候才会改变，因此我们可以使用$$\theta^{(k)}$$表示第k个错误。同时令$$\theta^{{1}}=0$$。

根据更新公式可得：

$$(\theta^{(k+1)})^Tu=(\theta^{(k)})^Tu+y^{(i)}(x^{(i)})^Tu\ge (\theta^{(k)})^Tu+\gamma$$

所以：

$$(\theta^{(2)})^Tu\ge (\theta^{(1)})^Tu+\gamma=\gamma$$

$$(\theta^{(3)})^Tu\ge (\theta^{(2)})^Tu+\gamma\ge 2\gamma$$

由数学归纳法可得：

$$(\theta^{(k+1)})^Tu\ge k\gamma\tag{4}$$

另外，

$$\|\theta^{(k+1)}\|^2=\|\theta^{(k)}+y^{(i)}x^{(i)}\|^2=\|\theta^{(k)}\|^2+\|y^{(i)}x^{(i)}\|^2+2y^{(i)}(x^{(i)})^T\theta^{(k)}$$

由感知器算法的定义可得:

$$y^{(i)}(x^{(i)})^T\theta^{(k)}\le 0$$

所以：

$$\|\theta^{(k+1)}\|^2\le \|\theta^{(k)}\|^2+\|x^{(i)}\|^2\le \|\theta^{(k)}\|^2+D^2$$

同样，由数学归纳法可得：

$$\|\theta^{(k+1)}\|^2\le kD^2\tag{5}$$

因为：

$$(\theta^{(k+1)})^Tu=\|\theta^{(k+1)}\|\cdot\|u\|\cdot \cos\phi\le \|\theta^{(k+1)}\|\cdot\|u\|=\|\theta^{(k+1)}\|\tag{6}$$

由公式4、5、6可得：

$$\sqrt{k}D\ge \|\theta^{(k+1)}\|\ge (\theta^{(k+1)})^Tu\ge k\gamma$$

所以：

$$k\le (D/\gamma)^2$$

# TensorFlow+

https://mp.weixin.qq.com/s/kYOwUWlTP4T0IYKDWDbCsg

tensorflow object detection API训练公开数据集Oxford-IIIT Pets Dataset

https://mp.weixin.qq.com/s/8uDsaZjsiKXGea6M-w-RvA

tensorflow object detection API使用之GPU训练实现宠物识别

https://mp.weixin.qq.com/s/knw7yuUxHe-qeCLfj20onw

Bayesian GAN的TensorFlow实现

https://mp.weixin.qq.com/s/Sxui9CvdGocIxVG2FM4JtQ

基于tensorflow使用CNN-RNN进行中文文本分类！

https://mp.weixin.qq.com/s/X7ISLRMy5cddpmO5mev4MA

自创数据集，使用TensorFlow预测股票入门

https://mp.weixin.qq.com/s/kJxXIN6D5TEEFSFhGJNIyw

开源神经网络图片上色技术解析

https://mp.weixin.qq.com/s/qXMRHxDDRa-_rJZMhXWB4w

详解TensorFlow的新seq2seq模块及其用法

https://mp.weixin.qq.com/s/YdcIDXadEnDsyfc6Iu1gGw

手把手教你用TensorFlow训练模型

https://mp.weixin.qq.com/s/Off0pgaRNyik2nvjHaQQkw

在TensorFlow中对比两大生成模型：VAE与GAN

https://mp.weixin.qq.com/s/rMYjsIgFNvv47F4YZjY8SA

如何在K8S上玩转TensorFlow？

https://zhuanlan.zhihu.com/p/30751039

TensorFlow全新的数据读取方式：Dataset API入门教程

https://mp.weixin.qq.com/s/SDVQSn1aVDXk_RPGuVcQgQ

TensorFlow深度自动编码器入门实践

https://mp.weixin.qq.com/s/mZ79KAuSIJLtBEXvKwUi-w

如何利用TensorFlow.js部署简单的AI版“你画我猜”图像识别应用

https://mp.weixin.qq.com/s/atQHJ5U2pJpSG6PguN7J4Q

如何保持运动小车上的旗杆屹立不倒？TensorFlow利用A3C算法训练智能体玩CartPole游戏

https://mp.weixin.qq.com/s/yi-PNmMNMbwSi56aXo6ZSQ

tensorflow对象检测框架训练VOC数据集常见的两个问题

# 世说新语+

## 2019.3（续）

主流发达国家教职体系，PI（Principal Investigator）制：助理教授（Assistant Professor，AP）->副教授（Associate Professor）->教授（Full Professor->讲席教授（Chair Professor）

----

载旗航空公司指被政府指定独占国际航线的权利、在机身涂有国旗标志的航空公司。

----

黄志千，1914～1965，飞机设计师。上海交通大学机械系航空专业（1937），沈阳飞机设计研究所总设计师。1965年5月20日，黄志千赴西欧考察，途经开罗上空因飞机失事不幸罹难。

----

1966年1月8日夜间，吴文献、吴珍加及吴春富三名中国人民解放军士兵驾一艘登陆艇叛逃马祖。次日下午，台湾军方用专机将他们接返台湾，途中遭中国大陆军机击落，机毁人亡。

----

1996年1月8日，非洲航空一架安妥诺夫An-32客机从金萨沙机场起飞失败后滑出跑道，冲入金沙萨的大型街头市场，机上死亡人数不多，但造成地面上227人死亡，253人重伤，意外地成为了飞机失事造成非乘客死亡人数最多的事故。

----

https://mp.weixin.qq.com/s/xUtNcTDa86njqR07XBJwnA

巨石阵真的是神秘“未解之谜”吗？

笑问石阵何处有？细数欧洲三万座

----

蒋委员长下令剿共-->共谍郭汝瑰制定剿共计划-->共谍刘斐审阅剿共计划之后上报-->共谍沈安娜记录一些临时的修改并整理上报-->共谍韩练成负责保管已确定的剿共计划-->剿共计划经由几乎全是共谍的南京军话总站下达-->出兵剿共

----

https://zhuanlan.zhihu.com/p/59014246

用大数据带你了解电影行业百年发展历程

----

https://zhuanlan.zhihu.com/p/59329666

如何使用大疆无人机开展国土三调

----

https://mp.weixin.qq.com/s/5BA0NaSdHEg86nQnhYET9w

羞愧吗？孩子营养餐很难解决吗？看看这位穷乡沟小学校长！

----

一个医生旅游回来，他的儿子骄傲的对他说，爸爸，看来我的医术已经远超与你了，我仅仅用三天时间就治好了你三年治不好的病人，你可以退休了。医生叹了口气，孩子，你以为你读医学院的学费从哪里来的？

----

历史上的大理段氏，在五代后晋时期建立大理国，统治了云南地区318年（937年—1254年），随后投降蒙古军，成为蒙古汗国和元帝国的大理总管，又继续统治了今天云南中西部地区128年，于明初洪武十五年（1382年）被明军攻灭，作为我国云南地区统治者一共存在了446年。

https://www.zhihu.com/question/20666711/answer/524642062

为什么金庸先生在《天龙八部》里会把大理段氏写成一个江湖味十足的皇权，难道历史上这个政权真的很江湖么？

----

https://mp.weixin.qq.com/s/FeSKZqe394-8xT4i-BOEog

掉入地下一万米

----

https://www.zhihu.com/question/22869308/answer/623204879

为什么写论文时一定要引用论文？

----

https://996.icu/

工作996，生病ICU

----

伟主（Great Masters）、善主（Good Masters）、贤主大人（Wise Masters）。

## 2019.4

