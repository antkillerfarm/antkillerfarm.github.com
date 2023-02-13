---
layout: post
title:  TF XLA（二）
category: DL Framework 
---

# XLA

## 混合backend

XLA支持混合多backend的运行，可用`tf.debugging.set_log_device_placement(True)`查看相关的设备指派信息。

设备指派主要由Placer模块负责：

https://www.cnblogs.com/deep-learning-stacks/p/9823486.html

TensorFlow中的Placement启发式算法模块——Placer

tensorflow/core/common_runtime/placer.cc

`tf.config.set_soft_device_placement(True)`能让tensorflow遇到无法用GPU跑的数据时，自动切换成CPU进行。

设备指派之后，就是根据设备指派的信息，切割计算图了。

https://www.cnblogs.com/deep-learning-stacks/p/10054529.html

TensorFlow的图切割模块——Graph Partitioner

tensorflow/core/graph/graph_partition.cc

## StreamExecutor

StreamExecutor是Google内部为并行编程模型开发的库。TensorFlow中的StreamExecutor是StreamExecutor的开源简版。

https://www.cnblogs.com/deep-learning-stacks/p/9386188.html

TensorFlow中的并行执行引擎——StreamExecutor框架

## backend优先级

`REGISTER_LOCAL_DEVICE_FACTORY(DEVICE_XLA_XXX_NPU, XlaXXXNpuDeviceFactory, 500);`

CPU的优先级是50，添加的backend的优先级只要大于50，就可以得到调度权。

## unit test

写好的backend需要测试，同时Unit Test也是编写一个backend的入门级入口。

```bash
bazel build -c dbg tensorflow/compiler/xla/tests:convolution_variants_test
./bazel-bin/tensorflow/compiler/xla/tests/convolution_variants_test_cpu --gtest_filter=*BackwardInputLowPaddingLessThanHighPadding*
```

## python/C++ wrapper

python: `pywrap_tfe.TFE_Py_Execute`

C++: `TFE_Py_Execute`

## Pytorch XLA

Pytorch官方提供了如下项目支持XLA：

https://github.com/pytorch/xla

粗看了一下，都是些上层的代码，底层直接调用TF的实现。所以如果目标硬件已经接入TF XLA接口的话，理论上不需要修改就可以跑pytorch。

## IR & pass

XLA IR一般如下所示：

```text
n121 = Identity[T=float, device=NPU:0](n98) @ n119
```

`[]`内是属性，`()`内是操作数，`@`表示要等到后面的操作数都做完了，才能开始执行。

HLO IR一般如下所示：

```text
%convolution = f32[16,24,24,6]{3,2,1,0} convolution(f32[16,1,28,28]{3,2,1,0} %transpose, f32[6,1,5,5]{3,2,1,0} %transpose.1), window={size=5x5}, dim_labels=bf01_oi01->b01f, metadata={op_type="Conv2D" op_name="sequential/conv2d/Conv2D"}
```

`[]`内是shape，`()`内是layout。

---

HLO可以有pass：`HloModulePass`

```cpp
HloPassPipeline pipeline("pass");
pipeline.AddPass<XXXPass>();
```

和TVM相仿，MatchPattern仍然是最简单的一类Pass。HLO的实现叫做`HloMatcher`。

替换可以使用`ReplaceWithNewInstruction`。

ConstFolding、AlgebraicSimplifier、HloCSE、HloDCE、TransposeFolding

上层的XLA graph也有pass：GraphOptimizer

tensorflow/core/common_runtime/graph_optimizer.cc

https://wzzju.github.io/tensorflow/xla/2021/12/23/xla-pass/

XLA Pass功能分析

## JAX

一款由谷歌团队打造（非官方发布），用于从纯Python和Numpy机器学习程序中生成高性能加速器（accelerator）代码，且特定于域的跟踪JIT编译器。

代码：

https://github.com/google/jax

文档：

https://jax.readthedocs.io/en/latest/

JAX的底层也是基于XLA的。

JAX并不是TF的替代品，它缺失了一些数据准备和调度的功能。这些功能一般可用haiku/flax提供。

RLax：这是一个基于Jax的强化学习库。

参考：

https://mp.weixin.qq.com/s/IMMdbF33ZHEz7N_XwgIhHA

试试谷歌这个新工具：说不定比TensorFlow还好用！

https://mp.weixin.qq.com/s/tZ3yWQ9--l9e81UqoUoWIQ

要替代TensorFlow？谷歌开源机器学习库JAX

https://mp.weixin.qq.com/s/eaYwiV2LZNRwzPEeOA1XFg

新星JAX ：双挑TensorFlow和PyTorch！有望担纲Google主要科学计算库和神经网络库

https://mp.weixin.qq.com/s/NhMbr_niHjSaqh2azuSaog

只知道TF和PyTorch还不够，快来看看怎么从PyTorch转向自动微分神器JAX

https://wzzju.github.io/jax/xla/2022/02/17/jax-cpp/

JAX程序转HLO执行

https://zhuanlan.zhihu.com/p/532504225

面向PyTorch用户的JAX简易教程(1): JAX介绍

https://zhuanlan.zhihu.com/p/544216783

面向PyTorch用户的JAX简易教程(2): 如何训练一个神经网络

## 参考

https://mp.weixin.qq.com/s/RO3FrPxhK2GEoDCGE9DXrw

利用XLA将GPU性能推向极限

https://mp.weixin.qq.com/s/MPI9KERDS-Al4DTBDRV04w

TensorFlow XLA工作原理简介

https://sketch2sky.com/

一个XLA方面的blog

https://tensorflow.juejin.im/performance/xla/jit.html

使用即时编译

https://blog.slinuxer.com/2019/06/tensorflow-xla

TensorFlow XLA初步接触

https://github.com/horance-liu/tensorflow-internals

电子书《TensorFlow Internals》

https://wzzju.github.io/tensorflow/xla/2021/06/12/xla-overview/

XLA编译执行原理分析

https://haosdent.gitbooks.io/tensorflow-document/content/resources/xla_prerelease.html

XLA: The TensorFlow compiler framework

# TensorFlow参考+

https://mp.weixin.qq.com/s/nnjyR4XGVZQ1zXCIPzTNlg

基于TensorFlow的变分自编码器实现

https://mp.weixin.qq.com/s/iMgesGmdb7Jq4muCxb-nFA

Tensorflow实战：Discuz验证码识别

https://mp.weixin.qq.com/s/4aJUGBpPG_6Oc5EqOmM0Iw

作为TensorFlow的底层语言，你会用C++构建深度神经网络吗？

https://github.com/yahoo/TensorFlowOnSpark

TensorFlow On Spark

https://mp.weixin.qq.com/s/7er3wNV_IhxhFDOIwNMpww

深度强化学习入门：用TensorFlow构建你的第一个游戏AI

# 俄乌战争=+

126独立旅是去年春天在赫尔松方向损失最严重的俄军部队，虽然俄罗斯官方没有公布任何具体损失的消息，但去年3月份该旅被授予“近卫”称号——这是克里姆林宫安抚伤亡惨重部队的标准流程。

---

4月7日乌克兰方面宣布在顿涅茨克干掉一架俄军最新型的ORLAN-10无人机。

好奇的乌克兰军人拆开战利品后发现，号称单价10万美元的ORLAN无人机，燃料供应系统居然使用了一个矿泉水瓶，负责照片和视频的佳能相机，被用魔术贴和胶水与无人机连接。

他们在电话中与上司汇报这一拆解结果时，他们的上校坚定地说： “这不可能，你在和我们开玩笑”，直到上校收到士兵们拍摄的照片和视频，他们的上司才认可了这一事实。

乌克兰军方把ORLAN无人机的拆解视频发给与他们合作的北约军事专家，这些专家们也惊呆了。

---

芬兰向美国雷神公司提出采购3.8亿美元的毒刺订单，科威特直接向雷神公司下单30亿美元毒刺订单，某蛙下单了11亿美元.....等等。

英国向雷神下单3亿美元的标枪订单，立陶宛向雷神下单1.25亿美元的标枪订单，巴西向雷神下单222套标枪订单金额未透露，但是应该也是数亿美元.......等等。

海马斯火箭炮更不必多说，立陶宛买5亿、爱沙尼亚买5亿，瑞士、比利时、拉脱维亚等国也纷纷下单购买。

根据媒体的说法，2022年光是标枪与海马斯火箭炮的军售就达到了280亿美元。

---

令普京意外的是，博伊科不仅没有笑脸相迎，反而政治立场开始逐渐倒向欧盟。甚至在欧盟将乌克兰列为入欧候选国以后，博伊科公开表示支持乌克兰加入欧盟。

开战后，俄罗斯曾找人来劝降维尔库尔，以为维尔库尔会箪食壶浆迎接他们，维尔库尔不仅不为之所动，反而在社交媒体怒斥劝降者是叛徒。曾经是亲俄派的维尔库尔，现在却成为了Kryvyi Rih军事管理局局长，带领当地军民反抗俄军侵略。

米哈伊洛·多布金，2006年3月至2010年3月任哈尔科夫市市长，2010年3月至2014年2月任哈尔科夫州州长。曾经是2014年前执政党地区党正式总统候选人。作为亚努科维奇派的亲俄铁杆人士，他曾经严厉批评在基辅参与集会的导致迈丹革命的亲西方示威者，并且质疑过当时亚努科维奇倒台后的过渡政府的合法性。

https://zhuanlan.zhihu.com/p/569767902

乌克兰“入俄公投”真的是民族自决吗？

https://www.163.com/dy/article/HJ964Q7F05533SBL.html

亲俄前哈尔科夫市长，在战争爆发后决定入伍参军，到前线对抗俄军

---

俄军说击毁很多海玛斯，乌克兰和美军笑而不语。其实俄军打掉的很多“海玛斯”只是乌军制作的木头模型，花几千美元做的，却引诱俄军往这上面丢价值几十万美元甚至几百万美元一发的精确导弹，搞得俄军现在很缺乏精确导弹了。

乌军制作的假“凯撒”，只是几根钢管搭接起来的，当然车是真卡车，能载着钢管到处跑，迷惑天上的俄罗斯侦察无人机，远看根本看不出破绽。

消息人士透露，乌军在战场上大量使用这类假目标，他们出动这辆假“凯撒”，后方几公里就是乌军真正的“凯撒”卡车炮，在成功吸引俄军炮兵开火后，“凯撒”随即根据反炮兵雷达获得的信息，利用其快速反应能力对俄军进行反炮兵作战，这也是俄军炮兵最近在乌军反炮兵作战下损失惨重的原因之一。

https://zhuanlan.zhihu.com/p/566523323

俄乌战况（9月20日）

---

![](/images/img5/Ukraine.jpg)

援助乌克兰的榜一大哥，原来一直是俄罗斯，美国其实只能屈尊第二。

---

在俄罗斯一直有军事游玩项目，其内容是游客花钱使用军事装备，比如坦克步战车等，碾压一下报废车，有钱的人多花个一千来元人民币还能干她娘的一炮。不过之前提供的军事装备都是T-55, PT-76等上个世纪五六十年代的，现在都快报废的装备。但是刚刚随手一划，他奶奶地竟然更新出T-80，BMP-2这种前线都缺的装备。

---

当欧美限价60美元后，像印度之类阴诈至极的“伙伴”，又趁机再度索要折扣，把油价打压到仅仅维持俄罗斯石油公司成本运作平衡的40多美元。

---

俄军现在已经分化出了各个山头，西部军区，中部军区，瓦格纳，车臣，顿涅茨克卢甘斯克，皇俄志愿兵都是山头，各有各的指挥。听不听军令看心情，出事就推锅。颇有功德林遗风。
