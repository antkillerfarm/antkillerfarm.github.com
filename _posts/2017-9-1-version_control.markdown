---
layout: post
title:  版本管理工具的前世今生, NLP参考资源（二）
category: resource 
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

# NLP参考资源

https://mp.weixin.qq.com/s/0wMTL7x87YXiin3xkf8eJA

中文文本分类技术实践与分享

https://mp.weixin.qq.com/s/27Mbw2ViT8eJPJasD6tdfw

神经混合模型：提升模型性能，显著降低困惑度

https://mp.weixin.qq.com/s/nIMk-xl8Wzy1ANcT3ApAng

百度提出问答模型GNR：检索速度提高25倍

https://mp.weixin.qq.com/s/eDA6-BqPmjGBOPsOom0VIw

自动组合神经网络做问答系统！

https://mp.weixin.qq.com/s/vt25TVX-tcurtAC63dYYNA

让聊天机器人同你聊得更带劲-对话策略学习

https://mp.weixin.qq.com/s/2dTCvWujgJ71Dsbm-pGcrA

谷歌的多语种神经网络翻译系统！

https://mp.weixin.qq.com/s/AqvzxiI7cNesdGt_IfVb-w

Highway Networks For Sentence Classification

http://mp.weixin.qq.com/s/k4dxj6qTGKNyoTn1uTb_aA

李航：深度学习NLP的现有优势与未来挑战

http://mp.weixin.qq.com/s/ZrFcpYf7E57j8E6zQsBexg

引入秘密武器强化学习，发掘GAN在NLP领域的潜力

https://mp.weixin.qq.com/s/0yRnBW_WWoUHInJcAeVd9A

基于深度学习的候选答案句抽取研究

https://mp.weixin.qq.com/s/y39QT55RcsNAmNS6mT1SKg

深度好奇提出文档解析框架：面向对象的神经规划

https://zhuanlan.zhihu.com/p/27552230

基于Depthwise Separable Convolutions的Seq2Seq模型_SliceNet原理解析

https://mp.weixin.qq.com/s/AlxiYM5ujM9TqzcFVLXK1w

语义分析的一些方法(一)

https://mp.weixin.qq.com/s/7KBtbip4JS0jQfOy_BWb3Q

语义分析的一些方法(二)

http://blog.csdn.net/malefactor/article/details/52166235

聊天机器人中对话模板的高效匹配方法

https://mp.weixin.qq.com/s/Cq7ivDYurS3CidvoemFfqA

Twitter是怎么做情感分析的？长文解读！

https://zhuanlan.zhihu.com/p/30268946

从CNN视角看在自然语言处理上的应用

https://mp.weixin.qq.com/s/EO51mtEf6TVHGHYbZBAdZw

情感回复系统Babbling

https://mp.weixin.qq.com/s/CGZhp1eJU0jd9dcGJzPu0w

LSF-SCNN：一种基于CNN的短文本表达模型及相似度计算的全新优化模型

https://zhuanlan.zhihu.com/p/30533380

Neural Response Generation——关于回复生成工作的一些总结

https://mp.weixin.qq.com/s/ZexnTKHEFKPso5fhZNQfQQ

聊天机器人中用户出行消费意图识别方法研究

https://mp.weixin.qq.com/s/HHPp0JJzpY3q0nC-DF3F3g

阿里：语言卷积神经网络应用于图像标题生成的经验学习

http://www.jianshu.com/p/ac1840abc63f

从Quora的187个问题中学习机器学习和NLP

http://mp.weixin.qq.com/s/-o-Ky5iMA3fZIjh7kzXjew

无监督神经机器翻译：仅需使用单语语料库

https://mp.weixin.qq.com/s/SVKlD0T8IkpVAKFJu-VQAg

胡一川：深度学习在智能助理中的应用

https://mp.weixin.qq.com/s/tGw2c0AMAZSVYddGUQDdOw

沈向洋：懂语言者得天下

https://mp.weixin.qq.com/s/DsE8X2Hgx1oYMoFUswgKkA

Raúl Garreta大神教你5步搭建机器学习文本分类器！

https://mp.weixin.qq.com/s/gIRa-P-xI5DuwY1OLH38MQ

Facebook最新对抗学习研究：无需平行语料库完成无监督机器翻译

https://mp.weixin.qq.com/s/da0M3qwtLN8el2plAQWHJQ

无监督式机器翻译，不需要人类干预和平行文本

https://mp.weixin.qq.com/s/LdU91j8OpDR-pidzzYVtqA

Pointer Networks在自然语言处理领域中的应用

https://mp.weixin.qq.com/s?__biz=MzU2OTA0NzE2NA==&mid=2247484926&idx=1&sn=4e34a442c126c46bba27a7f17df3e61e

聊天机器人Chatbot知识资料全集

https://mp.weixin.qq.com/s/i9AXKsd6XVxyuaTCKsAj3g

像Sheldon一样对“讽刺语言”分辨无能？别怕，MIT最新算法帮你助攻

https://mp.weixin.qq.com/s/Xe-T7EKvGDSR5GOeL4nvKg

从Chatbot到NER

https://mp.weixin.qq.com/s/ux-bheZwkjn0PY3SwiAUWw

用于神经机器翻译的全并行文本生成

https://mp.weixin.qq.com/s/ZlfxZMWz5lSVLxkAf9isJQ

斯坦福李纪为博士毕业论文：让机器像人一样交流

https://mp.weixin.qq.com/s/bNB7EFj1QgSNb9as1YfgJw

Reddit热门话题：你是否也对NLP的现状感到失望？

https://mp.weixin.qq.com/s/uVoLET8LTnC_z0uJ8r0bRA

对话系统综述：新进展新前沿

https://mp.weixin.qq.com/s/2cbzD1UFW66-3zwsZk9Bxg

用于文本的最牛神经网络架构是什么？

https://mp.weixin.qq.com/s/T4AkbKXf2pkyazIlzEZxhw

基于转移的语义依存图分析

https://mp.weixin.qq.com/s/mm9bXXTEzd8DwyYlMgGMZg

NLP多任务学习：一种层次增长的神经网络结构

https://mp.weixin.qq.com/s/hcOoFjFlPIgG1leP6caglA

利用价值网络改进神经机器翻译

https://mp.weixin.qq.com/s/GFI_ZCZxuLUi0tdW_gr7tw

基于循环神经网络问句关键词提取技术研究

https://zhuanlan.zhihu.com/p/31453283

浅谈NLP中条件语言模型(Conditioned Language Models)的生成和评估

https://mp.weixin.qq.com/s/vazeuiFvCC8aIhP2uAXoew

达观数据智能问答技术研究

https://mp.weixin.qq.com/s/M8Awv6Tod4SEo85F8xzqqg

基于半监督学习技术的达观数据文本过滤系统

https://mp.weixin.qq.com/s/Ounb14tu6lyebiOQqY4TyQ

达观数据如何打造一个中文NER系统

https://mp.weixin.qq.com/s/r_x1IUuyIQw3aif2f7-Fxw

邱锡鹏：深度学习在语义分析处理的最新发展

https://mp.weixin.qq.com/s/DWYisJtgAHXaPDQX0lYxfA

达观数据告诉你机器如何理解语言－中文分词技术

https://mp.weixin.qq.com/s/xnxuw06BIKfDTuTeSVguHQ

达观数据基于Deep Learning的中文分词尝试（上篇）

https://mp.weixin.qq.com/s/tk6JyChDThg5yJ4gqLOLwQ

达观数据基于Deep Learning的中文分词尝试（下篇）

https://mp.weixin.qq.com/s/tvtT5S9mDhXoYLKh9apgDw

达观文本指纹算法和系统简述

https://mp.weixin.qq.com/s/eiEME27eDMPSeKkwiisWnQ

达观数据搜索引擎的Query自动纠错技术和架构详解

https://mp.weixin.qq.com/s/RTxLTwNgmpXcuPe7vzI82g

达观数据分享文本大数据的机器学习自动分类方法

https://mp.weixin.qq.com/s/o5mE6IRs8ZkkXy8c5rdrjw

达观数据阐述推荐系统和搜索引擎的关系

https://github.com/Franck-Dernoncourt/NeuroNER/

一个NER方面的示例代码

https://github.com/crownpku/Information-Extraction-Chinese

这是中文NER的代码示例

https://mp.weixin.qq.com/s/5S5ikwnF7f55sL_VI73XSQ

吴恩达博士生Ziang Xie：深度文本生成最佳实战指南

http://www.jianshu.com/p/f45c3540c56e

Chatbot架构

https://mp.weixin.qq.com/s/AhoEzujMVUU-P7j5z_8sVQ

基于神经网络的实体识别和关系抽取联合学习

https://mp.weixin.qq.com/s/mgeQXkEWu5JlZxyAM0yiaQ

充分利用候选项的选择型机器阅读理解模型

https://mp.weixin.qq.com/s/RlarTDziwkGGM_CUa40XAw

机器之心线上分享：用于序列生成的推敲网络

https://mp.weixin.qq.com/s/-qk__SLahfssHuYsGNhKSA

阿里巴巴首次近万字公布人工智能对话交互技术


