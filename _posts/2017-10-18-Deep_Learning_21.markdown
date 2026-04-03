---
layout: post
title:  深度学习（二十一）——Ultra Deep Network（2）
category: DL 
---

* toc
{:toc}

# Ultra Deep Network

## mHC（续）

超连接（Hyper-Connections）采取了不同的方法。它不再是单一流，而是扩展到n条并行流，并带有可学习的混合矩阵：

$$x_{l+1} = H^{res}_l x_l + H^{post,T}_l F(H^{pre}_l x_l, W_l)$$

![](/images/img6/mHC.gif)

但问题是什么？那些混合矩阵是不受约束的。它们不仅能路由信号，还能放大信号。

DeepSeek的修复方案很干净：使用Sinkhorn-Knopp算法将混合矩阵约束为双重随机（doubly stochastic）矩阵。

一个双重随机矩阵具有以下特性：

- 所有条目非负。
- 行之和为1。
- 列之和为1。

https://taylorkolasinski.com/notes/mhc-reproduction/

DeepSeek's mHC: When Residual Connections Explode

https://taylorkolasinski.com/notes/mhc-reproduction-part2/

10,924x: The Instability Bomb at 1.7B Scale

## Attention Residuals

![](/images/img6/attn_res.png)

Residuals本身已经是一个非常强的Baseline，它对应于所有状态向量的等权求和，任何新设计想要超越它，那么至少在形式上要能够覆盖它。

Residuals：

$$
\boldsymbol{A}=\left(\begin{array}{c} 
1 \\ 
1 & 1 \\ 
1 & 1 & 1 \\ 
1 & 1 & 1 & 1 \\ 
1 & 1 & 1 & 1 & 1 \\ 
1 & 1 & 1 & 1 & 1 & 1 \\ 
1 & 1 & 1 & 1 & 1 & 1 & 1 \\ 
\end{array}\right)
$$

HC/mHC：

$$
\boldsymbol{A}=\left(\begin{array}{c} 
\boldsymbol{H}_1^{pre} \boldsymbol{H}_0^{post} \\ 
\boldsymbol{H}_2^{pre}\boldsymbol{H}_{1\leftarrow 1}^{res}\boldsymbol{H}_0^{post} & \boldsymbol{H}_2^{pre}\boldsymbol{H}_1^{post} \\ 
\boldsymbol{H}_3^{pre}\boldsymbol{H}_{2\leftarrow 1}^{res}\boldsymbol{H}_0^{post} & \boldsymbol{H}_3^{pre}\boldsymbol{H}_{2\leftarrow 2}^{res}\boldsymbol{H}_1^{post} & \boldsymbol{H}_3^{pre}\boldsymbol{H}_2^{post} \\ 
\boldsymbol{H}_4^{pre}\boldsymbol{H}_{3\leftarrow 1}^{res}\boldsymbol{H}_0^{post} & \boldsymbol{H}_4^{pre}\boldsymbol{H}_{3\leftarrow 2}^{res}\boldsymbol{H}_1^{post} & \boldsymbol{H}_4^{pre}\boldsymbol{H}_{3\leftarrow 3}^{res}\boldsymbol{H}_2^{post} & \boldsymbol{H}_4^{pre}\boldsymbol{H}_3^{post} \\ 
\boldsymbol{H}_5^{pre}\boldsymbol{H}_{4\leftarrow 1}^{res}\boldsymbol{H}_0^{post} & \boldsymbol{H}_5^{pre}\boldsymbol{H}_{4\leftarrow 2}^{res}\boldsymbol{H}_1^{post} & \boldsymbol{H}_5^{pre}\boldsymbol{H}_{4\leftarrow 3}^{res}\boldsymbol{H}_2^{post} & \boldsymbol{H}_5^{pre}\boldsymbol{H}_{4\leftarrow 4}^{res}\boldsymbol{H}_3^{post} & \boldsymbol{H}_5^{pre}\boldsymbol{H}_4^{post} \\ 
\boldsymbol{H}_6^{pre}\boldsymbol{H}_{5\leftarrow 1}^{res}\boldsymbol{H}_0^{post} & \boldsymbol{H}_6^{pre}\boldsymbol{H}_{5\leftarrow 2}^{res}\boldsymbol{H}_1^{post} & \boldsymbol{H}_6^{pre}\boldsymbol{H}_{5\leftarrow 3}^{res}\boldsymbol{H}_2^{post} & \boldsymbol{H}_6^{pre}\boldsymbol{H}_{5\leftarrow 4}^{res}\boldsymbol{H}_3^{post} & \boldsymbol{H}_6^{pre}\boldsymbol{H}_{5\leftarrow 4}^{res}\boldsymbol{H}_4^{post} & \boldsymbol{H}_6^{pre}\boldsymbol{H}_5^{post} \\ 
\boldsymbol{H}_7^{pre}\boldsymbol{H}_{6\leftarrow 1}^{res}\boldsymbol{H}_0^{post} & \boldsymbol{H}_7^{pre}\boldsymbol{H}_{6\leftarrow 2}^{res}\boldsymbol{H}_1^{post} & \boldsymbol{H}_7^{pre}\boldsymbol{H}_{6\leftarrow 3}^{res}\boldsymbol{H}_2^{post} & \boldsymbol{H}_7^{pre}\boldsymbol{H}_{6\leftarrow 4}^{res}\boldsymbol{H}_3^{post} & \boldsymbol{H}_7^{pre}\boldsymbol{H}_{6\leftarrow 5}^{res}\boldsymbol{H}_4^{post} & \boldsymbol{H}_7^{pre}\boldsymbol{H}_{6\leftarrow 6}^{res}\boldsymbol{H}_5^{post} & \boldsymbol{H}_7^{pre}\boldsymbol{H}_6^{post} \\ 
\end{array}\right)
$$

Full AttnRes：

$$
\boldsymbol{A}=\left(\begin{array}{c} 
\phi(\boldsymbol{w}_1, \boldsymbol{y}_0) \\ 
\phi(\boldsymbol{w}_2, \boldsymbol{y}_0) & \phi(\boldsymbol{w}_2, \boldsymbol{y}_1) \\ 
\phi(\boldsymbol{w}_3, \boldsymbol{y}_0) & \phi(\boldsymbol{w}_3, \boldsymbol{y}_1) & \phi(\boldsymbol{w}_3, \boldsymbol{y}_2) \\ 
\phi(\boldsymbol{w}_4, \boldsymbol{y}_0) & \phi(\boldsymbol{w}_4, \boldsymbol{y}_1) & \phi(\boldsymbol{w}_4, \boldsymbol{y}_2) & \phi(\boldsymbol{w}_4, \boldsymbol{y}_3) \\ 
\phi(\boldsymbol{w}_5, \boldsymbol{y}_0) & \phi(\boldsymbol{w}_5, \boldsymbol{y}_1) & \phi(\boldsymbol{w}_5, \boldsymbol{y}_2) & \phi(\boldsymbol{w}_5, \boldsymbol{y}_3) & \phi(\boldsymbol{w}_5, \boldsymbol{y}_4) \\ 
\phi(\boldsymbol{w}_6, \boldsymbol{y}_0) & \phi(\boldsymbol{w}_6, \boldsymbol{y}_1) & \phi(\boldsymbol{w}_6, \boldsymbol{y}_2) & \phi(\boldsymbol{w}_6, \boldsymbol{y}_3) & \phi(\boldsymbol{w}_6, \boldsymbol{y}_4) & \phi(\boldsymbol{w}_6, \boldsymbol{y}_5) \\ 
\phi(\boldsymbol{w}_7, \boldsymbol{y}_0) & \phi(\boldsymbol{w}_7, \boldsymbol{y}_1) & \phi(\boldsymbol{w}_7, \boldsymbol{y}_2) & \phi(\boldsymbol{w}_7, \boldsymbol{y}_3) & \phi(\boldsymbol{w}_7, \boldsymbol{y}_4) & \phi(\boldsymbol{w}_7, \boldsymbol{y}_5) & \phi(\boldsymbol{w}_7, \boldsymbol{y}_6) \\ 
\end{array}\right)
$$

Block AttnRes：

$$
\boldsymbol{A}=\left(\begin{array}{c:ccc:ccc} 
\phi(\boldsymbol{w}_1, \boldsymbol{y}_0) \\ 
\hdashline 
\phi(\boldsymbol{w}_2, \boldsymbol{y}_0) & \phi(\boldsymbol{w}_2, \boldsymbol{y}_1) \\ 
\phi(\boldsymbol{w}_3, \boldsymbol{y}_0) & \phi(\boldsymbol{w}_3, \boldsymbol{y}_{1:2}) & \phi(\boldsymbol{w}_3, \boldsymbol{y}_{1:2}) \\ 
\phi(\boldsymbol{w}_4, \boldsymbol{y}_0) & \phi(\boldsymbol{w}_4, \boldsymbol{y}_{1:3}) & \phi(\boldsymbol{w}_4, \boldsymbol{y}_{1:3}) & \phi(\boldsymbol{w}_4, \boldsymbol{y}_{1:3}) \\ 
\hdashline 
\phi(\boldsymbol{w}_5, \boldsymbol{y}_0) & \phi(\boldsymbol{w}_5, \boldsymbol{y}_{1:3}) & \phi(\boldsymbol{w}_5, \boldsymbol{y}_{1:3}) & \phi(\boldsymbol{w}_5, \boldsymbol{y}_{1:3}) & \phi(\boldsymbol{w}_5, \boldsymbol{y}_4)\\ 
\phi(\boldsymbol{w}_6, \boldsymbol{y}_0) & \phi(\boldsymbol{w}_6, \boldsymbol{y}_{1:3}) & \phi(\boldsymbol{w}_6, \boldsymbol{y}_{1:3}) & \phi(\boldsymbol{w}_6, \boldsymbol{y}_{1:3}) & \phi(\boldsymbol{w}_6, \boldsymbol{y}_{4:5}) & \phi(\boldsymbol{w}_6, \boldsymbol{y}_{4:5}) \\ 
\phi(\boldsymbol{w}_7, \boldsymbol{y}_0) & \phi(\boldsymbol{w}_7, \boldsymbol{y}_{1:3}) & \phi(\boldsymbol{w}_7, \boldsymbol{y}_{1:3}) & \phi(\boldsymbol{w}_7, \boldsymbol{y}_{1:3}) & \phi(\boldsymbol{w}_7, \boldsymbol{y}_{4:6}) & \phi(\boldsymbol{w}_7, \boldsymbol{y}_{4:6}) & \phi(\boldsymbol{w}_7, \boldsymbol{y}_{4:6}) \\ 
\end{array}\right)
$$


https://kexue.fm/archives/11664

Attention Residuals回忆录

https://www.zhihu.com/question/2016993095078684011

Kimi新发布的“注意力残差”有什么亮点？
