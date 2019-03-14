---
layout: post
title:  深度学习（四十七）——深度图像压缩, 深度贝叶斯学习, Spiking Neuron Networks
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
