---
layout: post
title:  知识图谱参考资源（一）
category: graph 
---

* toc
{:toc}

# 知识图谱参考资源

## 教程

https://web.stanford.edu/class/cs520/

CS 520: Knowledge Graphs

https://mp.weixin.qq.com/s/jqK5A8YTVL3iOKPjKdd6YQ

浙大陈华钧教授《知识图谱导论》课程系列PPT

https://mp.weixin.qq.com/s/dmgfvLUTg14KWqe216kkFg

300页文本知识提取与推断最新教程

## 综述

https://github.com/lihanghang/Knowledge-Graph

知识图谱深度学习相关资料整理

https://mp.weixin.qq.com/s/V_VDw-rRF49qXKTDG1Mpew

知识图谱论文大合集，这份干货满满的笔记解读值得收藏

https://mp.weixin.qq.com/s/aLljr0R5w7OcidbXf71FQw

知识图谱火了，但你知道它的发展历史吗？

https://mp.weixin.qq.com/s/nJip1omVx0HiScIzrlWShA

知识图谱简史：从1950到2019

https://wenku.baidu.com/view/38ad3ef7e109581b6bd97f19227916888586b959.html

知识图谱构建技术综述

https://wenku.baidu.com/view/e69a3619fe00bed5b9f3f90f76c66137ee064f15.html

知识图谱技术综述

https://wenku.baidu.com/view/b3858227c5da50e2534d7f08.html

知识图谱技术原理介绍

https://mp.weixin.qq.com/s/JLYegFP7kEg6n34crgP09g

基于知识图谱的问答系统关键技术研究

https://mp.weixin.qq.com/s/XgKvh63wgEe-CR9bchp03Q

什么是知识图谱？

https://mp.weixin.qq.com/s/sQPi40dCu6KKwQWOrlSGfw

从知识工程到知识图谱全面回顾

https://www.zhihu.com/question/52368821/answer/138745422

鲍捷：知识图谱怎样入门

https://mp.weixin.qq.com/s/WyB5Lssy9c0qJt8Ze7a0Ig

中文知识图谱构建技术以及应用的综述

https://mp.weixin.qq.com/s/cL1aKdu8ig8-ocOPirXk2w

如何构建知识图谱

https://mp.weixin.qq.com/s/qrP_tWlHyc3ZAMPOqu9E0A

知识图谱的发展概述

https://mp.weixin.qq.com/s/fWaEJvxdGbW2p-uYKYgrDA

知识图谱最新研究综述

https://mp.weixin.qq.com/s/cldY4iJFwzWqppn6UVT1rA

新瓶装旧酒：为什么需要知识图谱？什么是知识图谱？——KG的前世今生

https://zhuanlan.zhihu.com/p/44497869

知识图谱中的深度学习技术应用概述

https://mp.weixin.qq.com/s/vtj1HgDC2slsOw9DGoURCA

知识图谱实体链接：一份“由浅入深”的综述

https://mp.weixin.qq.com/s/UADUq4rdw30wpAuJG8mUpg

实体链接：信息抽取中的NLP的基础任务

https://mp.weixin.qq.com/s/0f5E82utl-faCpmvrDoPEg

知识图谱研究综述论文: 表示学习、知识获取与应用，25页pdf详述Knowledge Graphs技术趋势

https://mp.weixin.qq.com/s/L6fMcRa1_me2DKr2KADAjA

知识图谱前沿跟进: Philip S. Yu团队发布权威综述

https://mp.weixin.qq.com/s/qOt0V1XsQvwpKhNiaAfVjA

胡伟-知识图谱融合方法概述分享

https://mp.weixin.qq.com/s/6lTxdxBsw-HJzuKhvNMhcg

基于知识图谱的推荐系统综述

https://mp.weixin.qq.com/s/j9h-8Qk82dozM3LH8b9zMA

一文概览知识图谱在推荐系统的发展现状

## blog

https://zhuanlan.zhihu.com/kb-qa

揭开知识库问答KB-QA的面纱（知识图谱方面的系列专栏）

https://zhuanlan.zhihu.com/knowledgegraph

知识图谱-给AI装个大脑

https://github.com/shaoxiongji/awesome-knowledge-graph

知识图谱论文与笔记

## 知识表示学习

传统的知识图谱主要是基于规则学习构建的，然而规则不是数字，无法与ML/DL结合。因此，在word2vec之类的词向量化获得成功之后，如何对知识图谱进行向量化，就成为了学界的热点。这一向量化的过程，一般被称为知识表示学习。

参考：

https://mp.weixin.qq.com/s/IPKU8jwUk3eOYumbgiL1SQ

重新考虑用简单神经网络进行知识表示学习

https://mp.weixin.qq.com/s/z1hhG4GaBQXPHHt9UGZPnA

东南大学高桓：知识图谱表示学习

https://github.com/thunlp/KRLPapers

“知识表示学习”专题论文推荐

https://mp.weixin.qq.com/s/B-FaW5hAnLmmyuPTihviiQ

基于置信度的知识图谱表示学习框架

### transE

论文：

1、TransE，NIPS2013，《Translating embeddings for modeling multi-relational data》。

2、TransH，AAAI2014，《Knowledge graph embedding by translating on hyperplanes》。

3、TransD，ACL2015，《Knowledge graph embedding via dynamic mapping matrix》。

4、TransA，arXiv2015，《An adaptive approach for knowledge graph embedding》。

5、TransG，arxiv2015，《A Generative Mixture Model for Knowledge Graph Embedding)》

6、KG2E，CIKM2015，《Learning to represent knowledge graphs with gaussian embedding》。

参考：

http://www.cnblogs.com/chenbjin/p/5644457.html

word2vec+transE知识表示模型

http://blog.csdn.net/u011274209/article/details/50991385

TransE算法（Translating Embedding）

http://www.sohu.com/a/116866488_465975

基于翻译模型(Trans系列)的知识表示学习

https://mp.weixin.qq.com/s/2YbfL_1_SyM4wNozyaj4lw

知识图谱嵌入的Translate模型汇总（TransE，TransH，TransR，TransD）

### ComplEx

论文：

《Complex Embeddings for Simple Link Prediction》

### HolE

论文：

《Holographic Embeddings of Knowledge Graphs》

## 知网

知网（HowNet）最早由董振东和董强先生在20世纪90年代设计和构建，至今已有近30年历史。这也是最知名的中文知识图谱。

>董振东，1937～2019，上海人。中文信息处理专家。2011年获得中国中文信息学会首届终身成就奖。

官网：

http://www.keenage.com/

清华大学在此基础上开发了OpenHowNet：

https://hownet.thunlp.org

参考：

https://mp.weixin.qq.com/s/ZSbNOBp8D_qOY0HydTdP8g

清华大学刘知远：在深度学习时代用HowNet搞事情

https://mp.weixin.qq.com/s/6uzW_NNtYUUT8-QRfRKZAg

华人NLP最杰出HowNet成功融入DL模型

## 工具

https://mp.weixin.qq.com/s/CsHgn82lQQDVhjDJvp01cg

清华大学开源OpenKE：知识表示学习平台。

http://openkg.cn/

一个开放的中文知识图谱。

https://xlore.org

XLORE是融合中英文维基、法语维基和百度百科，对百科知识进行结构化和跨语言链接构建的多语言知识图谱，是中英文知识规模较平衡的大规模多语言知识图谱。

## fake news

https://mp.weixin.qq.com/s/Emlzfgoo99T9xAsTKJRQXg

一文看懂虚假新闻检测

https://mp.weixin.qq.com/s/8qFGiMjIHXSwozDmTe7XbA

《打击假新闻: 识别和缓解技术调查》

https://mp.weixin.qq.com/s/5D5cfLC6flnn9fCYlMplMQ

虚假新闻（Fake News）检测全面综述教程，156页PPT带你进入这一领域

https://mp.weixin.qq.com/s/tBuUozLFagqZYmr5e3AnMQ

如何用AI技术治理假新闻泛滥？看ASU大学舒凯等学者这篇《挖掘虚假信息和假新闻:概念、方法和最新进展》研究综述

## 参考

https://mp.weixin.qq.com/s/DR-atmyBQVEMVKIv48sSLA

人工智能技术最重要基础设施之一，知识图谱你该学习的东西

https://mp.weixin.qq.com/s/iqFXvhvYfOejaeNAhXxJEg

当知识图谱遇上聊天机器人

https://mp.weixin.qq.com/s/U-dlYhnaR8OQw2UKYKUWKQ

知识图谱前沿技术课程实录

https://mp.weixin.qq.com/s/WIro7pk7kboMvdwpZOSdQA

东南大学漆桂林：知识图谱的应用

https://mp.weixin.qq.com/s/JZYH_m1eS93KRjkWA82GoA

复旦肖仰华：基于知识图谱的问答系统

https://mp.weixin.qq.com/s?__biz=MjM5ODYwNDUyMQ==&mid=2649658169&idx=1&sn=88392dc21eda4019ade216f8e937bd71

肖仰华：基于知识图谱的可解释人工智能：机遇与挑战

https://mp.weixin.qq.com/s/C1kd8pD0yLu9nO9JVW1pEw

复旦大学肖仰华教授：知识图谱与认知智能

https://mp.weixin.qq.com/s/XTJ0t39SbeIDXtseCUcDBA

复旦大学肖仰华：领域知识图谱落地实践中的问题与对策

https://mp.weixin.qq.com/s/Am9gHuMaYnH_tFmnTSrBQQ

肖仰华教授：知识图谱落地的基本原则与最佳实践

https://mp.weixin.qq.com/s/I4sfT8rlwXIXxEjfdQoFbA

东南大学周张泉：基于知识图谱的推理技术

https://mp.weixin.qq.com/s/fslIcD7RVLEByzkeACUhXQ

达观数据：知识图谱与语义分析技术介绍

https://mp.weixin.qq.com/s/Nh7XJOLNBDdpibopVG4MrQ

中文通用百科知识图谱（CN-DBpedia）

https://zhuanlan.zhihu.com/p/30320631

知识图谱向量化表示及其在推荐系统上的应用

http://www.bigcilin.com/

哈工大在线版的知识图谱

https://mp.weixin.qq.com/s/sb7Q6a7FyZew5Ca-hbwRpQ

医学知识图谱构建技术与研究进展

https://mp.weixin.qq.com/s/1nl56AdZIkT03gnmimt8nQ

刘挺：从知识图谱到事理图谱

https://mp.weixin.qq.com/s/XT0fszONzRkAo3biVwS_QQ

大规模中文概念图谱CN-Probase正式发布

https://mp.weixin.qq.com/s/tDvp8BU3lIWN5ZUSYgBcpg

你不得不看的六篇知识图谱落地好文

http://www.k6k4.com/blog/show/aaazgq11o1478935786270

知识图谱好文章整理

https://mp.weixin.qq.com/s/YeSzOw6dRNiX32PmdWgLow

知识图谱在互联网金融行业的应用

http://blog.csdn.net/hadoopdevelop/article/details/79455758

如何系统学习知识图谱-胖子哥的实践经验分享。一个实践派的知识图谱blog

https://mp.weixin.qq.com/s/qw9i24goTsVgdk1qW6ie9A

知识图谱数据构建的“硬骨头”，阿里工程师如何拿下？

https://mp.weixin.qq.com/s/qsRTBR5g5LZ6UR7Wtqagyw

上交大发布知识图谱AceKG，超1亿实体，近100G数据量

https://mp.weixin.qq.com/s/QbKGm04k_cQbTxnfUFZQBQ

深度学习在知识图谱构建中的应用

https://mp.weixin.qq.com/s/fvgzvZwaMxsZWw-Y5AAe2g

知识图谱的自动构建

https://mp.weixin.qq.com/s/SdCFYYQXdYtQLy-GaQNfcw

UCSB提出变分知识图谱推理：在KG中引入变分推理框架

https://blog.csdn.net/hadoopdevelop/article/details/79784722

基于知识图谱的智能问答机器人技术架构

https://mp.weixin.qq.com/s/SrhACueWelBAGU4sC2nBtg

开放域中文知识图谱《大词林》

https://mp.weixin.qq.com/s/WU6PezrzTwQuXi7DE9Z0sg

W3C之RDFa中文文档

https://mp.weixin.qq.com/s/86y2VP3gMTUnfbN2OK5BNw

基于实体、属性和关系的知识表示学习

https://mp.weixin.qq.com/s/wBuy2-gNrumZ__-H48KEMA

关于知识图谱，各路大神最近都在读哪些论文？

https://mp.weixin.qq.com/s/MG_SrExDkbd1vVGLex0-RA

推荐算法不够精准？让知识图谱来解决

https://mp.weixin.qq.com/s/QO34vyt3uBSKvnYSW0Kumg

如何将知识图谱特征学习应用到推荐系统？

https://mp.weixin.qq.com/s/ePJPoQlOveTrCvCTy8_Xxg

知识图谱的技术与应用

https://mp.weixin.qq.com/s/fuI9U7aZpuk-WX6GQNtOuA

这是一份通俗易懂的知识图谱技术与应用指南

https://mp.weixin.qq.com/s/lBeV6XWzk5bqNGiIMok-zw

一文揭秘！自底向上构建知识图谱全过程

https://mp.weixin.qq.com/s/uSPkgV-zIBEEJ0Ml-ILXqQ

如何从零开始搭建知识图谱？

https://mp.weixin.qq.com/s/3x__Dl1x1nMgWpp0zz_fPg

如何构建知识体系：知识图谱搭建的第一步

https://mp.weixin.qq.com/s/DALB8eBgVWHhFcZoJFLSpg

基于图的RDF知识图谱数据管理

https://mp.weixin.qq.com/s/An9mGawI8mhtlqyrVxTzYw

SIGIR 2018基于知识图谱的文本信息检索
