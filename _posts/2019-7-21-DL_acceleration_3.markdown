---
layout: post
title:  深度加速（三）——Winograd（3）
category: DL acceleration 
---

# Winograd

## FFT与卷积

FFT是加速卷积运算的一种常用方法。但由于其原理，当卷积核小的时候，是没什么加速的，当核是3或者5时，速度有时更慢或者相当，而在CNN中卷积的核大多数比较小，FFT起到的加速作用很小，所以基本没人用。

此外，FFT是复数运算，如果没有特殊硬件，而用实数计算的话，还是比较费劲的。

![](/images/img2/Integer_multiplication_FFT.png)

说句题外话，上图是FFT进行整数乘法的示例图。对于超大整数，2维的FFT也是远远不够的。例如2019年2月，David Harvey和Van Der Hoeven就使用了1729维的FFT，计算了两个10亿位的超大整数的乘法。

参见：

http://www.cnblogs.com/jianyingzhou/p/4303578.html

convolution,fft, 加速

http://www.cnblogs.com/luoqingyu/p/5930181.html

数字信号处理--FFT与蝶形算法

http://blog.csdn.net/xxinliu/article/details/7438429

Cooley-Tukey算法 （蝶形算法）

https://www.zhihu.com/question/264307400

为什么很少人用FFT加速CNN卷积层的运算？

## Strassen algorithm

![](/images/img3/Strassen.png)

上图展示的是Strassen algorithm的步骤，该算法也是在矩阵运算加速领域比较知名的算法。

然而，实际的情况要比理论计算复杂的多。比如有的硬件支持加法/乘法的跳零（忽略对0的加法/乘法）操作，这时无论是Strassen还是Winograd都不会有太好的效果。

## 参考

https://colfaxresearch.com/falcon-library/

FALCON Library: Fast Image Convolution in Neural Networks on Intel Architecture

https://www.intelnervana.com/winograd/

"Not so fast, FFT": Winograd

https://www.encyclopediaofmath.org/index.php/Winograd_small_convolution_algorithm

Winograd small convolution algorithm

https://mp.weixin.qq.com/s/VcGwT0rKYQr13MQ_PRVmyg

每个AI程序员都应该知道的基础数论

https://mp.weixin.qq.com/s/jCPHXVI_utwnxaHqvW5nyA

商汤联合提出基于FPGA的快速Winograd算法：实现FPGA之上最优的CNN表现与能耗

https://www.leiphone.com/news/201804/SpmfJVa3al3uBDh2.html

斯坦福ICLR 2018录用论文：高效稀疏Winograd卷积神经网络

https://www.cnblogs.com/shine-lee/p/10906535.html

卷积神经网络中的Winograd快速卷积算法
