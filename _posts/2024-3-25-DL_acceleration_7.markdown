---
layout: post
title:  深度加速（七）——模型压缩与加速进阶（2）
category: DL acceleration 
---

* toc
{:toc}

# 模型压缩与加速进阶

https://mp.weixin.qq.com/s/p_qdKcQwQ8y_JUw3gQUEnA

谷歌大脑用强化学习为移动设备量身定做最好最快的CNN模型

https://mp.weixin.qq.com/s/OyEIcS5o6kWUu2UzuWZi3g

这么Deep且又轻量的Network，实时目标检测

https://mp.weixin.qq.com/s/8NDOf_8qxMMpcuXIZGJCGg

Google又发大招：高效实时实现视频目标检测

https://mp.weixin.qq.com/s/IxVMMu_7UL5zFsDCcYfzYA

AutoML自动模型压缩再升级，MIT韩松团队利用强化学习全面超越手工调参

https://mp.weixin.qq.com/s/BMsvhXytSy2nWIsGOSOSBQ

自动生成高效DNN，适用于边缘设备的生成合成工具FermiNets

https://mp.weixin.qq.com/s/nEMvoiqImd0RxrskIH7c9A

仅17KB、一万个权重的微型风格迁移网络！

https://mp.weixin.qq.com/s/pc8fJx5StxnX9it2AVU5NA

基于手机系统的实时目标检测

https://mp.weixin.qq.com/s/6wzmyhIvUVeAN4Xjfhb1Yw

论文解读：Channel pruning for Accelerating Very Deep Neural Networks

https://mp.weixin.qq.com/s/-X7NYTzOzljzOaQL7_jOkw

惊呆了！速度高达15000fps的人脸检测算法！

https://mp.weixin.qq.com/s/Faej1LKqurtwEIreUVJ0cw

普林斯顿新算法自动生成高性能神经网络，同时超高效压缩

https://mp.weixin.qq.com/s/uK-HasmiavM3jv6hNRY11A

深度梯度压缩：降低分布式训练的通信带宽

https://mp.weixin.qq.com/s/_MDbbGzDOGHk5TBgbu_-oA

中大商汤等提出深度网络加速新方法，具有强大兼容能力

https://mp.weixin.qq.com/s/gbOmpP7XO1Hz_ld4iSEsrw

三星提出移动端神经网络模型加速框架DeepRebirth

https://mp.weixin.qq.com/s/rTFLiZ7DCo6vzD5O64UnMQ

阿里提出新神经网络算法，压缩掉最后一个比特

https://mp.weixin.qq.com/s/m9I5TM9uJcgZvMusO667OA

5MB的神经网络也高效，Facebook新压缩算法造福嵌入式设备

https://mp.weixin.qq.com/s/FFs0-ROvbXSAIOspW_rMbw

超越MobileNetV3！谷歌大脑提出MixNet轻量级网络

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

# CUDA+

`vectorAdd<<<4096, 256, 0, s0>>>`表示内核函数vectorAdd将在GPU上以4096个块执行，每个块包含256个线程，总共有4096x256个线程。

0表示为这个内核函数分配的动态共享内存的大小，单位是字节。s0指定关联的stream。

---

遇到cudaXX找不到：

```bash
export CPATH=/usr/local/cuda/targets/x86_64-linux/include:$CPATH
export LD_LIBRARY_PATH=/usr/local/cuda/targets/x86_64-linux/lib:$LD_LIBRARY_PATH
export PATH=/usr/local/cuda/bin:$PATH
```

---

nvcc编译cuda程序，不运行device（GPU）部分代码的解决方案：指定GPU的arch。

`nvcc ./xxx.cu -o xxx -arch sm_90 -Wno-deprecated-gpu-targets`

---

执行环境标识符：

- `__global__`：在CPU调用父函数，子函数在GPU执行(异步)。用`__global__`修饰的一般就是内核(kernel)函数。
- `__device__`：在GPU调用父函数，子函数在GPU执行。 由`__device__`修饰的函数可以被由`__global__`和`__device__`修饰的函数调用。
- `__host__`：在CPU调用父函数，子函数在CPU执行。 

---

https://developer.download.nvidia.cn/assets/cuda/files/NVIDIA-CUDA-Floating-Point.pdf

IEEE 754 mode(default): `-ftz=false -prec-div=true -prec-sqrt=true`
fast mode: `-ftz=true -prec-div=false -prec-sqrt=false`

在fast模式中，非规格化数将被转换为零，并且除法和平方根运算不会被计算到最接近的真实值的浮点数值。

当浮点异常发生时，NVIDIA的GPU不会触发trap handlers，也没有指示上溢、下溢或者denormal的标志位。

---

`#pragma unroll`指令建议编译器完全展开for循环。如果N是一个常量，编译器会尝试将循环体展开N次。如果N不是一个常量或者太大而无法完全展开，编译器可能会忽略这个指令，或者展开一定次数的迭代。

---

因为GPU不支持常规的Kernel递归，CPU上的很多递归算法只能换思路后进行改写，不能直接按原思路实现。而随着动态并行（Dynamic Parallelism）的引入，GPU现在能直接在Kernel中启动Kernel了。

https://zhuanlan.zhihu.com/p/674856090

CUDA动态并行详解（CDP2）

---

在“blocked”排列中，每个线程拥有一组连续的数据项；在“striped”排列中，所有线程拥有的数据项交错存储。

---

早期的GPU硬件上只有一个execution engine，因此，不论是哪个进程、哪个线程发起的kernel launch，都在同一个队列里排队。

随着GPU的发展，GPU上面开始出现了多个execution engine。

一个stream就对应于一个执行队列（加一个执行单元），用户可以自行决定是否把两个kernel分开放在两个队列里。

https://zhuanlan.zhihu.com/p/699754357

一文读懂cuda stream与cuda event

---

Local Thread Block：当前正在执行的线程所在的线程块。

Remote Thread Block：线程块簇中除本地线程块之外的其他线程块。

---

pytorch CUDA RadixSort call stack：

```cpp
MediumRadixSort
should_use_small_sort
sortKeyValueInplace
launch_stable_sort_kernel
segmented_sort_large_segments
radix_sort_pairs_impl
NO_ROCM(at_cuda_detail)::cub::DeviceRadixSort::SortPairs
cub::DeviceRadixSort::SortPairs
DeviceRadixSort::custom_radix_sort
DispatchRadixSort::Dispatch
DeviceRadixSortSingleTileKernel
triple_chevron
BlockRadixSort
BlockRadixSortT(temp_storage.sort).SortBlockedToStriped
RankKeys
DescendingBlockRadixRank
BlockRadixRank
```

---

![](/images/img6/tensile_kernel_parameters_performance_interdependencies.png)

原始地址：

https://github.com/ROCm/Tensile/wiki/Kernel-Parameters

---

https://developer.nvidia.com/blog/even-easier-introduction-cuda/

An Even Easier Introduction to CUDA

http://ishare.iask.sina.com.cn/f/17211495.html

深入浅出谈CUDA技术

http://blog.csdn.net/xsc_c/article/category/2186063

某人的并行计算专栏

https://mp.weixin.qq.com/s/9D7uda3CV7volenhl-jchg

推荐几个不错的CUDA入门教程

https://mp.weixin.qq.com/s/bvNnzkOzGYYYewc3G9DOIw

GPU是如何优化运行机器学习算法的？

https://mp.weixin.qq.com/s/nAwxtOUi6HpIjVOREgEfaA

CUDA编程入门极简教程

https://mp.weixin.qq.com/s/-zdIWkuRZXhsLJmOZljOBw

《基于GPU-多核-集群等并行化编程》

https://mp.weixin.qq.com/s/bCb5VsH58JII886lpg9lvg

如何在CUDA中为Transformer编写一个PyTorch自定义层

https://mp.weixin.qq.com/s/OYSzol-vufiKPuU9YxtbuA

矩阵相乘在GPU上的终极优化：深度解析Maxas汇编器工作原理

https://zhuanlan.zhihu.com/p/358220419

PyTorch自定义CUDA算子教程与运行时间分析

https://zhuanlan.zhihu.com/p/358778742

详解PyTorch编译并调用自定义CUDA算子的三种方式

https://zhuanlan.zhihu.com/p/360441891

熬了几个通宵，我写了份CUDA新手入门代码

https://mp.weixin.qq.com/s/EZxO8IIBDJ4c7eQhUffc2w

怎样节省2/3的GPU？爱奇艺vGPU的探索与实践

https://mp.weixin.qq.com/s/3VjGpyXZSkJhy6sFPUsZzw

GPU虚拟化，算力隔离，和qGPU

https://zhuanlan.zhihu.com/p/383115932

大佬是怎么优雅实现矩阵乘法的？

https://zhuanlan.zhihu.com/p/410278370

CUDA矩阵乘法终极优化指南

https://www.zhihu.com/column/c_1437330196193640448

深入浅出GPU优化

https://www.zhihu.com/question/41060378

自己写的CUDA矩阵乘法能优化到多快？

https://zhuanlan.zhihu.com/p/559957579

简单谈谈CUDA的访存合并

https://zhuanlan.zhihu.com/p/565897763

GPGPU编程模型之CUDA

http://blog.csdn.net/augusdi/article/details/12833235

这是一篇转帖的CUDA教程，原帖比较分散，不好看。

https://zhuanlan.zhihu.com/p/544864997

cuda中threadIdx、blockIdx、blockDim和gridDim的使用

https://zhuanlan.zhihu.com/p/690717002

一文读懂cuda代码编译流程

https://zhuanlan.zhihu.com/p/690880124

并不太短的CUDA入门（The Not So Short Introduction to CUDA）

https://zhuanlan.zhihu.com/p/693690123

一文读懂nvidia-smi背后的nvml库

https://www.zhihu.com/question/445590537

问个CUDA并行上的小白问题，既然SM只能同时处理一个WARP，那是不是有的SP处于闲置？
