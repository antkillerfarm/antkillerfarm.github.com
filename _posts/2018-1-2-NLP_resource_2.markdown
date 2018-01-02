---
layout: post
title:  NLP参考资源（三）
category: resource 
---

# NLP参考资源

https://zhuanlan.zhihu.com/p/32314500

DL实战课程推荐-从0到1构建一个Chatbot系统

https://mp.weixin.qq.com/s/mNG9P6hvbRIGq7f5Jx4jIw

Facebook开源无监督机器翻译模型和大规模训练语料

https://mp.weixin.qq.com/s/GhJRebSXXq0ZFh3evAqLxw

漫谈神经语言模型之中文输入法

https://mp.weixin.qq.com/s/HMLwjLlFXR4WM92_OnKHbA

基于Apache MXNet，亚马逊NMT开源框架Sockeye论文介绍

https://mp.weixin.qq.com/s/rF4zKCbj3Jh8dDR1239lpA

基于双语主题模型的跨语言层次分类体系匹配

https://mp.weixin.qq.com/s/gQ9dV-IPWHTOLbI6u0D67g

可解释推荐系统：身怀绝技，一招击中用户心理

https://mp.weixin.qq.com/s/M_ZN0YuvegAnc2GXCZTt9Q

初学者指南：神经网络在自然语言处理中的应用

https://mp.weixin.qq.com/s/ouNxUvWC_mxap6P5z6_dFA

教聊天机器人进行多轮对话

https://mp.weixin.qq.com/s/fZv9FgbdQ1bWPoNdl9sF1A

“宝石迷阵”与信息检索

http://www.jianshu.com/p/0273c377c34e

机器学习算法在文本分类中的应用综述

https://mp.weixin.qq.com/s/BcUB3bXrPJF0WQetWs7Law

用深度学习解决自然语言处理中的7大问题，文本分类、语言建模、机器翻译等

https://mp.weixin.qq.com/s/GyE9qdXPGvrq12dMAf4nrQ

论文推荐：QA，增强学习，知识图谱，机器阅读理解

https://mp.weixin.qq.com/s/eCtqMIo3_UDxAR4fWzMjZQ

站在锤子手机背后，小源科技用AI打造短信场景服务

https://mp.weixin.qq.com/s/hkcVhyqiDW--F3QxucgEcA

亚马逊开源神经机器翻译框架Sockeye：基于Apache MXNet的NMT平台

https://mp.weixin.qq.com/s/vgejlnsBlOKMMRt-M3-d-w

深思考：实现人机多轮交互突破是攻克图灵测试的核心

https://mp.weixin.qq.com/s/rqB2COCAKsr3qvVXnhLwMg

数据到文本任务的近期相关工作介绍

https://mp.weixin.qq.com/s/KWWbaE2em4fEBInPoWTq8A

Python实现多种模型(Naive Bayes, SVM, CNN, LSTM, etc)用于推文情感分析

https://mp.weixin.qq.com/s/-K3WIo64_wJb9p6C49Z5fA

基于属性学习和额外知识库的图像描述生成和视觉问答

https://mp.weixin.qq.com/s/_klvAhH-K8tcYmamlr6FcA

Logistic Regression Models分析交互式问答

https://mp.weixin.qq.com/s/V3w5Iu804O6PmnBjmwCbgw

常用文本特征选择

https://mp.weixin.qq.com/s?__biz=MzIwMTQ4MzQwNQ==&mid=2653318072&idx=1&sn=a4f8912af0b1af816739a483f933803a

NLP概念图及自然语言系统架构简说

https://mp.weixin.qq.com/s/7yi67lPjseigeaqTD269mQ

触类旁通，专业技能热度智能分析

https://mp.weixin.qq.com/s/Zo6LjGE__vQMsWyuoZm6Hw

文本特征工程之N-Gram

https://mp.weixin.qq.com/s/PmdARJQC8O42huzxZNSz3A

搭建基于依存句法和短语结构句法结合的金融领域事件元素抽取系统实践

http://www.cnblogs.com/qcloud1001/p/7910255.html

LSF-SCNN：一种基于CNN的短文本表达模型及相似度计算的全新优化模型

http://www.cnblogs.com/qcloud1001/p/8074771.html

神经张量网络（NTN）：探索文本实体之间的关系

https://mp.weixin.qq.com/s/Y-skeJvkWlgkwKBpCjPaKA

中文文本挖掘流程详解

https://mp.weixin.qq.com/s/wqJP64BuIT27Le6esZNdLg

浅析深度学习在实体识别和关系抽取中的应用

https://mp.weixin.qq.com/s/Xy_jaOex72pLe4wwieKeUg

简单有效的多标准中文分词

# ACBM算法

ACBM算法是在AC（Aho-Corasick）自动机（UNIX上的fgrep命令使用的就是AC算法）的基础之上，引入了BM（Boyer-Moore）算法的多模扩展，实现的高效的多模匹配。和AC自动机不同的是，ACBM算法不需要扫描目标文本串中的每一个字符，可以利用本次匹配不成功的信息，跳过尽可能多的字符，实现高效匹配。

>注： Alfred Vaino Aho，1941年生，加拿大计算机科学家。普林斯顿大学博士，长期供职于贝尔实验室，后为哥伦比亚大学教授。egrep和fgrep的发明人，AWK语言的联合发明人。著有《Principles of Compiler Design Compilers: Principles, Techniques, and Tools》。该书由于封面上有龙图案，又被称为“龙书”，是编译原理方面的权威书籍。2003年获IEEE John von Neumann Medal。

>Margaret John Corasick，贝尔实验室研究员。

>Robert Stephen Boyer，德克萨斯大学教授。

>J Strother Moore，德克萨斯大学教授。Boyer的好朋友，两人的绝大多数成就都是合作完成的。

参见：

http://blog.csdn.net/sealyao/article/details/6817944

ACBM算法

https://mp.weixin.qq.com/s/yVOgAuD9hEYAMdLyvae2VA

最长公共子序列与最长公共子串

https://mp.weixin.qq.com/s/8rP3fF9iVnplhkFmxuj6fw

一文读懂KMP算法

# 高斯过程回归

从大的分类来说，机器学习的算法可分为两类：

1.定义一个模型，用训练数据训练模型的参数，然后用训练好的模型进行预测。这种方法的缺点在于，预测效果和模型与样本的匹配程度有关。比如对非线性样本采用线性模型，其预测效果通常不会太好。但是增加模型的复杂度，又会导致过拟合。

2.定义一个函数分布，赋予每一种可能的函数一个先验概率，可能性越大的函数，其先验概率越大。但是可能的函数往往为一个不可数集，即有无限个可能的函数，随之引入一个新的问题：如何在有限的时间内对这些无限的函数进行选择？一种有效解决方法就是高斯过程回归(Gaussian process regression，GPR)。

>注：Radford M. Neal，1956年生，加拿大科学家。多伦多大学博士（1995）和教授。贝叶斯神经网络的发明人。导师为Geoffrey Hinton。   
>个人主页：http://www.cs.toronto.edu/~radford/

>Danie G. Krige，1919～2013，南非矿业工程师和统计学家，威特沃特斯兰德大学教授。地理统计学早期的代表人物之一。

http://www.cnblogs.com/hxsyl/p/5229746.html

浅谈高斯过程回归

https://mqshen.gitbooks.io/prml/content/Chapter6/gaussian/gaussian_processes_regression.html

Gaussian Processes Regression

http://www.gaussianprocess.org/gpml/chapters/RW.pdf

Gaussian Processes for Machine Learning

http://people.cs.umass.edu/~wallach/talks/gp_intro.pdf

Introduction to Gaussian Process Regression

http://wenku.baidu.com/view/72f80113915f804d2b16c173.html

高斯过程回归方法综述


