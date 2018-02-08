---
layout: post
title:  深度学习（二十五）——深度推荐系统, 手势识别
category: DL 
---

# Deep speech 2

Deep speech 2是Deep speech原班人马2015年的作品。

论文：

《Deep speech 2: End-to-end speech recognition in english and mandarin》

代码：

https://github.com/PaddlePaddle/DeepSpeech

这个官方代码是PaddlePaddle实现的，由于比较小众，所以还有非官方的代码：

https://github.com/ShankHarinath/DeepSpeech2-Keras

![](/images/img2/Deep_Speech_2.png)

不出所料，这里使用CNN代替了FC，音频数据和图像数据一样，都是局部特征很明显的数据，从直觉上，CNN应该要比FC好使。

至于多层RNN或者GRU都是很自然的尝试。论文的很大篇幅都是各种调参，也就是俗称的“深度炼丹”。

论文附录中，如何利用集群进行分布式训练，是本文的干货，这里不再赘述。

# WaveNet

WaveNet是DeepMind 2016年的作品，主要用于语音合成，也可用于语音识别。

DeepMind在WaveNet方面，按照时间顺序有3篇论文：

《WAVENET: A GENERATIVE MODEL FOR RAW AUDIO》

《Neural Machine Translation in Linear Time》

《Parallel WaveNet: Fast High-Fidelity Speech Synthesis》

代码：

https://github.com/ibab/tensorflow-wavenet

一个Tensorflow实现

https://github.com/buriburisuri/speech-to-text-wavenet

这个Tensorflow实现，利用WaveNet实现了语音识别。

![](/images/img2/WaveNet.gif)

参考：

https://www.leiphone.com/news/201609/ErWGa8fs7yR1zn2L.html

DeepMind发布最新原始音频波形深度生成模型WaveNet，将为TTS带来无数可能

https://zhuanlan.zhihu.com/p/27064536

用Wavenet做中文语音识别

https://mp.weixin.qq.com/s/-NTQG7_-GqGQWrRhiGgAQQ

详述DeepMind wavenet原理及其TensorFlow实现

http://mp.weixin.qq.com/s/0Xg_acbGG3pTIgsRQKJjrQ

历经一年，DeepMind WaveNet语音合成技术正式产品化

https://mp.weixin.qq.com/s/u1UnAuGllcWn8Ik5wDPY6w

可视化语音分析：深度对比Wavenet、t-SNE和PCA等算法

# 深度推荐系统

推荐系统一直是AI能够落地且商业前景很好的一个研究方向。自2016年以来，该方向也逐渐被DL所侵蚀，尽管目前从招聘来说，这方面的职位仍以普通ML为主。

2017年5月，我曾面试了一家电商企业。当时给我的感觉，虽然里面的工程师较早接触ML，然而知识老化现象比较严重，对最基本的神经网络知识缺乏必要的了解。这显然给了后来者一个弯道超车的好机会。

深度推荐系统的算法包括：

https://mp.weixin.qq.com/s/qwDIvXlpP5UIBTwtpqhYsg

Auto-Encoder

https://mp.weixin.qq.com/s/AqgxnfR4h1FBRmmEe6uPqQ

CDL

https://mp.weixin.qq.com/s/WlgUVf1EjpO9UGqjTJN5ww

UWRL

https://mp.weixin.qq.com/s/KII9oNg7kqfco2MngUOGAw

AutoRec

https://mp.weixin.qq.com/s/mnGuPGtdw9d1BzeNpoYYqw

DeepCoNN

https://mp.weixin.qq.com/s/lJDiP7oeiFQSEyxWt_9uBA

NFM

https://mp.weixin.qq.com/s/G4bDj4a05K0kB4IZ6IosiQ

Wide & Deep

https://mp.weixin.qq.com/s/JNGKz4-fWG4ygl7f6UkxcQ

DeepFM

## 工具

https://www.librec.net/

这是一个Java写的推荐系统。

## 参考

https://zhuanlan.zhihu.com/p/26237106

深度学习在推荐算法上的应用进展

http://i.dataguru.cn/mportal.php?mod=view&aid=11463

深度学习在推荐领域的应用

https://mp.weixin.qq.com/s/hGvQvddD3i858XSK4z08Ug

主要推荐系统算法总结及Youtube深度学习推荐算法实例概括

https://mp.weixin.qq.com/s/yHtqWJUpCIvTStKW5TINaA

Youtube短视频推荐系统变迁：从机器学习到深度学习

https://mp.weixin.qq.com/s/N1oLs-saWN_ifkWEaWw_Vg

YouTube 2016年公布的基于深度学习的推荐算法

https://mp.weixin.qq.com/s/WzSO_XobY6kesDm4sF-hBg

深度学习之推荐篇

https://mp.weixin.qq.com/s/LKjVfhyhL4GVx6l5WC6-CQ

如何用深度学习实现用户行为预测与推荐

https://mp.weixin.qq.com/s/UrMsMHAkqNHJEl5lhAvLtA

腾讯提出并行贝叶斯在线深度学习框架PBODL：预测广告系统的点击率

http://mp.weixin.qq.com/s/Jiis7j3W3D5GG_ZdxplY7Q

淘宝搜索/推荐系统背后深度强化学习与自适应在线学习的实践之路

https://mp.weixin.qq.com/s/847h4ITQMtUlZcurJ9Vlvg

深度学习在美团点评推荐平台排序中的运用

https://mp.weixin.qq.com/s/AICgNDyWASx_B8NzWcFTqA

一文综述所有用于推荐系统的深度学习方法

https://mp.weixin.qq.com/s/zSBpqhoyROh74UZEItBanA

基于概率隐层模型的购物搭配推送：阿里巴巴提出新型用户偏好预测模型

http://mp.weixin.qq.com/s/nmLNKscP1qxyv_aoSrwEEw

基于大规模图计算的本地算法对展示广告的行为预测

https://mp.weixin.qq.com/s/8hNkntUauCSeVqc2v0QUqA

人工智能如何帮你找到好歌：探秘Spotify神奇的每周歌单

https://zhuanlan.zhihu.com/p/30720579

推荐中的序列化建模：Session-based neural recommendation

https://mp.weixin.qq.com/s/vpxLTcwenvlIvj5D-8uolg

一天造出10亿个淘宝首页，阿里工程师如何实现？

https://mp.weixin.qq.com/s/lZ4FOOVIxsdKvfW45CYCnA

你看到哪版电影海报，由算法决定：揭秘Netflix个性化推荐系统

http://www.cnblogs.com/qcloud1001/p/7483362.html

深度学习在CTR中应用

http://www.cnblogs.com/qcloud1001/p/7513982.html

常见计算广告点击率预估算法总结

https://mp.weixin.qq.com/s/Q8Mt9B1rzbeWXqIInrzSYQ

使用深度学习构建先进推荐系统：近期33篇重要研究概述

https://mp.weixin.qq.com/s/PlsFxKz_Igorh94Ni-78Hg

融合MF和RNN的电影推荐系统

https://mp.weixin.qq.com/s/JKMOhpLWWlrDzymDDEldXw

深度学习大行其道，个性化推荐如何与时俱进？

https://mp.weixin.qq.com/s/FBzd0x4_A9z-r0f3ZKFGuw

携程个性化推荐算法实践

https://mp.weixin.qq.com/s/Q01jy2RtbpBBHGtvtfhEGA

当机器学习遇到推荐系统，悉尼科技大学Liang Hu博士最新分享

https://mp.weixin.qq.com/s/zBcd2vCYZvb_T7De2QhRew

京东公布基于计算机视觉的电商推荐技术！

https://mp.weixin.qq.com/s/rIZUar6sUZXo2S1JwLrHug

AI研究新利器Etymo，妈妈再也不用担心我找不到论文！ 

# 手势识别

https://zhuanlan.zhihu.com/p/26630215

浅谈手势识别在直播中的运用

https://zhuanlan.zhihu.com/p/30561160

2017-最全手势识别/跟踪相关资源大列表分享

http://www.sohu.com/a/203306961_465975

浙江大学CSPS最佳论文：使用卷积神经网络的多普勒雷达手势识别

https://www.zhihu.com/question/20131478

我打算只根据手的形状来识别手势。用哪种机器学习算法比较好？

https://www.leiphone.com/news/201502/QM7LdSN874dWXFLo.html

带你了解世界最先进的手势识别技术

