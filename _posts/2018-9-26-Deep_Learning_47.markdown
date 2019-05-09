---
layout: post
title:  深度学习（四十七）——深度图像压缩, 深度贝叶斯学习, Spiking Neuron Networks, BERT
category: DL 
---

# 深度图像压缩

Tiny Network Graphics是图鸭科技推出一种基于深度学习的图片压缩技术。由于商业因素，这里没有论文，技术细节也不详，但是下图应该还是有些用的。

![](/images/img2/TNG.png)

参考：

https://mp.weixin.qq.com/s/WYsxFX4LyM562bZD8rO95w

图鸭发布图片压缩TNG，节省55%带宽

https://mp.weixin.qq.com/s/meK8UBnVHzA9YspQ2RFp6Q

体积减半画质翻倍，他用TensorFlow实现了这个图像极度压缩模型

https://mp.weixin.qq.com/s/_5tyt7pU0gIXbkmTOVEtDw

嫌图片太大？！卷积神经网络轻松实现无损压缩到20%！

https://mp.weixin.qq.com/s/a4oU8UK_hLMrKXNRQizAag

图鸭科技获CVPR 2018图像压缩挑战赛单项冠军，技术解读端到端图像压缩框架

https://mp.weixin.qq.com/s/VDyPjzXdwMGEsoXQmhrp9g

图鸭科技斩获CVPR图像压缩挑战赛冠军，TNGcnn4p技术全解读

https://mp.weixin.qq.com/s/B7reSwa9sCZqbkYVM5-VOA

图像压缩哪家强？请看这份超详细对比

https://mp.weixin.qq.com/s/K17wlC3tueNBfHkYBUFcQg

基于深度学习的HEVC复杂度优化。这是篇视频压缩的blog。

https://mp.weixin.qq.com/s/exUYS2v5VyRaMdFylWlobw

用循环神经网络进行文件无损压缩：斯坦福大学提出DeepZip

# 深度贝叶斯学习

https://mp.weixin.qq.com/s/pHAbxeYBI2q6pUHNrAt1og

贝叶斯学习与未来人工智能

https://mp.weixin.qq.com/s/Zd4rFU7Lebr4zmzxThNyVw

详解珠算：清华大学开源的贝叶斯深度学习库

https://mp.weixin.qq.com/s/RpaOrngeXTKycLb3iCygZw

利用贝叶斯神经网络进行随机动力系统中的学习与策略搜索

https://mp.weixin.qq.com/s/lKm_ypn5I7tSjoQHceJ0jQ

概率编程：使用贝叶斯神经网络预测金融市场价格

https://mp.weixin.qq.com/s/cDqxmRVQCIqdM5oiUh82YQ

Yee Whye Teh：《贝叶斯深度学习与深度贝叶斯学习》

https://mp.weixin.qq.com/s/Zk2YG-IJNhJxTBU8THSM-g

让DL可解释？这一份66页贝叶斯深度学习教程告诉你

https://mp.weixin.qq.com/s/-izo9VUdxN33pwVFGV_tjw

299页PPT带你回顾深度贝叶斯学习最新发展脉络

https://github.com/bayesgroup/deepbayes-2018

Seminars DeepBayes Summer School 2018

https://mp.weixin.qq.com/s/WCRYppBLdl_M4etUChnfgw

PyMC3和Theano代码构建贝叶斯深度网络

https://mp.weixin.qq.com/s/7mwJpQFWWXJ3dvTAwDFI7Q

贝叶斯卷积神经网络：架起深度学习与统计学的桥梁

https://mp.weixin.qq.com/s/2LkpuchuHs82Sxs5rD8bWA

《深度贝叶斯与序列学习》，279页PPT带你知晓深度贝叶斯序列模型在NLP最新进展

# Spiking Neuron Networks

除了基于BP算法的NN之外，Spiking Neuron Networks也是一大类NN。Spiking NN和人脑结构更相似，功耗也更小，但是相关训练和数据量化的算法尚不成熟，属于潜力股。

![](/images/img2/BrainChip_Fig2.gif)

参考：

https://homepages.cwi.nl/~sbohte/publication/paugam_moisy_bohte_SNNChapter.pdf

Computing with Spiking Neuron Networks

https://mp.weixin.qq.com/s/6dpKSaLFVo-ge4gtbG8GQg

简述脉冲神经网络SNN：下一代神经网络

https://mp.weixin.qq.com/s/0n50YO1jIv_mxqe0EeS6kw

综述AI未来：神经科学启发的类脑计算

https://mp.weixin.qq.com/s/5KA7jtlRmnXxijGQhU1k4A

DeepMind哈萨比斯狂推的神经科学，入门需要看什么书？

https://mp.weixin.qq.com/s/TWdeHVCgEf54STvdA1QUPg

DeepMind哈萨比斯长文：伟大的AI离不开神经科学

https://mp.weixin.qq.com/s/8ibcyvyBLYArAMhQElqRzg

Cell研究揭示生物神经元强大新特性，是时候设计更复杂的神经网络了！

https://mp.weixin.qq.com/s/cb6JBlb11xW0Xw0RWI4vFA

浙大&川大提出脉冲版ResNet：继承ResNet优势，实现当前最佳

https://mp.weixin.qq.com/s/yaAuVpuhSGabOswKnv9q5Q

脉冲神经网络与小样本学习

# BERT

论文：

《BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding》

代码：

https://github.com/google-research/bert

BERT算的上是Google暴力美学的新作了。如果用家用显卡GTX 1080Ti的话，大概需要几个月的训练时间。幸好Google已经提供了预训练的模型：

https://github.com/google-research/bert/blob/master/multilingual.md

这里有一个使用预训练模型的参考代码：

https://github.com/macanv/BERT-BiLSMT-CRF-NER

## ELMo

https://mp.weixin.qq.com/s/I315hYPrxV0YYryqsUysXw

NLP的游戏规则从此改写？从word2vec, ELMo到BERT

https://mp.weixin.qq.com/s/VL09dIQE6kAUzj5-CRFoXA

ELMo的朋友圈：预训练语言模型真的一枝独秀吗？

https://mp.weixin.qq.com/s/1nZV6wXzeIxIAAovFOageA

手把手教你用ELMo模型提取文本特征

## GPT 2.0

https://mp.weixin.qq.com/s/E7FLbXYvE9irSpJ9Cdx5tg

GLUE排行榜上全面超越BERT的模型近日公布了

https://mp.weixin.qq.com/s/ZitIqX-9MNk6L1mAC_AwBQ

OpenAI发布参数量高达15亿的通用语言模型GPT-2

https://mp.weixin.qq.com/s/7u_W4LTYqQBmz3geux5QNQ

对标Bert？刷屏的GPT 2.0意味着什么

## 参考

https://www.zhihu.com/question/298203515

如何评价BERT模型？

https://mp.weixin.qq.com/s/Fao3i99kZ1a6aa3UhAYKhA

全面超越人类！Google称霸SQuAD，BERT横扫11大NLP测试

https://mp.weixin.qq.com/s/INDOBcpg5p7vtPBChAIjAA

最强预训练模型BERT的Pytorch实现

https://mp.weixin.qq.com/s/SZMYj4rMneR3OWST007H-Q

解读谷歌最强NLP模型BERT：模型、数据和训练

https://mp.weixin.qq.com/s/8uZ2SJtzZhzQhoPY7XO9uw

详细解读谷歌新模型BERT为什么嗨翻AI圈

https://mp.weixin.qq.com/s/CofeiL4fImq98UeuJ4hWTg

预训练BERT，官方代码发布前他们是这样用TensorFlow解决的

https://mp.weixin.qq.com/s/vFdm-UHns7Nhbmdoiu6jWg

谷歌终于开源BERT代码：3亿参数量，机器之心全面解读

https://mp.weixin.qq.com/s/dV4RkxZOC9o2BxNi0GljKQ

谷歌最强NLP模型BERT官方中文版来了！多语言模型支持100种语言

https://mp.weixin.qq.com/s/fz-bQMAi5bs2_bvRhf3ERg

从Word Embedding到Bert模型—自然语言处理中的预训练技术发展史

https://mp.weixin.qq.com/s/pD4it8vQ-aE474uSMQG0YQ

两行代码玩转Google BERT句向量词向量

https://mp.weixin.qq.com/s/osmUZxAAX3x-oTHYJbzemA

谷歌BERT模型fine-tune终极实践教程

https://mp.weixin.qq.com/s/XmeDjHSFI0UsQmKeOgwnyA

小数据福音！BERT在极小数据下带来显著提升的开源实现

https://mp.weixin.qq.com/s/HXYDO5PM8UIoXgEPGe8p-w

图解当前最强语言模型BERT：NLP是如何攻克迁移学习的？

https://mp.weixin.qq.com/s/zz3j9HEuzw5e92MQXxSQsA

遗珠之作？谷歌Quoc Le这篇NLP预训练模型论文值得一看

https://mp.weixin.qq.com/s/k_33UK1RkMyHn6TSudU6Kg

详解谷歌最强NLP模型BERT

https://mp.weixin.qq.com/s/IN4YfoZnlBozwEFdhSvLZg

用可视化解构BERT，我们从上亿参数中提取出了6种直观模式

https://mp.weixin.qq.com/s/nIT3GIU0dUIYyGChxsiOWw

Google BERT应用之《红楼梦》对话人物提取

https://mp.weixin.qq.com/s/dcp_ANYijRmicMYX7OpJmA

如何用最强模型BERT做NLP迁移学习？

https://mp.weixin.qq.com/s/DR4SkgOfUT7KYiaXm5NynQ

跨语言版BERT：Facebook提出跨语言预训练模型XLM

https://mp.weixin.qq.com/s/epjjHmlmMFhWtRO_cCUITA

用BERT进行多标签文本分类

https://mp.weixin.qq.com/s/Wk6gvOS_Qnud6ib1esMFXA

加入Transformer-XL，这个PyTorch包能调用各种NLP预训练模型！

https://mp.weixin.qq.com/s/GqqU3Ixht1BzMnQeRYQEqQ

谷歌NLP深度学习模型BERT特征的可解释性表现怎么样？

https://mp.weixin.qq.com/s/2f91Ksj19rk_emoFpEmPfA

从BERT看大规模数据的无监督利用

https://mp.weixin.qq.com/s/g6-NjoFMPpxjsh38X-wTFQ

BERT，GPT-2这些顶尖工具到底该怎么用到我的模型里?

https://mp.weixin.qq.com/s/N6xBFZ82dkSGCbj6vC5nLQ

上下文预训练模型最全整理：原理、应用、开源代码、数据分享

https://mp.weixin.qq.com/s/-6XpuO7_ve_EdSPCMeWE7g

Attention isn’t all you need！BERT的力量之源远不止注意力

https://mp.weixin.qq.com/s/Y2bs2QegRadSR7lbiFFnWg

BERT一作Jacob Devlin斯坦福演讲PPT：BERT介绍与答疑

https://zhuanlan.zhihu.com/p/58425003

从Word2Vec到Bert，聊聊词向量的前世今生（一）

https://mp.weixin.qq.com/s/xoQhz6ljbsbzKRBJlTQQuQ

百度提出ERNIE，多项中文NLP任务表现出色

https://mp.weixin.qq.com/s/SfMIKfF_B4agFCHN_U_mzQ

BAM！利用知识蒸馏和多任务学习构建的通用语言模型

https://mp.weixin.qq.com/s/6G5Mu7-1omGtQ_9Gt9lUBw

基于预训练自然语言生成的文本摘要方法

https://mp.weixin.qq.com/s/yysnPauB22YgprpOi1ZWSQ

深入理解BERT Transformer，不仅仅是注意力机制

https://mp.weixin.qq.com/s/kFABJJ3fBC48-4DXK8PERQ

10大任务超越BERT，微软提出多任务深度神经网络MT-DNN

https://mp.weixin.qq.com/s/jlGfxkT_o9sgFlUuR_x5Tw

微软开源用于学习通用语言嵌入的MT-DNN模型

https://mp.weixin.qq.com/s/D68YzjYvpc2epGWFBP6rIQ

谷歌实习生新算法提速惊人！BERT训练从三天三夜，缩短到一个小时

https://mp.weixin.qq.com/s/iDGofh_ycWJzfqQriPEXGQ

如何用Python和BERT做中文文本二元分类？

https://zhuanlan.zhihu.com/p/62308732

浅谈Bert：语言理解中的预训练编码器

https://mp.weixin.qq.com/s/1Cz6js4kYdvc8g4oKjVPeA

BERT烹饪之法：fintune的艺术

https://mp.weixin.qq.com/s/nVM2Kxc_Mn7BAC6-Pig2Uw

BERT模型的标准调优和花式调优

https://mp.weixin.qq.com/s/FwmEIZ3ugeZBbLIGHmH-_g

BERT之后，GLUE基准升级为SuperGLUE：难度更大

https://mp.weixin.qq.com/s/SDVxn3Ra1dKmr-XgKNg5IA

罗玲：From Word Representation to BERT

https://mp.weixin.qq.com/s/-bh8BL4LxnevS8xnW5U9ZA

中科院自动化所提出BIFT模型：面向自然语言生成，同步双向推断

https://mp.weixin.qq.com/s/7yCnAHk6x0ICtEwBKxXpOw

序列到序列自然语言生成任务超越BERT、GPT！微软提出通用预训练模型MASS
