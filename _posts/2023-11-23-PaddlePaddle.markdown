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

# 欧洲+

1930年8月29日，因拖欠工资和工作福利问题，在与柏林党部书记戈培尔谈判无果后，柏林冲锋队在冲锋队东区队长瓦尔特·施滕纳斯的率领下在8月30日捣毁了纳粹党的柏林党部，同时还把戈培尔的家砸了。

希特勒被迫亲自赶往现场安抚，对2000名冲锋队员发表演讲称自己一直和他们站在一起，还与施滕纳斯拥抱。

1931年4月4日，纳粹党报《人民观察家》刊登署名文章，宣布瓦尔特·施滕纳斯“从未信奉社会主义、向党员传播有毒思想”，是“潜藏在党内的布尔乔亚、阴谋反对社会主义”。纳粹认为他们才是社会主义，苏联是犹太人操纵的骗局。

施滕纳斯后来被戈林保了，派到中国，德国顾问团返回的时候，知道回去没有好果子吃，留华至1949年，返回西德，1983年在吕登沙伊德离世。

---

1431年，教皇尤金四世当时陷入资金困境，向佛罗伦萨公爵借了2.5万个金币，几年过后还不上钱，于是只能决定割让土地来还钱抵债。双方于1440年一致同意把新边界设在里约河（Rio）。

问题是意大利有好几千条里约河，教皇也没跟佛罗伦萨公爵说清楚是哪一条……就在他们准备画地图的国境线上，就有两条都叫里约河的河流。佛罗伦萨人认为是a里约河，梵蒂冈认为是b里约河，由于两边都是厚道人，两条里约河之间的领土就这样莫名其妙的变成了无主之地。

https://www.zhihu.com/answer/1974885316646178886

科斯帕亚无政府区

---

捷克人被威胁称，若不签署投降文件，他们将面临第三帝国的制裁，甚至可能目睹心爱的首都布拉格在黎明前被炸弹夷为平地。尽管哈查恳求希特勒允许他的人民过上民族独立的生活，但这些请求被置之不理。四个德国人围着桌子，不断将文件塞给捷克人，同时用笔威胁他们。希特勒甚至警告说，每一个试图阻止德国入侵的捷克人都将面临德国的强大打击。

面对如此巨大的压力，哈查在会议期间两次晕倒，但都被希特勒的私人医生救醒。最终，在凌晨4点，哈查无奈地签署了匆忙翻译成捷克语的投降书，被迫充当纳粹的傀儡，直到红军占领该国后，他被投入布拉格监狱，并于1945年6月27日离世。

---

在神圣罗马帝国，司法决斗甚至延伸到了家庭矛盾。

若妻子指控丈夫谋杀，且无证人，法庭会判决夫妻决斗。

但为了男女公平，丈夫必须陷在齐腰深的土坑里且绑住一只手，妻子则在地面上用装有重石的布袋发动攻击。

这场决斗绝非 “夫妻吵架打闹”，而是真正的生死局。

如果妻子用石袋把坑里的丈夫打晕或打死，妻子获胜，指控成立。

如果丈夫设法用短棍将妻子拖入土坑，或者让妻子倒地无法动弹，丈夫获胜。

战败的一方即使没在场上被打死，也会被立刻由法警带走执行死刑，战败的丈夫会被活埋，战败的妻子则会被送上火刑柱烧死。

1842年，林肯凭借自己193公分（6英尺4英寸）的“奇葩身高”和超长手臂，在手枪和长刀之间选择冷兵器决斗，靠着一寸长一寸强，硬生生把167公分（5英尺6英寸）的对手吓到认输。

https://www.zhihu.com/answer/2058886538805752015

司法决斗
