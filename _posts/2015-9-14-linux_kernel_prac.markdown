---
layout: post
title:  linux内核研究（二）
category: linux 
---

* toc
{:toc}


# Uart驱动分析

## Write

1.与应用层的接口

这一层的操作是基于文件的。众所周知，UART属于TTY设备。因此实际执行的函数是tty_write@tty_io.c。

2.`tty_ldisc.ops->write`

3.`n_tty_write@n_tty.c`

4.`tty_struct.ops->write => tty_operations->write@serial_core.c`

5.`uart_write@serial_core.c`

6.`__uart_start@serial_core.c`

7.`uart_port.ops->start_tx=>uart_ops->write@uartlite.c`

这一层以下，就和具体的设备有关了。这里以Xlinux的uartlite为例。

8.`ulite_start_tx`

9.`ulite_transmit`

这里已经是具体的寄存器操作了。

## Read

Read的过程要复杂一些，可分为上层调用部分和底层驱动部分。

上层调用部分：

1.`tty_read@tty_io.c`

2.`tty_ldisc.ops->read`

3.`n_tty_read@n_tty.c`

上层调用，到这里为止。这个函数执行到add_wait_queue时，会等待底层驱动返回接收的数据。底层驱动可以是中断式的，也可以是轮询式的。函数会调用copy_from_read_buf，将内核态的数据搬到用户态。

底层驱动部分

1.`ulite_startup=>ulite_isr@uartlite.c`

这里仍以Xlinux的uartlite为例。初始化阶段注册ulite_isr中断服务程序。

2.`ulite_receive@uartlite.c`

具体的寄存器操作。

3.`tty_flip_buffer_push@tty_buffer.c`

4.`tty_schedule_flip@tty_buffer.c`

调用schedule_work唤醒上层应用。

## select代码分析

1.`select@select.c`

2.`core_sys_select@select.c`

3.`do_select@select.c`

# I2C的GPIO实现

## 概述

I2C的GPIO实现的代码在drivers/i2c/busses/i2c-gpio.c中。从本质来说这是一个i2c_adapter，它使用i2c_bit_add_numbered_bus函数将自己注册到I2S总线上。

由于i2c_adapter是直接寻址设备，因此I2C的GPIO实现是以platform driver的方式注册的，可以在/sys/devices/platform/i2c-gpio.0/i2c-0路径下查看总线上现有的设备（这里的0指的是0号i2c总线，某些系统中可能有不止一条i2c总线）。

## Write

`drivers/i2c/i2c-dev.c: i2cdev_write`

`drivers/i2c/i2c-core.c: i2c_master_send`

`drivers/i2c/i2c-core.c: i2c_transfer`

`drivers/i2c/i2c-core.c: __i2c_transfer`

这里会调用i2c_adapter结构的algo->master_xfer函数。具体到i2c-gpio就是：

`drivers/i2c/algos/i2c-algo-bit.c: bit_xfer`

`drivers/i2c/algos/i2c-algo-bit.c: sendbytes -- 发送字节`

`drivers/i2c/algos/i2c-algo-bit.c: i2c_outb -- 发送bit`

以下以SDA线的操作为例。这里调用i2c_algo_bit_data结构的setsda函数。具体到i2c-gpio就是：

`drivers/i2c/busses/i2c-gpio.c: i2c_gpio_setsda_val`

再以下就是具体的GPIO寄存器操作了。

# Driver Probe

## 驱动模块加载方式

静态：直接编译到内核中。

动态：编译成.ko文件，然后用insmod命令加载之。

## 驱动分类（按总线类型分）

驱动按设备总线类型分，可分为两类：

1.直接地址访问设备。这类设备的驱动被称为platform driver。

2.间接地址访问设备，也称作总线地址访问设备。这类设备的驱动按总线类型，可分为PCI驱动、USB驱动等等。

## 驱动的probe函数

作为驱动的实现来说，首先就是要实现驱动的probe函数。probe函数起到了驱动的初始化功能。但之所以叫probe，而不是init，主要是由于probe函数，还具有设备检测的功能。

以下以s3c24xx_uda134x音频驱动为例，说一下设备检测的机制：

1.驱动模块主要处理两个对象：设备和驱动。s3c24xx_uda134x属于platform driver，因此它的设备对象的数据结构是platform_device类型的，而它的驱动对象的数据结构是platform_driver类型的。

2.驱动对象的注册。使用module_platform_driver宏即可。

3.生成设备对象。

4.设备检测时，首先根据总线类型，调用总线驱动的probe函数，再根据设备类型调用设备驱动的probe函数。最终完成设备对象和驱动对象的绑定。

## module_platform_driver详解

module_platform_driver定义在include/linux/platform_device.h中。

它的主要内容是注册和反注册驱动，以及module_init/module_exit。

介绍module_init/module_exit的文章很多，这里仅将要点摘录如下：

1.__init宏修饰的函数会链接到.initcall.init段中，这个段只在内核初始化时被调用，然后就被释放出内存了。

2.定义别名。

`int init_module(void) __attribute__((alias(#initfn)));`

不管驱动模块的init函数名字叫什么，都定义别名为init_module。这样insmod在加载.ko文件时，就只找init_module函数的入口地址就可以了。

## 生成设备对象详解

这里以mini2440为例，说明一下静态生成设备对象的过程：

在arch/arm/mach-s3c24xx/mach-mini2440.c中有一个mini2440_devices数组。这个数组定义了板子初始化阶段静态生成的设备对象。

在该文件的mini2440_init函数中，调用platform_add_devices函数，将mini2440_devices数组添加到内核中。

这个例子中和音频有关的设备为s3c_device_iis、uda1340_codec和mini2440_audio。这三个设备的name分别为s3c24xx-iis、uda134x-codec和s3c24xx_uda134x，与相应驱动名称一致，正好对应ASOC中的Platform、Codec和Machine。

PC上的情况比较特殊。由于设备数量种类繁多，因此并不采用将所有设备对象都放到一个数组中的方式。而是在驱动模块加载时，在模块的init函数中，生成设备对象。某些嵌入式驱动模块也采用了类似的方法。

## probe被调用的流程

`drivers/base/driver.c: driver_register`

`drivers/base/bus.c: bus_add_driver`

`drivers/base/dd.c: driver_attach`

`drivers/base/dd.c: _driver_attach`

`drivers/base/dd.c: driver_probe_device`

`drivers/base/dd.c: really_probe`

# 模块初始化

Linux既然由若干模块组成，那么这些模块在启动阶段，必然存在一个加载顺序的问题。这方面可以通过以下的宏来确定加载的优先级。

```c
#define pure_initcall(fn)		__define_initcall(fn, 0)
#define core_initcall(fn)		__define_initcall(fn, 1)
#define core_initcall_sync(fn)		__define_initcall(fn, 1s)
#define postcore_initcall(fn)		__define_initcall(fn, 2)
#define postcore_initcall_sync(fn)	__define_initcall(fn, 2s)
#define arch_initcall(fn)		__define_initcall(fn, 3)
#define arch_initcall_sync(fn)		__define_initcall(fn, 3s)
#define subsys_initcall(fn)		__define_initcall(fn, 4)
#define subsys_initcall_sync(fn)	__define_initcall(fn, 4s)
#define fs_initcall(fn)			__define_initcall(fn, 5)
#define fs_initcall_sync(fn)		__define_initcall(fn, 5s)
#define rootfs_initcall(fn)		__define_initcall(fn, rootfs)
#define device_initcall(fn)		__define_initcall(fn, 6)
#define device_initcall_sync(fn)	__define_initcall(fn, 6s)
#define late_initcall(fn)		__define_initcall(fn, 7)
#define late_initcall_sync(fn)		__define_initcall(fn, 7s)
```

上面的宏中，越前面的优先级越高。同一优先级下，按照链接顺序确定加载顺序，因此可以通过修改链接文件来修改加载顺序，但一般来说，并没有这个必要。

运行阶段的加载，由于是动态加载，没有加载顺序的问题（程序员代码控制加载顺序），因此这些宏都被编译成module_init。

# Camera

常见的Camera从软件角度，可分为三类：

1.Soc Camera。

2.USB Camera。

3.IP Camera。

其中，前两类设备需要相关的驱动程序。Linux内核配置方法如下：

```text
Device Drivers --->
    <*> Multimedia support --->
    [*]   Cameras/video grabbers support
    [*]   Media USB Adapters  --->
        <*>   USB Video Class (UVC)
        <*>   GSPCA based webcams  --->  
    [*]   V4L platform devices  --->
        <*>   SoC camera support
```

UVC是Microsoft与另外几家设备厂商联合推出的为USB视频捕获设备定义的协议标准，目前已成为USB org标准之一。

GSPCA同样是一种标准，早期的很多摄像头用的就是这一标准。

这两类设备的应用层接口为V4L2。教程可参见：

http://blog.csdn.net/mmz_xiaokong/article/details/5666993

对于UVC设备，可以用相关工具直接获取图像。常用的工具有：

GTK+ UVC Viewer

http://guvcview.sourceforge.net/

luvcview

https://github.com/ksv1986/luvcview

IP Camera无须驱动，只要提供网口就行了。有的设备支持wifi，连网口也省了。

IP Camera的应用层接口一般为网络协议接口，如TCP/IP、HTTP、RTP/RTSP等。

# GPIO

GPIO相对来说是最简单的一类驱动，代码在drivers/gpio文件夹下。

从硬件来说，GPIO有两种输出模式：

push-pull模式电平转换速度快，但是功耗相对会大些。

open-drain模式功耗低，且同时具有“线与”的功能。（同时注意GPIO硬件模块内部是否有上拉电阻，如果没有，需要硬件电路上添加额外的上拉电阻）

可在系统的/sys/class/gpio路径下查看当前的GPIO设备。

参考文献：

http://blog.csdn.net/mirkerson/article/details/8464290

http://www.cnblogs.com/lagujw/p/4226424.html

http://www.aichengxu.com/view/18529

http://wenku.baidu.com/view/a6c4b6bfc77da26925c5b001.html?re=view

## 从gpio_set_value到寄存器操作

`include/linux/gpio.h: gpio_set_value`

`include/asm-generic/gpio.h: __gpio_set_value`

`drivers/gpio/gpiolib.c: gpiod_set_raw_value`

这里会调用gpio_chip结构的set函数指针。我们只需要在定义gpio_chip结构的时候，将寄存器操作函数设置到set函数指针中即可。gpio_chip结构可在模块初始化阶段，使用gpiochip_add函数添加到系统中。

## 输入事件处理

1.轮询方式

drivers/input/keyboard/gpio_keys_polled.c中提供了轮询方式的输入事件处理。该实现主要是注册了一个名为gpio-keys-polled的input_polled_dev。而input_polled_dev则利用queue_delayed_work实现任务的调度。

2.中断方式

drivers/input/keyboard/gpio_keys_polled.c中提供了中断方式的输入事件处理。该实现最终调用了request_irq注册中断处理函数。

无论是轮询还是中断，处理的结果最终都会调用input_event函数生成应用层可访问的事件。

代码的编译配置在：

```text
Device Drivers --->
Input device support --->
Keyboards --->
<*> GPIO Buttons
<*> Polled GPIO buttons
```

## GPIO的使用方向

除了gpio_direction_input和gpio_direction_output之外，devm_gpio_request_one和gpio_request_one也可用于设置使用方向，只要设置好flag参数即可。内核中的leds-gpio和gpio-keys模块都是使用后面的方法设置使用方向的。

此外，在board级的GPIO实现中，需要注意以下几点：

1.是否有单独的寄存器用于设置使用方向。这个问题与具体的硬件有关，有的硬件可根据赋值语句的方向，自动切换GPIO的使用方向。

2.如果有单独的设置使用方向的寄存器的话，需要在gpio_set_value和gpio_get_value函数的实现中，将使用方向的设置操作添加进去。I2C的algo代码并不会在set或着get操作时，修改GPIO的使用方向。

## active_low

active_low的设置要根据硬件的连接，如果按下按键为高电平那么active_low=0，如果按下按键为低电平那么active_low=1.如果这个参数搞错了，按键松开后就不断发按键键码，表现为屏幕上乱动作。

也因为active_low的存在，input_event返回的value实际上并不是GPIO的值，1表示按键按下，0表示按键抬起。
