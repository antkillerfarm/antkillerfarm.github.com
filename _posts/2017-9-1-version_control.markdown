---
layout: post
title:  版本管理工具的前世今生, TensorFlow（二）
category: technology 
---

# 版本管理工具的前世今生

参考Wiki的相关词条，可将版本工具分为三代：

1.本地版本管理
	
开源：SCCS (1972) RCS (1982)

私有：PVCS (1985) QVCS (1991)

以RCS最为著名，不过由于年代久远，我从来没用过。

2.客户端/服务器版本管理

开源：CVS (1986, 1990 in C) CVSNT (1998) QVCS Enterprise (1998) Subversion (2000)

私有：Software Change Manager (1970s) Panvalet (1970s) Endevor (1980s) DSEE (1984) Synergy (1990) ClearCase (1992) CMVC (1994) Visual SourceSafe (1994) Perforce (1995) StarTeam (1995) Integrity (2001) Surround SCM (2002) AccuRev SCM (2002) SourceAnywhere (2003) Vault (2003) Team Foundation Server (2005) Team Concert (2008)

我用过的包括CVS、SVN、ClearCase和Visual SourceSafe。

这一代的工具，以CVS为开端。ClearCase和Visual SourceSafe与CVS差不多同时期，因此功能上也多有相同，总的来说就是ClearCase功能强，但不好用。SourceSafe好用，但功能差。

其中，ClearCase我在之前的公司有用过，当时为了简单的check in和check out，竟然还需要编写脚本以简化流程。不过功能的确是没的说，分支合并权限都不在话下。

VSS以现在的角度来看，基本就是个垃圾了，它是MS收购的一家公司的产品，目的是填补VS在这方面的空白。但这样弱的工具，即使MS内部的人也基本不用，这也是后来MS发布Team Foundation Server的重要原因。

SVN是这一代的集大成者，使用简单的同时，仍保有相当强度的功能，唯一诟病的就是合并功能太弱。

3.分布式版本管理

开源：GNU arch (2001) Darcs (2002) DCVS (2002) ArX (2003) Monotone (2003) SVK (2003) Codeville (2005) Bazaar (2005) Git (2005) Mercurial (2005) Fossil (2007) Veracity (2010)

私有：TeamWare (1990s?) Code Co-op (1997) BitKeeper (1998) Plastic SCM (2006)

早期比较有名的是BitKeeper。它在2002~2005年是Linux Kernel所使用的版本工具。Linus曾经非常喜欢它。可惜后来由于商业利益的关系，开发者的要挟惹毛了Linus。大神发威开发了Git，从此BitKeeper也就无人问津了。

从这件事情可以看出，Linus其实并不是一个像Richard Stallman那样的开源洁癖狂，现有的东西够用，他就懒得节外生枝了。但也不要因此小看他的水平，大神发起威来，搞死像BitKeeper这样的工具还是绰绰有余的。

目前较为主流的Bazaar、Git和Mercurial都出现在2005年，这并不是偶然的。实际上都是BitKeeper和开源社区之间战争的产物。

后Git时代的工具，如Fossil和Veracity，相比Git来说，对权限、BUG跟踪之类的功能做了进一步的扩展。

# TensorFlow

## 参考

https://mp.weixin.qq.com/s/JSZwQkyxSSwfBWKJ578j3A

TensorFlow最好的入门文章

https://mp.weixin.qq.com/s/jMPVl3CWvL7MSzq5F12YxQ

维度、广播操作与可视化：如何高效使用TensorFlow

https://mp.weixin.qq.com/s/EytvywrsgydXAJQhuUqKvg

简易浣熊识别器是如何实现的

https://mp.weixin.qq.com/s/YOyOR8fdaEKcydAywcc-HA

如何使用TensorFlow API构建视频物体识别系统

https://mp.weixin.qq.com/s/gnDTOLWuPZiCVzspTk_zCQ

TensorFlow轻度入门

https://mp.weixin.qq.com/s/MYBTWL3X_OhLZL6C4rISzw

TensorFlow训练线性回归

http://www.jianshu.com/p/d443aab9bcb1

在TensorFlow上使用LSTM进行情感分析

https://mp.weixin.qq.com/s/5QYlh6gV9IqdQfraK4DC8w

10种深度学习算法的TensorFlow实现

https://zhuanlan.zhihu.com/p/28475975

如何优雅地用TensorFlow预测时间序列：TFTS库详细教程

https://mp.weixin.qq.com/s/zZCEOdNQsPovn_i-C57Z9g

如何使用最流行框架Tensorflow进行时间序列分析？

https://mp.weixin.qq.com/s/CqOo7Fu6t5-yJiYhzo03oQ

利用TensorFlow和神经网络来处理文本分类问题

https://mp.weixin.qq.com/s/VlvQmrS7Qi2qq6fTBXKTYw

从零开始用TensorFlow搭建卷积神经网络

https://mp.weixin.qq.com/s/hETnA81WlkMG3rftAHg9bw

PyTorch和TensorFlow哪家强：九项对比读懂各自长项短板

http://blog.csdn.net/wiinter_fdd/article/details/72821923

Tensorflow中的模型持久化

https://mp.weixin.qq.com/s/7R-Gvegnta9XBwIaSPBL_Q

基于Tensorflow的验证码识别

https://mp.weixin.qq.com/s/Es_5KUnkDzMwf_8WD8aW3g

GitHub万星：适用于初学者的TensorFlow代码资源集

https://mp.weixin.qq.com/s/3QgtemxxsQmuNQVEdpiMwA

如何做准确率达98%的交通标志识别系统？

https://mp.weixin.qq.com/s/pSE2V8wD3_KHMI71kLTXng

如何基于TensorFlow使用LSTM和CNN实现时序分类任务

https://mp.weixin.qq.com/s/dHkmDvFVUGmt4Ch-gv3s1g

一步一步带你用TensorFlow玩转LSTM

https://mp.weixin.qq.com/s/Bx5Djj-RE0jPJ7LjyQ7GPg

基于gym和tensorflow的强化学习算法实现

https://mp.weixin.qq.com/s/3URLEdhB8hs0XXekKbvsnw

使用TensorFlow在卷积神经网络上实现L2约束的softmax损失函数

https://mp.weixin.qq.com/s/dgLJrn3omUKMqmqTIEcoyg

Tensorflow实现DDPG

https://mp.weixin.qq.com/s/FdPrRQr0LukcWh7B703MlQ

利用tf.gradients在TensorFlow中实现梯度下降

# NLP参考资源

https://mp.weixin.qq.com/s/GyE9qdXPGvrq12dMAf4nrQ

论文推荐：QA，增强学习，知识图谱，机器阅读理解

https://mp.weixin.qq.com/s/eCtqMIo3_UDxAR4fWzMjZQ

站在锤子手机背后，小源科技用AI打造短信场景服务

https://mp.weixin.qq.com/s/hkcVhyqiDW--F3QxucgEcA

亚马逊开源神经机器翻译框架Sockeye：基于Apache MXNet的NMT平台

https://mp.weixin.qq.com/s/vgejlnsBlOKMMRt-M3-d-w

深思考：实现人机多轮交互突破是攻克图灵测试的核心

https://mp.weixin.qq.com/s/0wMTL7x87YXiin3xkf8eJA

中文文本分类技术实践与分享

https://mp.weixin.qq.com/s/27Mbw2ViT8eJPJasD6tdfw

神经混合模型：提升模型性能，显著降低困惑度


