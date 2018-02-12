---
layout: post
title:  机器学习（二十八）——Monte-Carlo
category: ML 
---

# Monte-Carlo（续）

## Monte Carlo Policy Evaluation

Monte Carlo Policy Evaluation的目标是对状态s进行估值。它的步骤是：

>当s被访问到(visited)时:   
>>增加计数：$$N(s)\leftarrow N(s) + 1$$   
>>增加总奖励：$$S(s)\leftarrow S(s) + G_t$$   
>>$$V(s) = S(s)/N(s)$$   
>反复多次：$$N(s)\to \infty,V(s)\to v_{\pi}(s)$$

根据Visit的策略不同，Monte Carlo Policy Evaluation又可分为：First-visit MC和Every-Visit MC。

两者的差别在于：First-visit MC的每次探索，一旦抵达状态s，就结束了，Every-Visit MC到达状态s之后还可以继续探索。



## 参考

https://mp.weixin.qq.com/s/F9VlxVV4nXELyKxdRo9RPA

强化学习——蒙特卡洛

https://www.zhihu.com/question/20254139

蒙特卡罗算法是什么？

http://www.cnblogs.com/2010Freeze/archive/2011/09/19/2181016.html

概率算法-sherwood算法

http://www.cnblogs.com/chinazhangjie/archive/2010/11/11/1874924.html

概率算法

https://mp.weixin.qq.com/s/wfCyii6bS-GxMZPg2TPaLA

蒙特卡洛树搜索是什么？如何将其用于规划星际飞行？

$$V(S_t)\leftarrow V(S_t)+\alpha(R_{t+1}+\gamma V(S_{t+1})-V(S_t))$$

