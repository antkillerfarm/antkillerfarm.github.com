---
layout: post
title: 《算法导论》研习笔记 1
category: algorithms
---

# 内容简介 #

* 算法分析基础.
* 递归算法
* 概率分析与随机化算法

## 算法分析基础 ##

为了精确衡量一段程序在计算设备中的运行性能，前人们发明了一种数学方法，即时间复杂度于空间复杂度。通常，如果一段程序或者一个算法有被衡量的价值，她必须有循环元素。因为非循环的程序段或者算法的运行时间是完全可以预料的，通常我们称作"常量时间"。一旦有了循环元素，我们分析性能的时候就有了一个神秘的变量：n，也就是问题的规模，再通俗点说就是循环的次数。

例如，《算法导论》中第二章对插入排序的分析里，我们先粗略的得到了一个变量为n的函数，所以我们对算法的分析，从某种意义上来说是对这个函数 f(n) 的分析：即当n逐渐增大，f(n) 的增长趋势。为了方便分析 f(n) 的增长趋势，先人们发明了几种基于 f(n) 的函数集合，也叫Asymptotic notation，即渐近表达式，常用的有三种：O标记，<img src="http://chart.googleapis.com/chart?cht=tx&chl=%5CTheta%20" style="border:none;" />标记，以及<img src="http://chart.googleapis.com/chart?cht=tx&chl=%5COmega%20" style="border:none;" />标记，我们一一来看：

1. O标记是我们最常见的标记，简称Big-O-notation，他定义如下：

   <img src="http://chart.googleapis.com/chart?cht=tx&chl=O%5Bg(n)%5D" style="border:none;" /> = {<img src="http://chart.googleapis.com/chart?cht=tx&chl=f(n)" style="border:none;" />: 存在正整数 c 和 n0使得 <img src="http://chart.googleapis.com/chart?cht=tx&chl=0%5Cle%20f(n)%20%5Cle%20cg(n)" style="border:none;" />对于所有的<img src="http://chart.googleapis.com/chart?cht=tx&chl=n%5Cge%20n0" style="border:none;" />都成立 }.

   从定义中可看出，O标记定义了f(n)的上界，也就是说算法真正的时间复杂度函数永远不会超过O[g(n)]，这也是O标记如此常用的原因，因为我们大多是在分析算法在最坏情况下的性能。

2. <img src="http://chart.googleapis.com/chart?cht=tx&chl=%5COmega%20%5Bg(n)%5D" style="border:none;" /> = {<img src="http://chart.googleapis.com/chart?cht=tx&chl=f(n)" style="border:none;" />: 存在正整数 c 和 n0使得<img src="http://chart.googleapis.com/chart?cht=tx&chl=0%5Cle%20cg(n)%20%5Cle%20f(n)" style="border:none;" />对于所有的<img src="http://chart.googleapis.com/chart?cht=tx&chl=n%5Cge%20n0" style="border:none;" />都成立 }.

  这个很明显是定义了性能函数的下界，通俗的说就是最好，最理想的状况。

3. <img src="http://chart.googleapis.com/chart?cht=tx&chl=%5CTheta%20%5Bg(n)%5D" style="border:none;" /> = {<img src="http://chart.googleapis.com/chart?cht=tx&chl=f(n)" style="border:none;" />:  存在正整数 c1，c2 和 n0使得<img src="http://chart.googleapis.com/chart?cht=tx&chl=0%5Cle%20c1g(n)%20%5Cle%20f(n)%20%5Cle%20c2g(n)" style="border:none;" />对于所有的<img src="http://chart.googleapis.com/chart?cht=tx&chl=n%5Cge%20n0" style="border:none;" />都成立 }.


## 递归算法 ##

\begin{equation}
	E = mc^2
\end{equation}

