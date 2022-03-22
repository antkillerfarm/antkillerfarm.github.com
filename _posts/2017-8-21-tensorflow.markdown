---
layout: post
title:  TensorFlow（一）
category: AI 
---

* toc
{:toc}

# 概述

TensorFlow是Google主导的开源深度学习库。

官网：

https://www.tensorflow.org/

代码：

https://github.com/tensorflow/tensorflow

TensorFlow提供了一个可视化的神经网络展示：

http://playground.tensorflow.org/

还有若干已经实现好的经典神经网络模型（比如Autoencoder、ResNet等）：

https://github.com/tensorflow/models/

TensorFlow的官方教程：

http://tensorflowtutorial.net/tensorflow-tutorial

教程中文版：

http://wiki.jikexueyuan.com/project/tensorflow-zh/

官方的基于TensorFlow的ML教程（原生中文版）：

https://developers.google.cn/machine-learning/crash-course/

API官方中文版：

http://tensorflow.google.cn/?hl=zh-cn

另一个API中文版：

https://www.w3cschool.cn/tensorflow_python/

TensorFlow官方中文社区论坛：

https://www.tensorflowers.cn

TensorFlow中文社区：

http://www.tensorfly.cn/

安装：

`sudo pip install tensorflow`

由于我的PC显卡不合要求，因此直接安装的是CPU版本，这也是最通用的版本。

TensorFlow技术栈：

![](/images/article/tensorflow_layer.png)

# CS 20SI

斯坦福最近专门为Tensorflow开设了一门课程：CS 20SI: Tensorflow for Deep Learning Research。

网址：

https://web.stanford.edu/class/cs20si/syllabus.html

课程的主讲Chip Huyen，一个越南妹子，目前在斯坦福读本科（大三）。应该说本科生上讲台的确是一件稀罕事，在这里为斯坦福的学术氛围点赞。

个人主页：

https://huyenchip.com

不用为课程的质量担心，Chip Huyen的师兄们客串了很多节课，Andrej Karpathy为课程设计了网页。

Chip Huyen可谓是一战成名。但也带来了烦恼，再去请教别人的时候，总有助教或同学会反问：“这些你不该早就知道了么？你不就是教这些的么？”

参见：

http://www.sohu.com/a/164277987_473283

一名在斯坦福教授TensorFlow教师的“忏悔”：我觉得自己像个骗子

---

Chip Huyen的新课：

https://stanford-cs329s.github.io/

CS 329S: Machine Learning Systems Design

# blog

http://www.jianshu.com/u/eaec1fc422e9

一个TF的blog

http://blog.csdn.net/u012436149

一个TensorFlow+PyTorch的blog

# 源代码编译

**Step 1**：安装Bazel。

参见[这里](/toolchain/2017/11/07/makefile.html#Bazel)

**Step 2**：编译TensorFlow。

```bash
./configure
# configure的时候要选择一些东西是否支持，这里建议都选N，不然后面会包错，如果支持显卡，就在cuda的时候选择y
bazel build --config=opt //tensorflow/tools/pip_package:build_pip_package # CPU only
bazel build --config=opt --config=cuda //tensorflow/tools/pip_package:build_pip_package # GPU
bazel build --config=opt --copt=-g --strip=never //tensorflow/tools/pip_package:build_pip_package # debug mode + CPU
bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg #生成wheel文件
```

configure脚本会自动选择CPU指令集优化，因此源代码编译的TensorFlow的CPU版本，肯定比pip安装的要运行的快。

注意：这里即使只编译TF for python3，也要安装python2，否则bazel脚本会出错。

bazel编译相当消耗资源，在配置低的机器上，可通过如下选项限制使用资源的数量。（最常出现的后果是内存耗尽导致的假死。）

`--jobs n --local_resources availableRAM(MB),availableCPU,availableIO`

例子：

`bazel build --jobs 2 --verbose_failures --local_resources 850,3.0,1.0 --config=opt //tensorflow/tools/pip_package:build_pip_package`

按照我的实践`--local_resources`其实用处不大，有的C++文件编译需要上GB空间，即使有约束也会突破。而`--jobs`相对好一些，一般按照每个job 2GB来估算，就可以保证TensorFlow顺利编译成功。

编好之后，可以找到`bazel-bin/tensorflow/python/_pywrap_tensorflow_internal.so`，这个文件在debug模式下，体积竟然高达8GB。。。

**Step 3**：安装TensorFlow。

```bash
sudo pip uninstall tensorflow
sudo pip install /tmp/tensorflow_pkg/tensorflow-1.3.0-cp27-cp27mu-linux_x86_64.whl
```

>安装之后，不要在tensorflow源代码文件夹下运行python，会报cannot import name 'build_info'的错误。

加入CPU指令集优化之后的版本，要比通用版快50%～100%，因此，编译源码安装还是很有价值的。

GPU编译基本和CPU编译差不多，唯一需要注意的是：Ubuntu 16.04的gcc编译器是5.4.0，然而CUDA 8.0不支持5.0以上的编译器，因此需要降级，把编译器版本降到4.9。命令如下：

`sudo apt install g++-4.9`

configure脚本会询问使用什么版本的gcc，填`/usr/bin/gcc-4.9`即可。

此外，Bazel的版本也要和其他部件一致。

具体的要求参见：

https://tensorflow.google.cn/install/source

---

2018.11

GPU编译已经没有太大意义了，只需`pip install tensorflow-gpu`即可。但需要注意的是，TF版本需要匹配对应的CUDA版本，否则安装过程不会有错误，但是运行时就会出问题。

---

2020.11

现在TF的GPU版本已经比较完善了，但是相应的Nvidia依赖的安装，仍然比较麻烦。

这里的Nvidia依赖，包括三个层面：

- Nvidia driver

https://www.nvidia.com/download/index.aspx

- CUDA

https://developer.nvidia.com/cuda-toolkit-archive

CUDA自带了特定版本的Driver，但是和driver版本并无对应关系，不使用自带的driver也是完全可以的。

- cuDNN

https://developer.nvidia.com/rdp/cudnn-archive

这个需要注册Nvidia账号才能下载。。。囧

下载解压之后，将cuda/include/cudnn.h文件复制到/usr/local/cuda/include文件夹，将cuda/lib64/下所有文件复制到/usr/local/cuda/lib64文件夹中，并添加读取权限。

Ubuntu 20.04已经自带了相应的driver和CUDA：

`sudo apt install nvidia-driver-440 nvidia-utils-440`

Pytorch可支持最新的CUDA，因此这样就可以了。

而TF不支持最新的CUDA版本，所以还需要：

`sudo apt install libcudart10.1`

cuDNN这个没办法，只有硬装。

装好之后，会有如下提示：

`successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero`

这个是正常的，NUMA是多机多卡才有的东西。

---

参考：

http://www.jianshu.com/p/b1faa10c9238

TensorFlow CPU环境SSE/AVX/FMA指令集编译

http://www.hankcs.com/ml/compile-and-install-tensorflow-from-source.html

从源码编译安装TensorFlow

http://blog.csdn.net/sinat_28731575/article/details/74633476

Mac下使用源码编译安装TensorFlow CPU版本

http://www.cnblogs.com/wangduo/p/7383989.html

Ubuntu 16.04+CUDA 8.0+cuDNN v5.1+TensorFlow(GPU support)安装配置详解

https://mp.weixin.qq.com/s/bxJ7VCI_120psBti-j376w

在ubuntu上配置tensorflow 1.7+CUDA踩过的坑

https://zhuanlan.zhihu.com/p/81724891

配置ubuntu18.04+cuda9.0+cudnn服务器tensorflow-gpu深度学习环境

https://blog.csdn.net/qq_43202953/article/details/107951031

Ubuntu 20.04安装cuda和cudnn

# 快速更新

对于要对tensorflow的C++部分进行调试的人来说，每次都重装pip包，显然是一件很麻烦的事情。这时可以考虑使用符号链接来省去打包和解包的时间。

```bash
# create soft link(do it only in the first time.)
cd <python lib path>/tensorflow/python
ln -s <tf path>/bazel-bin/tensorflow/python/_pywrap_tensorflow_internal.so _pywrap_tensorflow_internal.so

cd <python lib path>/tensorflow
ln -s <tf path>/bazel-bin/tensorflow/libtensorflow_framework.so.2.3.1 libtensorflow_framework.so.2

# <python lib path> usually like this: /usr/lib/python3.8/site-packages

# update
bazel build --config=opt --copt=-g --strip=never //tensorflow/python:pywrap_tensorflow
```

# 基本概念

**Variables**：维持计算图执行过程中的状态信息的变量。一般来说，这就是神经网络的参数。

**Placeholders**：对于每个样本都不相同的变量。比如神经网络的输入变量x和输出变量y。

声明：

`x = tf.placeholder(tf.float32, [None, 784])`

Placeholders在图的执行过程中，需要由真实的tensor填充之：

`sess.run(train_step, feed_dict={x: batch_xs, y_: batch_ys})`

这里的batch_xs就是用来填充x的tensor。

# 图计算

图计算是各个深度学习框架的中心概念，这里单独提出来讨论一下。

![](/images/article/fused_graph.png)

上图是softmax运算的计算图示例。计算图是一个有向无环图，它的结点代表原始数据和计算中间结果，边代表数据的流向。计算操作被称为**Operation**。

因此，**计算图实际上是工作流（work flow）思想在数值计算领域的拓展**。这种流水线一般被称为**Graph**。

流水线有了，还要有被加工的数据。数据在流水线上被加工的执行过程，被称为**Session**。

Graph和Session的关系，类似于类和对象的关系。Session是Graph的动态实例。

图计算的大致步骤如下：（这里以OpenVX函数为例，因为它更接近底层和硬件。）

1.vxCreateGraph。创建计算图。

2.vxProcessGraph。运行计算图。

在大多数Tensorflow示例中，你看不到Graph的身影。但它并不是不存在，而是默认所有新加入的Operation都添加到默认的Graph。

以下是使用多个Graph的示例：

```python
import tensorflow as tf
g1 = tf.Graph()
with g1.as_default():
    c1 = tf.constant([1.0])
with tf.Graph().as_default() as g2:
    c2 = tf.constant([2.0])

with tf.Session(graph=g1) as sess1:
    print sess1.run(c1)
with tf.Session(graph=g2) as sess2:
    print sess2.run(c2)
```

Tensorflow对计算图的简化，不仅在于使用默认的Graph。还在于可以只计算部分的Graph。部分Graph，也被称作Sub Graph。

以上面的softmax运算为例，如果`sess.run(add)`的话，后面的ReLU和softmax运算都不会被执行。

反过来，如果只想执行ReLU和softmax的话，则可以`sess.run(softmax, feed_dict={add: add_tensor})`。也就是把Sub Graph的output作为`sess.run`的参数，而把input作为feed_dict的参数。

虽然图计算是Tensorflow的主要使用方式，然而一般性的tensor计算（即非图计算），也是完全可行的。Tensorflow没有提供相关的API，直接使用numpy就可以了。
