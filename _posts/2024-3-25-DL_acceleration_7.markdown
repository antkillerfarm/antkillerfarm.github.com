---
layout: post
title:  深度加速（七）——模型压缩与加速进阶（2）
category: DL acceleration 
---

* toc
{:toc}

# 模型压缩与加速进阶

https://mp.weixin.qq.com/s/uXbLb5ITHOU0dZRSWNobVg

算力限制场景下的目标检测实战浅谈

https://mp.weixin.qq.com/s/DoeoPGnS88HQmxagKJWLlg

小米开源FALSR算法：快速精确轻量级的超分辨率模型

https://mp.weixin.qq.com/s/wT39oUWfrQK-dg7hGXRynQ

实时单人姿态估计，在自己手机上就能实现

https://mp.weixin.qq.com/s/RVsXUnAJ2f0Cby7BPaWifA

人物属性模型移动端实验记录

https://mp.weixin.qq.com/s/yCcK6UJqm850HON7xU3R6g

模型压缩重要方向-动态模型，如何对其长期深入

https://zhuanlan.zhihu.com/p/93020471

轻量型网络：IdleBlock

https://mp.weixin.qq.com/s/AjuTXFmxHYdUUqodSpP_4w

10倍加速！爱奇艺超分辨模型加速实践

https://mp.weixin.qq.com/s/rzv8VCAxBQi0HsUcnLqqUA

处理移动端传感器时序数据的深度学习框架：DeepSense

https://mp.weixin.qq.com/s/UYk3YQmFW7-44RUojUqfGg

上交大ICCV：精度保证下的新型深度网络压缩框架

https://mp.weixin.qq.com/s/ZuEi32ZBSjruvtyUimBgxQ

揭秘支付宝中的深度学习引擎：xNN

https://mp.weixin.qq.com/s/0KlnQ8UUxpyhBRdeo0EOAA

用于网络压缩的滤波器级别剪枝算法ThiNet

https://mp.weixin.qq.com/s/FvR6loJ8KUxm7qwclestcQ

专门为卷积神经网络设计的训练方法：RePr

https://mp.weixin.qq.com/s/67GSnZnJySFrCESvmwhO9A

论文解读Channel pruning for Accelerating Very Deep Neural Networks

https://mp.weixin.qq.com/s/Lkxc_9sbRY157sMWaD5c7g

视频分割在移动端的算法进展综述

https://mp.weixin.qq.com/s/ie2O5BPT-QxTRhK3S0Oa0Q

剪枝需有的放矢，快手&罗切斯特大学提出基于能耗建模的模型压缩

# NN Quantization+

## 二值神经网络（续）

https://zhuanlan.zhihu.com/p/431680710

谈谈BNN二值化神经网络的设计，以及几代学界工作的演进 -（1）架构与原理

https://zhuanlan.zhihu.com/p/433429767

谈谈BNN二值化神经网络的设计，以及几代学界工作的演进 -（2）二值训练

https://zhuanlan.zhihu.com/p/435285316

谈谈BNN二值化神经网络的设计，以及几代学界工作的演进 -（3）二值化设计法则、推理框架与发展潜力

https://mp.weixin.qq.com/s/lVja7woyFWpmr9sH0CitAA

BMXNet：基于MXNet的开源二值神经网络实现

https://mp.weixin.qq.com/s/naDk0mmxd08dNl9LawLUnw

不使用先验知识与复杂训练策略，从头训练二值神经网络！

## 参考

https://mp.weixin.qq.com/s/Xvlxs-Os2meduHrEQFc7vg

第一次胜过MobileNet的二值神经网络，-1与+1的三年艰苦跋涉

https://mp.weixin.qq.com/s/Ak9Yh_MBDR6i7J2rDR99eQ

低成本的二值神经网络介绍以及它能代替全精度网络吗?

https://mp.weixin.qq.com/s/tbRj5Wd69n9gvSzW4oKStg

异或神经网络

https://mp.weixin.qq.com/s/XzLJzfvpP93cDYplf6-LXA

港科腾讯等提出Bi-Real net：超XNOR-net 10%的ImageNet分类精度

https://mp.weixin.qq.com/s/wCx7rQFwC2mW45FMR77tGQ

二值网络，围绕STE的那些事儿

https://mp.weixin.qq.com/s/7L26ghhDqdMU6LRV0iD6vQ

模型量化从1bit到8bit，二值到三值

https://mp.weixin.qq.com/s/M79xGWWtJUB6wBVlHXw8ig

低精度神经网络：从数值计算角度优化模型效率

https://www.chiphell.com/thread-1620755-1-1.html

新Titan X的INT8计算到底是什么鬼

https://mp.weixin.qq.com/s/5LhLbzyWTlP2R_zGAIKuiA

INT8量化训练

https://mp.weixin.qq.com/s/S9VcoS_59nbZWe_P3ye2Tw

减少模型半数内存用量：百度&英伟达提出混合精度训练法

https://zhuanlan.zhihu.com/p/35700882

CNN量化技术

https://mp.weixin.qq.com/s/9DXMqiPIK5P5wzUMT7_Vfw

基于交替方向法的循环神经网络多比特量化

https://mp.weixin.qq.com/s/PDeChj1hQqUrZiepxXODJg

ICLR oral：清华提出离散化架构WAGE，神经网络训练推理合二为一

https://mp.weixin.qq.com/s/KgM1k1bziLTCec67hQ8hlQ

超全总结：神经网络加速之量化模型

https://mp.weixin.qq.com/s/7dzQhgblEm-kzRnpddweSw

嵌入式端CNN网络计算的量化-动态定点法（1）

https://mp.weixin.qq.com/s/M3NcH30zY5Wlj76BDPQlMA

模型压缩一半，精度几乎无损，TensorFlow推出半精度浮点量化工具包，还有在线Demo

https://www.zhihu.com/question/498135156

如何看待FAIR提出的8-bit optimizer：效果和32-bit optimizer相当？

https://mp.weixin.qq.com/s/D3ZKidCV7OhAeqWqWg521w

如何训练和部署FP16/Int8等低精度机器学习模型?

https://jackwish.net/neural-network-quantization-introduction-chn.html

神经网络量化简介

https://mp.weixin.qq.com/s/70GuFnJGhtIZEA-PECHjaA

混合精度对模型训练和推理的影响

https://mp.weixin.qq.com/s/xIbF3rNv2mC2G4RBDhIvJQ

哈佛大学在读博士：模型量化——更小更快更强

https://zhuanlan.zhihu.com/p/128018221

8比特数值也能训练模型？商汤提出训练加速新算法

https://zhuanlan.zhihu.com/p/132561405

模型量化了解一下？

https://mp.weixin.qq.com/s/xnszH9WSKGBwqtHUuYua1g

混合精度训练，提速，减内存

https://mp.weixin.qq.com/s/YImszcJDsvw5ygo2wCj3Hw

模型量化的核心技术点有哪些，如何对其进行长期深入学习

https://mp.weixin.qq.com/s/bK0n9u6DIl4SY7mxS8CVRw

模型量化技术原理及其发展现状和展望

https://zhuanlan.zhihu.com/p/223018242

NNIE量化算法及实现

https://zhuanlan.zhihu.com/p/79744430

Tensorflow模型量化(Quantization)原理及其实现方法

https://mp.weixin.qq.com/s/du3hb2oM5X6bMocdOab4dg

模型量化: 只有整数计算的高效推理

https://mp.weixin.qq.com/s/7Si6GQlj8IvYajoVnwm5DQ

INT4量化用于目标检测

https://mp.weixin.qq.com/s/7VEiQ0y8kB4nODtLCx1UQA

模型量化打怪升级之路

https://mp.weixin.qq.com/s/TXWdx3bbBNfaG3yp2G56ew

提速还能不掉点！深度解析MegEngine 4 bits量化开源实现

https://www.zhihu.com/question/627484732

目前针对大模型进行量化的方法有哪些？

# 朝鲜+

80年代，看到韩国申办奥运成功，和韩国商量合办奥运，要求开幕式闭幕式在平壤举办，还要奥运收入的一半。被拒绝后派特工把韩国的客机炸了。

1983年的仰光事件，朝鲜特工在缅甸国父纪念堂里安放炸弹，炸毁纪念堂并造成大量人员伤亡。

姜民哲此后被关押在缅甸20多年，只有一个韩国人在挂记他。

1998年，韩国国家情报院副院长罗钟一在整理材料时看到了一份简报，上面说姜民哲服刑期间没有任何访客，一度有绝望情绪。

于是罗钟一拜托韩国驻缅甸大使馆人员去看望了姜民哲一次，为他带去了食物以及朝鲜半岛的消息。姜民哲沉默了很久，直到看望人员将走时才说了句：祖国为什么将我抛弃在了这里，装作不知道。如果我能出去，请带我去韩国吧。

后来缅甸政府有人事变动，就不再允许韩国大使馆去探望姜民哲，此人仿佛被世界遗忘了。

2008年姜民哲因肝癌在狱中病逝，过了很久，罗钟一向缅甸官员打听姜民哲的下落，对方将事情告之，然后说：你是第一个，也是唯一一个问及此事的人。

https://zhuanlan.zhihu.com/p/407842614

1983年仰光刺杀事件：韩国总统因迟到死里逃生、四名部长被炸身亡

---

北韩在土改后推行了最高可达40%的实物税（交公粮），不久后又将农民的土地全部夺走归于国家，农民只不过从“地主的佃户”变成了“国家的佃户”。

https://www.donga.com/cn/article/all/20150330/440400/1

重新审视65年前李承晚的“土改”

---

二将军在青年时期就阅片无数，看得多了于是觉得自己拍。故事梗概是二将军自己操刀设计的，剧本是请作家改编的。但，导演和剪辑是从南朝鲜秘密绑架来的！

---

80年代朝韩双方敌对最剑拔弩张时期，朝鲜已经开始手搓韩元计划，但南边查的严也就没闹出多大动静。

等到九十年代我国境内特别是东北和华南地区大量出现做工精美版画正规纸张稀碎的假币。这种钱一碰到水就稀烂。我们老百姓那是闻之色变，每次拿到十元以上的钱都要细细分辨生怕中招。当时警方分析可能是海对岸岛上的登子所为，所以那时候叫这种钱为台版假币。

直到后来在朝鲜边境逮到大批带着假币过来的人员才掌握朝鲜自制外汇的事。后经我外事部门抗议甚至威胁封了边境，最主要我们加快了升级第五套人民币的进程，那边手搓人民币才最终停止。

当然，同一时期朝方对外汇的需求一直很大，他还在尝试制作日元美元。美元由于做工最复杂一直没进展，日元已经小批量制作并投入市场。

---

我从未到过朝鲜，也从没有要到那里的强烈愿望。那是一个极不寻常的国家。即便是在中国，人民也享有某些基本权利；但在朝鲜，人民几乎与世隔绝。朝鲜自称是社会主义的人间天堂，事实上它是全世界治理得最差的国家之一，就连让人民温饱的基本要求，也无法达到。-- by 李光耀

---

早在朝鲜共产党组织成立之初，就有火曜派，马列派，上海派，伊市派（伊尔库茨克），北风会，汉城派等诸多派别，而共产国际多次试图帮助朝鲜成立一个统一的共产党组织都以失败告终。

---

抗美援朝时候，姥爷还跟随九兵团入朝作战过，两根手指和半个脚掌永远留在了朝鲜。

朝鲜没外汇，那个朝元也没人敢要，所以基本都是以物换物，给他粮食，他用木材煤炭矿石啥的交换。开始几年还算顺利，舅舅还带我去平壤溜达过一圈，算我这辈子唯一的出国经历。

九十年代末，出事了，莫名其妙的理由，舅舅被朝鲜扣留了，说的还挺严重，要枪毙啥啥的。最后在家里人努力之下，把人救回来了。代价是，所有的欠款一笔勾销，还搭进去百十吨粮食。

舅舅回来说的，就是朝鲜官方不想付账了，那些年做边贸的，不管是粮食纺织品，还是小五金，来了个一勺烩，能扣住人的，都会来这么一趟。姥爷那时还在世，气的好几天吃不下饭，天天把金家天团挂嘴边。

你是没在丹东，有的租车，朝鲜人过来，把车租一天，直接开回朝鲜，给的几百块人民币还是朝鲜自己印刷的。

---

跟伽耶的境遇差不多，高句丽和百济都不是和新罗、高丽一般“统合三韩”的国家。因此这两国的始祖王虽然得到朝鲜王室立庙供奉，但对于找两国后人奉祀之事，就没太大执行的必要了。

https://www.zhihu.com/answer/2792657451

国外有没有“二王三恪”一类的传统？
