---
layout: post
title:  深度学习（二十九）——Graph NN（1）
category: DL 
---

# Graph NN

Graph Neural Networks是2019年以来比较热门的方向。然而由于没有大佬全面投入，相关研究比较零散，被人戏称paper survey比paper还多。。。囧

![](/images/img3/GNN.jpg)

## 教程

http://web.stanford.edu/class/cs224w/

CS224W: Machine Learning with Graphs

https://mp.weixin.qq.com/s/xc_TnMLs3o2LQ8eM4naZDw

AAAI2019 Tutorial《图表示学习》, 180页PPT带你从入门到精通

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

## DGL

https://mp.weixin.qq.com/s/I8pGqpKnRJp9HRglHfMZCw

手把手教你用DGL框架进行批量图分类

https://mp.weixin.qq.com/s/rGC8O2Pyq8WL8D8ATMbH0Q

NYU、AWS联合推出：全新图神经网络框架DGL正式发布

https://zhuanlan.zhihu.com/p/93828551

图神经网络库DGL零基础上手指南

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

## Graph Nets

https://mp.weixin.qq.com/s/c5rvWfIjujw6TNszDzPMdw

DeepMind开源图深度学习(GraphDL)工具包，基于Tensorflow和Sonnet

## Other Tools

https://mp.weixin.qq.com/s/POMluy69sphGZ_AlDnJ0og

阿里重磅发布大规模图神经网络平台AliGraph，技术架构和算法独家解读

https://mp.weixin.qq.com/s/KjlIa3oxqfk-iu6Ba5NixQ

图神经网络开发必备组件，NetworkX、稀疏矩阵、稀疏Tensor等

https://mp.weixin.qq.com/s/CvV16eK9EUm148dOw0EEcA

TensorFlow开源NSL神经结构学习框架

https://mp.weixin.qq.com/s/Uf8l2yn5iCFCUFWVvIvAOw

腾讯开源图计算框架Plato

https://mp.weixin.qq.com/s/h1vDImYTLEheatZnScZwbg

使用DeepWalk从图中提取特征

https://mp.weixin.qq.com/s/PEltOwR1Am7RX6N-4UN9vw

集成图网络模型实现、基准测试，清华推出图表示学习工具包（CogDL）

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

https://mp.weixin.qq.com/s/Irs_fLrf4oybc3sAfpmEeA

图嵌入（Graph embedding）综述

https://mp.weixin.qq.com/s/hyW3b7o4kRZN0oflMLYlTw

图嵌入概述

https://mp.weixin.qq.com/s/s6E2vV1KrQDI4SeAnkYTKw

图神经网络将成AI下一拐点！MIT斯坦福一文综述GNN到底有多强

https://mp.weixin.qq.com/s/5oOobY_3blbXYYxuuQmShQ

一文读懂图神经网络

https://mp.weixin.qq.com/s/U51C2t92nlE7Tv7oKXgx2A

一份完全解读：是什么使神经网络变成图神经网络？

https://mp.weixin.qq.com/s/vK0bzljCNdR1OumUmsi2sA

斯坦福大牛Jure Leskovec：图神经网络研究最新进展

https://mp.weixin.qq.com/s/lt9lZbulkW0C8A_xi6hodQ

浅析图卷积神经网络

https://mp.weixin.qq.com/s/XSug_qOqq_QaphkiRlGkIg

图卷积GCN前沿方法介绍

https://mp.weixin.qq.com/s/aeQyZ8cpz81cK8Dg-84mjA

网络表征学习综述

https://mp.weixin.qq.com/s/bsNDI9YxFdaB2Q5aRz9ECw

图卷积神经网络的变种与挑战

https://mp.weixin.qq.com/s/oKwxWbCkH-xqYSJIBdb92A

2018超网络节点表示学习

https://mp.weixin.qq.com/s/WQlSghxG89JCroNZSmop8w

朱军：关于图的表达学习

https://mp.weixin.qq.com/s/WMpcamrHjUDnYwqyISdooA

斯坦福Jure Leskovec图表示学习：无监督和有监督方法

https://mp.weixin.qq.com/s/mTCrTPzyeogwRHfgitfK6Q

为什么说图网络是AI的未来？

https://mp.weixin.qq.com/s/B8rJRlnwGJKUSI17Ot66Xw

从CNN到GCN的联系与区别——GCN从入门到精（fang）通（qi）

https://mp.weixin.qq.com/s/DUv5c6ce-dgLOBAE4ChiQg

图神经网络为何如此强大？看完这份斯坦福31页PPT就懂了！

https://mp.weixin.qq.com/s/OV-rXGU8DTNqv3QZcKo00Q

Graph Learning

https://mp.weixin.qq.com/s/2SaBiMJzhSRw1G0giL9TAw

深入理解图注意力机制

https://mp.weixin.qq.com/s/sg9O761F0KHAmCPOfMW_kQ

图卷积网络到底怎么做，这是一份极简的Numpy实现

https://mp.weixin.qq.com/s/PkUJsnZdihPM7q9BpvO8Ag

深度学习中不得不学的Graph Embedding方法

https://mp.weixin.qq.com/s/PxNGJ0hcmCo-2zvWD-rfug

GCN作者Thomas Kipf最新Talk：利用图神经网络进行无监督学习

https://mp.weixin.qq.com/s/CpDZEqo14X_lCBh6i7feIA

图卷积神经网络(GCN)文本分类详述

https://mp.weixin.qq.com/s/t2kjxrcn6O9tbJ-IQELboQ

高君宇：图神经网络在视频分类中的应用

https://mp.weixin.qq.com/s/SWcJut6QqOvbziirxTd2Kg

斯坦福教授ICLR演讲：图网络最新进展GraphRNN和GCPN

https://mp.weixin.qq.com/s/Lakq83_ngUJf1ES3N7J9_g

图卷积在基于骨架的动作识别中的应用

https://mp.weixin.qq.com/s/5wSgC4pXBfRLoCX-73DLnw

什么是图卷积网络？行为识别领域新星

https://mp.weixin.qq.com/s/1-Dmckby2NcXsaoK08zk8w

视频理解中的图表示学习

https://mp.weixin.qq.com/s/sJB4N_ObUqKM8H65yU_1sg

Graph基础知识介绍

https://mp.weixin.qq.com/s/jBQOgP-I4FQT1EU8y72ICA

图神经网络的“开山之作”CGN模型

https://mp.weixin.qq.com/s/DJAimuhrXIXjAqm2dciTXg

何时能懂你的心——图卷积神经网络（GCN）

https://mp.weixin.qq.com/s/edrh-HXqW01Yx7c8tQ8UxA

从数据结构到算法：图网络方法初探

https://mp.weixin.qq.com/s/ftz8E5LffWFfaSuF9uKqZQ

Graph Neural Network：GCN算法原理，实现和应用

https://mp.weixin.qq.com/s/JvtrGa0YiUmR6UA5wBQ-pQ

图神经网络GNN最新理论进展和应用探索

https://mp.weixin.qq.com/s/zQU47tjpTCPiLdEmUmZx3Q

图卷积神经网络及其应用

https://mp.weixin.qq.com/s/8Sz_jo7pokL_nzupEBGGdg

当深度强化学习遇见图神经网络

https://mp.weixin.qq.com/s/_aydey5ZVwrObmoFXXIYcw

Bengio等人提出图注意网络架构GAT，可处理复杂结构图

https://zhuanlan.zhihu.com/p/34232818

《Graph Attention Networks》阅读笔记

https://zhuanlan.zhihu.com/p/28170197

《Gated Graph Sequence Neural Networks》阅读笔记

https://blog.csdn.net/yorkhunter/article/details/104056795

综述论文“A Comprehensive Survey on Graph Neural Networks”

https://mp.weixin.qq.com/s/rTnv7XOnIvRDGDdhbuoE9g

深度图相似学习综述

https://mp.weixin.qq.com/s/FDWoAXGOlRxOEkvli3gd5Q

55页图深度学习导论《A Gentle Introduction to Deep Learning for Graphs》

https://mp.weixin.qq.com/s/Pm1HiEQOBnbo_GQ_v6Y_zw

腾讯提出自适应图卷积神经网络，接受不同图结构和规模的数据

https://zhuanlan.zhihu.com/p/31067515

《Semi-Supervised Classification with Graph Convolutional Networks》阅读笔记

https://mp.weixin.qq.com/s/6viSk0Ts_7eTfYrWYi_HDQ

基于图结构的实体和关系联合抽取模型简介

https://zhuanlan.zhihu.com/p/36117802

《Learn to Represent Programs with Graphs》阅读笔记。这篇论文讲述了DL在程序代码纠错方面的应用。
