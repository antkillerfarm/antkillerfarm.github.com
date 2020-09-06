---
layout: post
title:  深度学习（二十九）——Graph NN（1）
category: DL 
---

* toc
{:toc}

# Graph NN

Graph Neural Networks是2019年以来比较热门的方向。然而由于没有大佬全面投入，相关研究比较零散，被人戏称paper survey比paper还多。。。囧

![](/images/img3/GNN.jpg)

由于Graph Neural Networks和图表示学习(Represent Learning for Graph)有很密切的联系。因此，这里的章节编排上如无特殊说明，不对两者的内容加以区分。

## 教程

http://web.stanford.edu/class/cs224w/

CS224W: Machine Learning with Graphs

https://mp.weixin.qq.com/s/xc_TnMLs3o2LQ8eM4naZDw

AAAI2019 Tutorial《图表示学习》, 180页PPT带你从入门到精通

https://mp.weixin.qq.com/s/tD49ynMOyVTK-oWllcUWaw

图神经网络新书《图表示学习》，140页pdf，William L. Hamilton-McGill University

https://mp.weixin.qq.com/s/rgcDlFA1_Qbu8xRH7WZrtA

清华大学《图神经网络-算法、理论和应用》教程

https://mp.weixin.qq.com/s/0l2uOhmoBJOZJe0VO3cuZw

南洋理工大学：图神经网络，Graph Neural Networks，附121页ppt

https://mp.weixin.qq.com/s/LrGWJIdPdUNZ3jyC8tdE6w

Graph Neural Network（GNN）综述

https://zhuanlan.zhihu.com/p/75307407

图神经网络（Graph Neural Networks，GNN）综述

https://mp.weixin.qq.com/s/WW-URKk-fNct9sC4bJ22eg

深度学习时代的图模型，清华发文综述图网络

https://mp.weixin.qq.com/s/Rr6SC-se_0q8dfEz0oUwlA

清华大学孙茂松课题组:《图神经网络: 方法与应用》综述论文

https://mp.weixin.qq.com/s/b_QqUxFbQ70xmsxGMtoaDQ

网络图模型知识点综述

https://zhuanlan.zhihu.com/p/54505069

图卷积网络(GCN)新手村完全指南

https://mp.weixin.qq.com/s/yGwKK_pl5p9mg_KKFiQkSA

图神经网络GNN的自然语言处理，附315页PPT及作者博士论文下载

https://mp.weixin.qq.com/s/IHXDqlU1dURrwAIwps50_g

新加坡国立大学：基于图学习与推理的推荐系统，附133页ppt

https://mp.weixin.qq.com/s/8jCX3Wi-w-b9AbEx9sa36A

58页PPT揭示图神经网络研究最新进展

https://mp.weixin.qq.com/s/lK5b3E84e2idh64v3SBfkg

南洋理工Xavier：图深度学习最新进展，35页ppt，Deep Learning on Graphs

https://mp.weixin.qq.com/s/eA9a3478oxHd3zsepNVTpQ

图数据表示学习综述论文

https://mp.weixin.qq.com/s/zDXlJtqDRW_Mm56gL4MLEw

《图机器学习导论》附69页PPT

https://mp.weixin.qq.com/s/AzJ_X2xpOTXmYN-GxzghzA

图神经网络GNN模型与应用：305页ppt教程，密歇根州立大学

https://mp.weixin.qq.com/s/hyHUkiEyXGn3v-M0d0igVg

想入门图深度学习？这篇55页的教程帮你理清楚了脉络

https://mp.weixin.qq.com/s/ePqSwDhCgE1fGWSphuZuBg

WSDM2020教程《基于图学习和推理的推荐系统》，附130页PPT下载

https://mp.weixin.qq.com/s/t1VojTRdnULTiycE-qnypw

图神经网络（GNN）过去、现在、应用和未来最新研究进展分享

https://mp.weixin.qq.com/s/B3BFZkmHkLT7WsI4ssdODA

AGL:可扩展工业图机器学习系统

https://mp.weixin.qq.com/s/_qhqJTntrty-hr3BC_Kskg

图表示学习算法推理，46页ppt，Petar@DeepMind

https://mp.weixin.qq.com/s/-QEwQgZ0t33r92W6kcqSIw

图神经网络表达能力的研究综述，41页pdf

https://mp.weixin.qq.com/s/oVJnVNQBuAPsWKTgOCxCRw

韩家炜：最新《异构网络表示学习》2020综述论文大全

https://mp.weixin.qq.com/s/f61AX_Gt_UNEopw1JrkKCw

图神经网络推理，27页ppt精炼讲解

https://mp.weixin.qq.com/s/4wqIxphHauecSCRxy-6p3w

元学习与图神经网络逻辑推导，55页ppt

https://mp.weixin.qq.com/s/fUZ0G0lzqfzaCjPSYBNIzQ

图神经网络，Graph Neural Networks，附60页ppt

https://mp.weixin.qq.com/s/So07A88G2fGYDiDZgrv3EA

最新图学习推荐系统综述：Graph Learning Approaches to Recommender Systems

https://mp.weixin.qq.com/s/Nvgt70529OQ5f7fkGH2Pgw

最新《图卷积神经网络GCN》2020概述，76页ppt，NTU-Xavier Bresson，纽约大学深度学习课程

https://mp.weixin.qq.com/s/byVdEPcCmVPJOk-uIyGsbw

GCN大佬Thomas Kipf博士论文《深度学习图结构表示》178页pdf阐述图卷积神经网络等机制与应用

https://mp.weixin.qq.com/s/6kzFlHqJPYzHPhyS2nxLOw

最新《图机器学习》综述论文，38页pdf阐述最新图表示学习进展

https://mp.weixin.qq.com/s/aDQFz_IYrhOmAXGvjbCKXg

图神经网络导论，清华大学刘知远

https://mp.weixin.qq.com/s/AMGhs8XEJrr9-L5NRiSYWw

神经网络的图结构，48页ppt（尤佳轩&何恺明）

https://mp.weixin.qq.com/s/hvVxgND75-sKUdWhr-OWOw

图神经网络:基础与应用，322页ppt

## 综述

《A Comprehensive Survey on Graph Neural Networks》

《Deep Learning on Graphs: A Survey》

## DGL

https://mp.weixin.qq.com/s/I8pGqpKnRJp9HRglHfMZCw

手把手教你用DGL框架进行批量图分类

https://mp.weixin.qq.com/s/rGC8O2Pyq8WL8D8ATMbH0Q

NYU、AWS联合推出：全新图神经网络框架DGL正式发布

https://zhuanlan.zhihu.com/p/93828551

图神经网络库DGL零基础上手指南

https://zhuanlan.zhihu.com/p/115342917

四大图神经网络架构

## PyTorch Geometric

https://mp.weixin.qq.com/s/5HOA9Pmb3fjsfTVnFMdBIA

新的PyTorch图神经网络库，比前辈快14倍

https://mp.weixin.qq.com/s/_aIPVnJfTWMkCbh4h6MAEA

PyTorch & PyTorch Geometric图神经网络(GNN)实战

https://mp.weixin.qq.com/s/E8m0bAHxcwHRJQlc3nJhlg

Github火爆图神经网络框架pytorch_geometric原理解析—基于边的高效GNN实现

## PyTorch-BigGraph

https://mp.weixin.qq.com/s/Ux3_baKdA_Fee-jmcs4Myg

开源了！现在用PyTorch做超大规模图嵌入，上亿个节点也能快速完成

https://mp.weixin.qq.com/s/OUjMmxio9OCyuN0mJW-fdg

完爆旧系统！Facebook开源图神经网络库PBG，无需GPU搞定数十亿节点图嵌入

https://mp.weixin.qq.com/s/idznSOGOp0o5N86boLo3aw

使用Facebook Pytorch BigGraph从知识图谱中提取知识

https://mp.weixin.qq.com/s/FpLYdowTUzApeiQP1d7DNg

Pytorch Biggraph简介及官方文档解读

## Graph Nets

https://mp.weixin.qq.com/s/c5rvWfIjujw6TNszDzPMdw

DeepMind开源图深度学习(GraphDL)工具包，基于Tensorflow和Sonnet

## NetworkX

NetworkX是一个用Python语言开发的图论与复杂网络建模工具，内置了常用的图与复杂网络分析算法，可以方便的进行复杂网络数据分析、仿真建模等工作。

官网：

https://networkx.github.io/

参考：

https://www.cnblogs.com/kaituorensheng/p/5423131.html

python复杂网络分析库NetworkX

https://mp.weixin.qq.com/s/WYM7k9gddAndlLBuQWTbSA

一文读懂Python复杂网络分析库networkx

## SNAP

SNAP（Stanford Network Analysis Platform）是一个复杂网络分析的库。

官网：

http://snap.stanford.edu/

## Other Tools

https://mp.weixin.qq.com/s/POMluy69sphGZ_AlDnJ0og

阿里重磅发布大规模图神经网络平台AliGraph，技术架构和算法独家解读

https://mp.weixin.qq.com/s/KjlIa3oxqfk-iu6Ba5NixQ

图神经网络开发必备组件，NetworkX、稀疏矩阵、稀疏Tensor等

https://mp.weixin.qq.com/s/CvV16eK9EUm148dOw0EEcA

TensorFlow开源NSL神经结构学习框架

https://mp.weixin.qq.com/s/Uf8l2yn5iCFCUFWVvIvAOw

腾讯开源图计算框架Plato

https://mp.weixin.qq.com/s/PEltOwR1Am7RX6N-4UN9vw

集成图网络模型实现、基准测试，清华推出图表示学习工具包（CogDL）

https://mp.weixin.qq.com/s/zS2Slg33yAi3xKNjP_-oWg

灵活、轻便，阿里开源简化GNN应用框架Graph-Learn

## DeepWalk

https://mp.weixin.qq.com/s/SXnRyUj_mMs8UEtNyP6qNw

DeepWalk论文解读

https://mp.weixin.qq.com/s/h1vDImYTLEheatZnScZwbg

使用DeepWalk从图中提取特征

https://mp.weixin.qq.com/s/F2jF1vuzK4u8ZPrDK_CyLw

KDD2018网络表示学习最新教程：DeepWalk作者Perozzi等人带你探索最前沿

## 概述

最早的图神经网络起源于Franco博士的论文《The graph neural network model》。

>Franco Scarselli，意大利人，University of Florence大学博士。University of Siena教授。

## 参考

https://github.com/thunlp/GNNPapers

清华NLP图神经网络GNN论文分门别类，16大应用200+篇论文

https://github.com/nnzhan/Awesome-Graph-Neural-Networks

图神经网络论文列表

https://github.com/DeepGraphLearning/LiteratureDL4Graph

图深度学习资源汇总

https://github.com/IndexFziQ/GNN4NLP-Papers

自然语言领域中图神经网络模型（GNN）应用现状（论文列表）

https://github.com/jdlc105/Must-read-papers-and-continuous-tracking-on-Graph-Neural-Network-GNN-progress

Papers on Graph neural network(GNN)

https://github.com/benedekrozemberczki/awesome-graph-classification

图网络大列表

https://mp.weixin.qq.com/s/SW6V-AxGq1z9Uq7qIJLj5A

Github上热门图深度学习（GraphDL）源码与工业级框架

http://www.p-chao.com/2019-01-20/%e5%9b%be%e7%a5%9e%e7%bb%8f%e7%bd%91%e7%bb%9cgnn/

图神经网络GNN的简单理解

https://mp.weixin.qq.com/s/_TAhfkjj1wsWEZDT8q5K8Q

图表示学习极简教程

https://github.com/icoxfog417/graph-convolution-nlp

图卷积神经网络自然语言处理应用代码和教程

https://mp.weixin.qq.com/s/VEAnkznZUyZ1RCJulSnwGg

基于图结构网络的表征学习

https://mp.weixin.qq.com/s/rxZQrhvRk6Dw3AWpGJS4dg

《基于图的句子意思表征》教程, 300多页PPT带你进入这一新兴领域

https://mp.weixin.qq.com/s/bMpugd2Lp35VPr8fQAPzsg

一文概览图卷积网络基本结构和最新进展

https://mp.weixin.qq.com/s/w5ldyp00CqkX8Kp-8Aw0nQ

图深度学习(GraphDL)，下一个人工智能算法热点？一文了解最新GDL相关文章

https://mp.weixin.qq.com/s/Jt6CjMqNFEXWoL5pkLeVyw

洛桑理工：Graph上的深度学习报告

https://mp.weixin.qq.com/s/TGuEvNXw_9S5-9a3KyDvvw

基于图卷积网络的图深度学习

https://mp.weixin.qq.com/s/kcXp-uWcmIsAVfa63mor4g

图卷积网络介绍及进展

https://mp.weixin.qq.com/s/eelcT5x_kWC0dDt0_Ph4qg

清华朱文武组一文综述GraphDL五类模型

https://mp.weixin.qq.com/s/0rs8Wur7Iv6jSpFz5C-KNg

来自IEEE Fellow的GNN综述

https://mp.weixin.qq.com/s/cdbHoR_E_mpIdcvmNGWfDA

掌握图神经网络GNN基本，看这篇文章就够了

https://www.cnblogs.com/SivilTaram/p/graph_neural_network_1.html

从图(Graph)到图卷积(Graph Convolution)：漫谈图神经网络模型 (一)

https://www.cnblogs.com/SivilTaram/p/graph_neural_network_2.html

从图(Graph)到图卷积(Graph Convolution)：漫谈图神经网络模型 (二)

https://www.cnblogs.com/SivilTaram/p/graph_neural_network_3.html

从图(Graph)到图卷积(Graph Convolution)：漫谈图神经网络模型 (三)

https://mp.weixin.qq.com/s/Irs_fLrf4oybc3sAfpmEeA

图嵌入（Graph embedding）综述

https://mp.weixin.qq.com/s/hyW3b7o4kRZN0oflMLYlTw

图嵌入概述

https://mp.weixin.qq.com/s/4YlDC24vC-H7PHRZhhiZJg

图节点嵌入(Node Embeddings)概述，9页pdf

https://mp.weixin.qq.com/s/s6E2vV1KrQDI4SeAnkYTKw

图神经网络将成AI下一拐点！MIT斯坦福一文综述GNN到底有多强
