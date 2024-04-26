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

# 非洲+

马斯克出生和成长在南非，他的自传记录自己儿时一直被霸凌，亲身经历了南非种族变化的历史。

直到他17岁去到加拿大，才知道原来睡觉的时候不用担心莫名其妙的挨打，也不用把财物贴身藏在身体下面。这些都在他的自传里面写过。

---

禾丰粮油在一五年遭遇了几次武装洗劫，在最严重的一次是全公司被扣为人质，禾丰老板被冲锋枪指着头，禾丰的员工被绑匪开枪杀了一个，打伤几个。

瘦田无人耕，耕开有人争。

你别看不要租金免费让你种地。

收成的时候，地里的粮食还是你的吗？

https://www.zhihu.com/question/491637199

为什么非洲是土地肥沃，物产丰富的第一大洲，却经常闹饥荒呢？

---

古埃及人训练的努比亚雇佣兵、宗教信仰和管理制度深刻影响努比亚人，为自己培养了一个猛兽。在抵御喜克索斯恢复埃及法老王朝的过程中努比亚人又学到了最新的冶金、军事技能，充分发展了军事能力。到古埃及二十王朝后，政治动荡，23、24实力极度衰弱。努比亚人反噬几乎占领尼罗河上下游全境，建立古埃及25王朝，差一点替古埃及人完成完全拥有尼罗河的壮举。

![](/images/img5/Egypt.png)

https://www.zhihu.com/question/622715700

为什么中国完全拥有长江流域，埃及却没有完全拥有尼罗河流域？

https://zhuanlan.zhihu.com/p/614387681

黑非洲古代列国志：库施王国

---

在利比里亚独立后超过130年期间，利比里亚处于真辉格党专政状态，其中有108年是美裔利比里亚人组成的真辉格党连续执政。在真辉格党执政的期间，一切反对真辉格党的政党长期被禁止活动（注：但并未解散反对党）直到1980年4月真辉格党才在由总统卫队的多伊发动的军事政变中被推翻，大部分美裔利比里亚人家庭在1980年代逃往美国，政变结束了美裔利比里亚人及真辉格党对利比里亚政权的长期垄断，多伊成为第一个当地土著黑人国家元首，之后多伊利比里亚的独裁统治导致了1990年代的内战，战争使得利比里亚迅速崩溃。

https://www.zhihu.com/answer/3452400553

为什么海地和利比里亚照搬了美国的制度，却一直都没有好起来？

---

https://zhuanlan.zhihu.com/p/311321195

埃塞俄比亚，打起来了

https://www.zhihu.com/question/40795708

索马里兰是一个什么样的存在？

https://www.zhihu.com/question/33912256

索马里有多混乱？

https://zhuanlan.zhihu.com/p/367150702

刚刚总统阵亡的乍得，是个怎样的国家？

https://zhuanlan.zhihu.com/p/368174414

超级工程，撒哈拉沙漠大运河！

https://zhuanlan.zhihu.com/p/371417880

警惕，该组织已经渗透多个国家

https://zhuanlan.zhihu.com/p/375381294

14年了，美国非洲司令部为什么设在德国？

https://www.zhihu.com/question/271066736/answer/2178362363

中国在非洲修了哪些铁路？

https://zhuanlan.zhihu.com/p/455814066

非洲的眼泪，为何如此贫穷（乍得湖）

https://zhuanlan.zhihu.com/p/540600235

南苏丹，独立这11年，过得太惨了

https://m.thepaper.cn/baijiahao_14423653

法军“薮猫行动”研究

https://www.thepaper.cn/newsDetail_forward_13394840

法国马里战争的八年之殇：从闪电战到“西西弗斯困境”

https://zhuanlan.zhihu.com/p/606196452

被当做耗材一样榨干的国家（尼日尔）

https://zhuanlan.zhihu.com/p/410234239

“作茧自缚”林碧春：嫁给比自己大29岁非洲皇帝，今有苦说不出

https://www.zhihu.com/answer/1973637384

如何看待2020年以来埃塞俄比亚北部提格雷地区的武装冲突？

https://zhuanlan.zhihu.com/p/630031328

这个组合国家，为什么要换首都？（坦桑尼亚）

https://zhuanlan.zhihu.com/p/648500271

索马里为何陷入无政府战乱31年都难以自拔？

https://zhuanlan.zhihu.com/p/149513288

发生在非洲的第三次世界大战：刚果金战争

https://www.zhihu.com/question/57601726

十八世纪购买一名黑奴，相当于现在多少美元？
