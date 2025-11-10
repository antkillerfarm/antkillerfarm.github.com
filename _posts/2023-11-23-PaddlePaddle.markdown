---
layout: post
title:  PaddlePaddle, Huggingface
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

safetensors没有重点关注性能和跨平台交换。在大模型高效序列化、数据压缩、量化等方面存在不足，并且它只保存了张量数据，没有任何关于模型的元数据信息。

GGUF把模型权重+分词器+超参数+元数据全部打包进一个.gguf文件，针对快速加载和保存模型进行了优化，使其能够高效地进行推理。GGUF由Georgi Gerganov开发，（他也是流行的 C/C++ LLM 推理框架llama.cpp的开发者）。

https://zhuanlan.zhihu.com/p/848013326

一文搞懂大模型文件存储格式新宠GGUF

---

tokenizers的含义参见《Tokenization》一节。

代码：

https://github.com/huggingface/tokenizers

参考：

https://zhuanlan.zhihu.com/p/591335566

Huggingface详细教程之Tokenizer库

---

bos_token_id：Beginning of Sequence，表示序列或句子的开始。类似的还有eos_token_id。

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
# About Model
class LlamaForCausalLM(LlamaPreTrainedModel)
class LlamaModel(LlamaPreTrainedModel)
class LlamaDecoderLayer(nn.Module)
class LlamaPreTrainedModel(PreTrainedModel)
class PreTrainedModel(nn.Module, ModuleUtilsMixin, GenerationMixin, PushToHubMixin, PeftAdapterMixin)
class GenerationMixin
def generate(..., assistant_model: Optional["PreTrainedModel"] = None, ...):
result = self._assisted_decoding
ForCausalLMLoss

# About Trainer
transformers.Trainer.train()
_inner_training_loop
```

---

Hugging face推出的diffusion models的框架。

代码：

https://github.com/huggingface/diffusers


---

huggingface出品的库的cache路径：

`~/.cache/huggingface`

---

Huggingface的datasets库，提供了加载数据集的功能。

例如json格式的数据集的加载在：

datasets/packaged_modules/json/json.py

数据一般保存为Apache Arrow支持的格式。

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

https://huggingface.co/blog/zh/pytorch-ddp-accelerate-transformers

从PyTorch DDP到Accelerate到Trainer，轻松掌握分布式训练

https://huggingface.co/blog/zh/deepspeed-to-fsdp-and-back

Hugging Face Accelerate 两个后端的故事：FSDP与DeepSpeed

# xFormers

xFormers是Meta推出的基于PyTorch的深度学习库，专注于Transformer架构的优化和应用。

代码：

https://github.com/facebookresearch/xformers

# USA+

为什么绝大多数来自国外的宣传，似乎只提到了海伦·凯勒的人生前20年？从1880年出生到1968年逝世，享年88岁高龄的海伦·凯勒，人生后60年究竟发生了什么？

这个问题的答案非常简单，长达60多年的后半生之所以被美国为首的西方国家隐瞒，是因为海伦·凯勒成为了一名坚定的社会主义者。

https://www.zhihu.com/question/332120833

海伦凯勒故事是真的吗?

---

完美的美国总统是卡特的道德、里根的人望和尼克松的智力。但现在的小布什是尼克松的道德、里根的智力和卡特的人望。

---

Cnn报道，自从特朗普胜选以后海湖庄园每天都是人山人海，大量的人在祈求特朗普能给个一官半职，也有金主说客在影响特朗普选举内阁成员。

就连马斯克胜选以后，也从未离开海湖庄园，每天陪特朗普打高尔夫球，讨论内阁成员名单。

Ps：马斯克俨然一副十三爷的排头，美利坚常务副皇帝了。

蓬佩奥身为MAGA领导干部，丧失理想信念，背弃初心使命，对MAGA不忠诚不老实，私心膨胀，将公权力作为变现工具，在重大问题上立场不坚定，与MAGA CENTRAL不一致，甚至发表反对言论，对MAGA人民产生了恶劣的社会影响。蓬佩奥的行为严重违反纪律，构成严重职务违法并涉嫌受贿犯罪，性质严重，影响恶劣，应予严肃处理。

https://www.zhihu.com/question/3725850091

如何看待特朗普发推宣布新政府将不任用迈克·蓬佩奥与妮基·黑利？

---

英国《金融时报》报道，民主党的诸多金主，ibm、苹果等公司去年官宣退出“X”，不在这个软件上投广告，只因为马斯克支持特朗普，从而导致“x”收入大减。

随着特朗普当选，现在广告商争相涌入“X”，以换取马斯克的好感，方便搭上特朗普的快车。这些企业真是生动形象给大家展示了什么叫前倨后恭，小丑本丑。

---

国务卿不是专管外交事务的，只要不设专门部门管理的事务都由国务卿管理。

美国联邦政府最早只有三个部门——财政部、战争部和司法部。换句话说，除了财政、战争和司法，其他的一切政务都由国务卿处理，是名副其实的美国总理。

美国国务卿的权力被急剧削弱是在十九世纪末二十世纪初，作为美国行政改革的一部分，美国先后设立内政部用以管理土地和协调各州事务；设立了农业部以管理农业、森林和食品销售；设立了商务和劳工部部以管理工业、矿业、劳工和贸易。

---

特赦所赦免的是司法责任，不是政治责任。

特赦的前提是认罪，毕竟没有罪的话怎么特赦？

以懂王为例，2021卸任的时候背了一屁股官司，当时幕僚就建议他对自己特赦，但懂王最后情愿卸任后天天跑法院都不特赦，就是因为特赦就是认罪。

---

英伟达前三的大股东：

- Vanguard (先锋集团)持有8.73%的股份，是NVIDIA的最大股东。
- BlackRock (贝莱德)持有7.48%的股份，排名第二。
- State Street (道富集团)持有3.99%的股份，位居第三。

沃尔玛的前三大股东是：先锋、贝莱德、道富。

亚马逊的前三大股东是：先锋、贝莱德、道富。

苹果的前三大股东是：先锋、贝莱德、道富。

先锋是美国养老金管理者，你这类似于问中国人，中石油、中烟草、中银行的幕后老板是谁。
