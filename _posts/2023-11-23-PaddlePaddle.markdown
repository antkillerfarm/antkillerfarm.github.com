---
layout: post
title:  PaddlePaddle, Huggingface, Pytorch（三）
category: DL Framework 
---

* toc
{:toc}

# PaddlePaddle

https://mp.weixin.qq.com/s/BRMTR688FcQyUxR8rxwH2Q

如何用百度深度学习框架PaddlePaddle做数据预处理

https://mp.weixin.qq.com/s/yTZs_rQnuZK1GAy3V1Cuog

一文读懂百度PaddlePaddle EDL技术

https://mp.weixin.qq.com/s/VKK4M_0S7HaBkvURqWgseQ

PaddlePaddle与TensorFlow的对比分析

https://mp.weixin.qq.com/s/ync8iu8nmpJoI5Sfnj8DqQ

百度深度学习平台PaddlePaddle框架解析

https://mp.weixin.qq.com/s/dGapoc3YUkzjItrTUi3DQg

新手入门？一步一步教你如何安装PaddlePaddle

https://mp.weixin.qq.com/s/tAhaVwlsxJ5ml0wxiew_4Q

在PaddlePaddle上实现MNIST手写体数字识别

https://mp.weixin.qq.com/s/nGXhXvEd8mXbpTqmFHNJcQ

说说为什么选择Paddlepaddle而不是TensorFlow

https://mp.weixin.qq.com/s/G8hUNK5hyZPDHt9JRROMKw

PaddleFluid和TensorFlow基本使用概念对比

https://mp.weixin.qq.com/s/RuVtfoGDdbWSBBfN-A0OOw

使用PaddleFluid和TensorFlow实现图像分类网络SE_ResNeXt

https://mp.weixin.qq.com/s/JULU6bO7sPUbEJZ9tUDqiQ

使用PaddleFluid和TensorFlow训练RNN语言模型

https://mp.weixin.qq.com/s/ceCopgMboWvNxl8qgmeB-Q

使用PaddleFluid和TensorFlow训练序列标注模型

https://mp.weixin.qq.com/s/PjwsfdlwZMRvuprkmi-saQ

云脑科技-实习僧文本匹配模型及基于百度PaddlePaddle的应用

https://mp.weixin.qq.com/s/oC1ZMRNrJK9oY4y6oHxzbA

百度PaddlePaddle的新特性与大规模稀疏数据分布式模型训练

https://mp.weixin.qq.com/s/ZV32xDgGTriIAUbHohc90Q

4个学生，1个暑假，百度深度学习开源平台PaddlePaddle诞生了智能桃子分拣器

https://mp.weixin.qq.com/s/m9Kh4Syo5c6a0aAW0a7kAw

基于深度学习的图像超分辨率重建

https://mp.weixin.qq.com/s?__biz=MzIwMTc4ODE0Mw==&mid=2247492226&idx=1&sn=eafb23c1658f487f47254128bcc6e1b2

PyraNet：基于特征金字塔网络的人体姿态估计

https://mp.weixin.qq.com/s/QBI9C_2CKLTvR0SoDdF27g

百度PaddlePaddle开源视频分类模型Attention Cluster

https://mp.weixin.qq.com/s/e63adoVJ5M_LalMCbB6CIQ

PARL源码走读：使用策略梯度算法求解迷宫寻宝问题

https://mp.weixin.qq.com/s/n9ZJEJQqyrgNZr5zJbnS8g

飞桨上线万能转换小工具，教你玩转TensorFlow、Caffe等模型迁移

https://mp.weixin.qq.com/s/1jtO1efF2UauaUZFDRzQHw

比Pytorch Hub更早？三分钟带你弄懂PaddleHub！

https://mp.weixin.qq.com/s/gfJKmoeR6n3m89r4F3R_yw

PaddleDetection物体检测统一框架

# Huggingface

Hugging face起初是一家总部位于纽约的聊天机器人初创服务商，他们本来打算创业做聊天机器人，然后在github上开源了一个Transformers库，虽然聊天机器人业务没搞起来，但是他们的这个库在机器学习社区迅速大火起来。目前已经共享了超100,000个预训练模型，10,000个数据集，变成了机器学习界的github。

官网：

http://www.huggingface.co

国内镜像：

https://hf-mirror.com

它的国内竞品有：modelscope

Huggingface提供的工具库中比较知名的有Accelerate和Optimum。Accelerate专注于开箱即用的分布式训练，而Optimum作为Transformer的扩展，通过利用用户目标硬件的最大效率来加速模型训练和推理。

Optimum又是另一开源项目的封装：

https://onnxruntime.ai/docs/performance/transformers-optimization.html

---

Huggingface的transformers项目，为该网站HUB下的模型提供了支撑，成为了目前最流行的transformers专用框架。

代码：

https://github.com/huggingface/transformers

---

Safetensors是Huggingface提出的一种模型格式，旨在解决ONNX格式的不足，特别关注模型安全性、隐私保护和快速加载。

代码：

https://github.com/huggingface/safetensors

---

tokenizers的含义参见《Tokenization》一节。

代码：

https://github.com/huggingface/tokenizers

参考：

https://zhuanlan.zhihu.com/p/591335566

Huggingface详细教程之Tokenizer库

---

transformers库里的llama模型：

huggingface_transformers/src/transformers/models/llama/modeling_llama.py

```python
self.layers = nn.ModuleList(
    [LlamaDecoderLayer(config, layer_idx) for layer_idx in range(config.num_hidden_layers)]
)
```

nn.Sequential里面的模块按照顺序进行排列的，所以必须确保前一个模块的输出大小和下一个模块的输入大小是一致的。而nn.ModuleList并没有定义一个网络，它只是将不同的模块储存在一起，这些模块之间并没有什么先后顺序可言。因此后者必须实现自己的forward函数。

---

```python
class LlamaForCausalLM(LlamaPreTrainedModel)
class LlamaModel(LlamaPreTrainedModel)
class LlamaDecoderLayer(nn.Module)
class LlamaPreTrainedModel(PreTrainedModel)
class PreTrainedModel(nn.Module, ModuleUtilsMixin, GenerationMixin, PushToHubMixin, PeftAdapterMixin)
class GenerationMixin
def generate(..., assistant_model: Optional["PreTrainedModel"] = None, ...)
result = self._assisted_decoding
```

---

Hugging face推出的diffusion models的框架。

代码：

https://github.com/huggingface/diffusers

---

参考：

https://zhuanlan.zhihu.com/p/535100411

Huggingface超详细介绍

https://mp.weixin.qq.com/s/yRn1AELGK3jgfOaaxIo67Q

Huggingface简介及BERT代码浅析

https://mp.weixin.qq.com/s/-fQ9VlaOjssDufcZuCe91w

结合HuggingFace代码浅析Transformer

https://zhuanlan.zhihu.com/p/650438817

GPU分布式训练推理——Accelerate

https://zhuanlan.zhihu.com/p/610800518

Optimum + ONNX Runtime: 更容易、更快地训练你的Hugging Face模型

# xFormers

xFormers是Meta推出的基于PyTorch的深度学习库，专注于Transformer架构的优化和应用。

代码：

https://github.com/facebookresearch/xformers

# Pytorch

https://mp.weixin.qq.com/s/yGtDUbvO2APT88MwcSh8IA

GAN如此简单的PyTorch实现，一张脸生成72种表情

https://mp.weixin.qq.com/s/u9GEDCmR-PT0--0Xf4vKDA

Pytorch的tensorboard食谱帮你可视化误差结果

https://mp.weixin.qq.com/s/HginBrMOfEEWsZKq67u6EA

在Pytorch中构建流数据集

https://zhuanlan.zhihu.com/p/98535650

研究生应当掌握的并行训练方法（单机多卡）

https://zhuanlan.zhihu.com/p/86441879

pytorch多gpu并行训练

https://mp.weixin.qq.com/s/KP4etDrGlJmRAMQmR1mTJA

基于C++的PyTorch模型部署

https://mp.weixin.qq.com/s/uUxwMFGF9nJiraVQsIqu2Q

PyTorch重大更新：将支持自动混合精度训练！

https://zhuanlan.zhihu.com/p/145427849

PyTorch Parallel Training（单机多卡并行、混合精度、同步BN训练指南文档）

https://mp.weixin.qq.com/s/Y6sJhnmjRwN2uopHgr7nFA

让PyTorch更轻便，这款深度学习框架你值得拥有！

https://zhuanlan.zhihu.com/p/104019160

PyTorch常用代码段

https://mp.weixin.qq.com/s/5Nq3y8hwhQG1UV9lHuBQvA

13个你一定要知道的PyTorch特性

https://mp.weixin.qq.com/s/cKvkvWgVPuq9y1A5q1OPEQ

跟着指南学PyTorch—迁移学习教程

https://mp.weixin.qq.com/s/gJgxh4l0CXTlaJaQ_FS3YQ

PyTorch的数据增强与数据标准化

https://mp.weixin.qq.com/s/Mo7XhRcPkgurmQPJ3Zu1ug

基于PyTorch的计算机视觉框架

https://mp.weixin.qq.com/s/PWABh72t92pUOJufcmzvag

用PyTorch做深度学习实验！Facebook新框架Ax和BoTorch双双开源

https://mp.weixin.qq.com/s/LcwlCai7PMYOBwsLXPS5HA

PyTorch语义分割开源库semseg

https://mp.weixin.qq.com/s/oDYMTb9NWxVsW07FLQKA_Q

万字综述，核心开发者全面解读PyTorch内部机制

https://www.zhihu.com/question/274635237

Pytorch有什么节省内存（显存）的小技巧？

https://mp.weixin.qq.com/s/xe5zmJklT2sqn_zffmyrLg

Sharded:在相同显存的情况下使pytorch模型的参数大小加倍

https://mp.weixin.qq.com/s/maOnO_o5y19X2D-ZnLjsJA

PyTorch中的In-place操作是什么？为什么要避免使用这种操作？

https://zhuanlan.zhihu.com/p/299736532

使用PyTorch 1.6 for Android

https://mp.weixin.qq.com/s/1ugk6uI6lfWEEUvtKIfYNA

9个让PyTorch模型训练提速的技巧！

https://mp.weixin.qq.com/s/kZvdgWqk1KLi790rly3YYQ

Pytorch中的分布式神经网络训练

https://mp.weixin.qq.com/s/biHcUt55-9RfqYJ_Dg_7Tg

TorchMetrics：PyTorch的指标度量库

https://zhuanlan.zhihu.com/p/363319763

PyTorch vs LibTorch：网络推理速度谁更快？

https://mp.weixin.qq.com/s/RBclQdtaA8prvSoUUdhrEQ

机器学习的Pytorch实现资源集合

https://mp.weixin.qq.com/s/zPv-3fMy1rZwAwPqjs7oAA

Pytorch图像分类从模型自定义到测试

https://zhuanlan.zhihu.com/p/46636027

1张图学会PyTorch+TensorFlow+MXNet+TF Eager

https://mp.weixin.qq.com/s/yS9PAw926Y7AsGRW0eHG0Q

基于PyTorch的GAN框架TorchGAN：用架构级API轻松定制GAN项目

https://github.com/CVBox/PyTorchCV

一个基于pytorch的CV框架

https://mp.weixin.qq.com/s/w09hcJof80m2VGwn7SgKmQ

TorchSeg—基于PyTorch的快速模块化语义分割开源库

https://mp.weixin.qq.com/s/TsR-jgO2c2-dbqnk1mEj8w

想读读PyTorch底层代码？这份内核机制简介送给你

https://mp.weixin.qq.com/s/Lzt3LbO6lBbOebNV1d2pLQ

迁移学习不好懂？这里有一个PyTorch项目帮你理解

https://mp.weixin.qq.com/s/7fK6GNyzYTP0fQy7F01fZw

PyTorch深度学习模型训练加速指南2021

https://mp.weixin.qq.com/s/SReuVBN8WIXFlnwho3wqgQ

最详细的Pytorch底层算子扩展总结

https://mp.weixin.qq.com/s/14_pt0_skKYNw2sAK5Zptw

Pytorch底层算子扩展最详细的总结

https://zhuanlan.zhihu.com/p/363317178

模型转换：由Pytorch到TFlite

https://mp.weixin.qq.com/s/Mj7xI5rFTxKaXswYi9_mRQ

我的PyTorch模型比内存还大，怎么训练呀？

https://mp.weixin.qq.com/s/TjCUCbXL3oSgJNYoEltgrg

7个提升PyTorch性能的技巧

https://zhuanlan.zhihu.com/p/275755543

一款全平台轻量级pytorch推理框架Msnhnet

https://mp.weixin.qq.com/s/MJgQqwWa4wyNgtKZaX_ADQ

Tensorboard可视化与Hook机制

https://mp.weixin.qq.com/s/jnV_4REXOR-ema1kOC95Nw

跨越重重“障碍”，我从PyTorch转换为了TensorFlow Lite

https://mp.weixin.qq.com/s/Svh27YIG2jYWqXwhhEuoSw

使用Pytorch和BERT进行多标签文本分类

https://mp.weixin.qq.com/s/_3iJCO4gQz7mWcW7G8kimQ

Pytorch实现卷积神经网络训练量化（QAT）

https://mp.weixin.qq.com/s/HQnI8rzPvZN6Q_5c8d1nVQ

唯快不破：基于Apex的混合精度加速

https://mp.weixin.qq.com/s/NupSd4e01cvQ3CRnjy1npw

超原版速度110倍，针对PyTorch的CPU到GPU张量迁移工具开源

https://zhuanlan.zhihu.com/p/87572724

一文看懂align_corners
