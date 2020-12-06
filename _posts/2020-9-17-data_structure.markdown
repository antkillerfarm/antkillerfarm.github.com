---
layout: post
title:  数据结构 & 普通CS算法, 设计模式
category: resource 
---

* toc
{:toc}

# 数据结构 & 普通CS算法

## TopK

https://zhuanlan.zhihu.com/p/76734219

面试官最喜爱的TopK问题算法详解

https://www.cnblogs.com/qlky/p/7512199.html

海量数据中找出前k大数（topk问题）

https://www.jianshu.com/p/5b8f00d6a9d7

topK算法问题

## 正则表达式

https://mp.weixin.qq.com/s/OtVRL37CNt_d5yEJPzzBzg

一个由正则表达式引发的血案

https://mp.weixin.qq.com/s/udEQzNk7vTltFhLyAXrsNQ

图文解读助你理解和使用正则表达式

https://zhuanlan.zhihu.com/p/27971065

关于多正则匹配

https://segmentfault.com/a/1190000007929344

Python正则表达式re模块简明笔记

https://mp.weixin.qq.com/s/Tewaynja3ggkcpzAli-1YQ

Python正则表达式

# Trie树

Trie树也称字典树，因为其效率很高，所以在在字符串查找、前缀匹配等中应用很广泛，其高效率是以空间为代价的。

下面以英文单词构建的字典树为例，这棵Trie树中每个结点包括26个孩子结点，因为总共有26个英文字母(假设单词都是小写字母组成)。

如给出字符串"abc","ab","bd","dda"，根据该字符串序列构建一棵Trie树。则构建的树如下:

![](/images/article/trie.jpg)

参考：

https://mp.weixin.qq.com/s/J7C70ASC6GZJqmqut0Z6Ag

双数组Trie树(DoubleArrayTrie)Java实现

https://mp.weixin.qq.com/s/nMmrVPvjaPOLgxKNjMzetw

动画+解析，轻松理解“Trie树”

## 排序

![](/images/img2/DS.png)

![](/images/img2/sort.png)

上面两个图的原始地址：

http://bigocheatsheet.com/

该网站还提供了其他关于算法复杂度的资料。

https://mp.weixin.qq.com/s/xttZJVR0r5vssOJ19sBHGQ

快速排序算法

https://mp.weixin.qq.com/s/jQ6fEi_K2RSesJxZn_kujg

外部排序

https://mp.weixin.qq.com/s/QAAfjSeZtfd3PSN3wAP8RQ

什么是外部排序？

https://mp.weixin.qq.com/s/Yp1hD2Bbmj3pRrLYjfwfEw

用Python手写十大经典排序算法

https://mp.weixin.qq.com/s/qLbCjTkDUHUosWQJjymqzQ

诸葛亮 vs 司马懿，排序算法大战谁能笑到最后？

https://www.cnblogs.com/tuding/p/7335853.html

双调排序Bitonic Sort，适合并行计算的排序算法

https://mp.weixin.qq.com/s/fP36LYFjAqqfP3rbYxAJlA

优秀的排序算法如何成就了伟大的机器学习技术

![](/images/img2/sort.gif)

https://mp.weixin.qq.com/s/FAp10hI05qLLZi5BypondA

除了冒泡排序，你知道Python内建的排序算法吗？

这篇blog其实和Python关系不大，它主要讲述了Timsort排序算法。

>Tim Peters，美国软件工程师。他于2002年在python上实现了Timsort排序算法。该算法后来被诸如Java、Android、GNU Octave等所采用。

早期最好的排序算法是QuickSort，比如C语言库自带的qsort函数，使用的就是该算法。然而它对于倒序数组这种最坏情况的复杂度居然是$$O(n^2)$$。

https://mp.weixin.qq.com/s/gBHmBLGILd6rZ-6cuelw0Q

这可能是你听说过最快的稳定排序算法（Timsort）

# Bloom Filter

https://blog.csdn.net/zhaodedong/article/details/78186450

Bloom Filter的原理和实现

https://blog.csdn.net/zhaodedong/article/details/78445910

Bloom Filter的数学背景

https://mp.weixin.qq.com/s/aPepcwG_VMioqGQ_Fp3deg

海量数据处理利器之布隆过滤器

## 参考

https://mp.weixin.qq.com/s/JiYRhcTv2qgLfVyGzI8uHQ

印度小哥用Python和Java实现所有AI算法

https://mp.weixin.qq.com/s/52JFdyaLwZq7nWiZq6z6qA

二叉堆是什么鬼？

https://mp.weixin.qq.com/s/4V1E2L14c3k-5i0c-JyTVQ

如何在10亿数中找出前1000大的数

https://mp.weixin.qq.com/s/yNn57Uw_-itzZXCSEAzrJw

一起搞定面试中的二叉树（一）

https://mp.weixin.qq.com/s/msv2NZ0aG96d3xQhX3HhNA

一起搞定面试中的二叉树（二）

http://blog.csdn.net/u013709270/article/details/53470428

跳跃表的原理及实现

https://mp.weixin.qq.com/s/t7Q0slX3q8Qlhg8F8pXrZQ

如何找到字符串中的最长回文子串？

https://mp.weixin.qq.com/s/d9yNsUVFg9UZN62xuOdxow

为什么MySQL数据库要用B+树存储索引？

https://mp.weixin.qq.com/s/o0JFTpGa4MLtDKHf4B2Ing

快速理解为啥这个查询使用索引，那个查询不使用索引

https://mp.weixin.qq.com/s/CtPywscoA_FvF2d9NLEenw

如何从100亿URL中找出相同的URL？

https://mp.weixin.qq.com/s/bdZ5e8CaiPuI8TpLNRrFUQ

大数据近似最近邻搜索哈希方法综述（上）

https://mp.weixin.qq.com/s/jiZw-x6EMhUIySIObm5XjA

大数据近似最近邻搜索哈希方法综述（下）

https://mp.weixin.qq.com/s/jeQawOIomUAjIp7GhuBk3A

一致性哈希算法及其在分布式系统中的应用

https://blog.csdn.net/cywosp/article/details/23397179

五分钟理解一致性哈希算法(consistent hashing)

https://www.cnblogs.com/BCOI/p/9072444.html

TREAP

https://blog.csdn.net/yishizuofei/article/details/81660841

多路查找树：2-3树、2-3-4树、B树、B+树、B*树、R树

https://mp.weixin.qq.com/s/D9kdAPws1XXZUyd0IKUzyw

理解B+树

http://www.cppblog.com/mysileng/archive/2013/04/06/199159.html

Skip List（跳跃表）原理详解与实现

https://mp.weixin.qq.com/s/M5syxE9Ln4UDLThPh5iuJg

各种字符串Hash函数比较

https://mp.weixin.qq.com/s/IaYnfEZ2bUXIpQvN3ZqFGA

图解红黑树

https://juejin.im/post/5e509b27f265da57455b3f33

面试被问“红黑树”，我一脸懵逼......

https://blog.csdn.net/wo541075754/article/details/54632929

Merkle Tree（默克尔树）算法解析（Merkle Tree，通常也被称作Hash Tree，顾名思义，就是存储hash值的一棵树。）

https://mp.weixin.qq.com/s/M8U9B7UA2AdfnJi5EpTv-g

算法面试中经常问的“字符串”问题

https://mp.weixin.qq.com/s/QHounf4el7nmXnpMSJHjvg

这个问题不简单：寻找缺失元素

https://blog.csdn.net/changtao381/article/details/8936765

Splay Tree（一种二叉排序树）

https://blog.csdn.net/simpsonk/article/details/72832959

史上最强图解Treap总结

https://mp.weixin.qq.com/s/p4tddWB4kjFufkv3x2SYpw

图解6种树，你心中有数吗

https://mp.weixin.qq.com/s/fAYjIcFlHoXK2E38JJSFpA

C++优先队列priority_queue

https://mp.weixin.qq.com/s/KZq5SjPESQnQaNU1Mn5a-A

一文把三个经典求和问题吃的透透滴

# 设计模式

面向对象的设计模式有七大基本原则：

开闭原则（Open Closed Principle，OCP）

单一职责原则（Single Responsibility Principle，SRP）

里氏代换原则（Liskov Substitution Principle，LSP）

依赖倒转原则（Dependency Inversion Principle，DIP）

接口隔离原则（Interface Segregation Principle，ISP）

合成/聚合复用原则（Composite/Aggregate Reuse Principle，CARP）

最少知识原则（Least Knowledge Principle，LKP）或者迪米特法则（Law of Demeter，LOD）

在架构领域，有两种常见架构方法RUP和TOGAF。

参考：

https://mp.weixin.qq.com/s/9gDGQhzRAL3pj35VAinZbQ

设计模式在外卖营销业务中的实践

https://mp.weixin.qq.com/s/KYq_nEXQ-5WYdYzZvGLGGg

我向面试官讲解了单例模式，他对我竖起了大拇指

# GAN参考资源+

https://mp.weixin.qq.com/s/cUFQ6EADa39h2eFoa_Dh0A

最高76%破解成功率！GAN已经能造出“万能指纹”，你的手机还安全吗？

https://mp.weixin.qq.com/s/_tABIMkWX8L5xQFmvPI7rw

有效稳定对抗模型训练过程，伯克利提出变分判别器瓶颈

https://mp.weixin.qq.com/s/xr9fDv9DFkwi2ImV4RZAHg

换个角度看GAN：另一种损失函数

https://mp.weixin.qq.com/s/U1rrPfJDLgXHRj__XwrMZw

只有条件GAN才能稳定训练？对抗+自监督的无监督方法了解一下

https://mp.weixin.qq.com/s/xHKQlFFkBQLBg2GdZuGPSw

提升GAN训练的技巧汇总

https://mp.weixin.qq.com/s/ctB90bNhaMYvbLE4yketHQ

一文读懂对抗机器学习Universal adversarial perturbations

https://mp.weixin.qq.com/s/zRNtEKS2dKxyQU4VZm6Mwg

用GANs来自动生成音乐

https://mp.weixin.qq.com/s/zwzl-Tel3Avc4Dm7L5FS5A

NLP中的对抗训练+PyTorch实现

https://mp.weixin.qq.com/s/Ze2BXEexTIpNluRSdfeCsA

GAN和PS合体会怎样？东京大学图像增强新研究：无需配对图像，增强效果还可解释

https://mp.weixin.qq.com/s/qLhnvLhXHhRoPGtPWYCY0w

ICCV2019最佳论文SinGAN全面解读

https://zhuanlan.zhihu.com/p/97138846

《SinGAN:从单个自然图像中学习生成模型》论文笔记

https://mp.weixin.qq.com/s/mXjNtZvUHutBABh-f9qisQ

那些底层的图像处理问题中，GAN能有什么作为？

https://mp.weixin.qq.com/s/zVGDYhBiLCNhKTnkzbMxGA

时间序列GAN，Subadditivity of Probability Divergences

https://mp.weixin.qq.com/s/ssD3NAvGx5Eu4-oWy4KtzA

如何生动有趣地对GAN进行可视化？

https://mp.weixin.qq.com/s/qz4wUpSeF8Nlem8x4CrR-Q

学习一个宫崎骏画风的图像风格转换GAN

https://zhuanlan.zhihu.com/p/50790727

SeqGAN: Sequence GAN with Policy Gradient

https://mp.weixin.qq.com/s/bH5yYbwq6NGQJ84xUDhoxg

生成对抗网络在图像翻译上的应用

https://mp.weixin.qq.com/s/3Gsmrl4HbcnXpje0nyAq2w

中国西北大学和北京大学的研究结果是否将终结CAPTCHA验证码时代？

https://zhuanlan.zhihu.com/p/53260242

抛开复杂证明，我们从直觉上理解W-GAN为啥这么好训

https://mp.weixin.qq.com/s/FJA8Tctq_p4Mj-KgNn_OGg

为什么让GAN一家独大？Facebook提出非对抗式生成方法GLANN

https://mp.weixin.qq.com/s/SGCoCy8wJEhEpLSHrYK3pQ

韩松、朱俊彦等人提出GAN压缩法：算力消耗不到1/9，现已开源

https://mp.weixin.qq.com/s/D-rh9m7G-ERjWEEG6BQJNg

每个人都能用英伟达GAN造脸了

https://mp.weixin.qq.com/s/qRW344wWgS9PJ6UCwknS8g

Local GAN

https://mp.weixin.qq.com/s/-rwzlX-WipaIdV6ESOimxw

GAN加持！英伟达发布“山寨”游戏创造器，已完美复现《吃豆人》

https://mp.weixin.qq.com/s/orm5r4XHyotBCpBKqfZ-ng

GANSynth：使用GAN制作音乐

https://mp.weixin.qq.com/s/4zKgFfyLGAmqgHtf_Wb0nw

使用GAN生成序列数据

https://mp.weixin.qq.com/s/oRCbr0TCzFmTuf5jpWKBaA

GAN版马里奥创作家来了：一个样本即可训练，生成关卡要素丰富

https://mp.weixin.qq.com/s/2NrPolWxV-L-dFUXgGOd9w

在GAN中通过上下文的复制和粘贴，在没有数据集的情况下生成新内容

https://mp.weixin.qq.com/s/iqCMA7E_vtdymVxxz7bpRA

生成对抗网络（GAN）的数学原理全解

https://mp.weixin.qq.com/s/D0gLR6YU3rYTbFqSCwyi9Q

Semi-Supervised GAN
