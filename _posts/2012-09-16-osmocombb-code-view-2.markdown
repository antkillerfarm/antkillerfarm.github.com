---
layout: post
title: OsmocomBB 代码分析(2)
category: technology
---

# 开始 #

现在开始真正的代码之旅，这次主要讨论如下几个主题：

* Firmware 上电如何寻找BCCH.
* TDMA_sched机制.

首先我们要知道点常识，手机之所以能实现与其他手机实现实时通信，很关键的一点，是手机必须与基站（BTS）同步，因为GSM是TDMA与FDMA结合的通信系统，所以想要同步必须在时间，频率上同时与一个基站同步。这里面有很多知识与细节，比如RF功率检测，GMSK调制与解调，DSP core与ARM core的通信。但是在这个章节，我们不涉及信号处理和RF相关的细节，以及运行在host端的layer2&layer3的高层协议栈，我们只在乎ARM core的相关代码流程。如下图所示：

![alt text](focus_layer1.png "Title")

中间有色部分及时我们关注的重点。

# 背景知识 #

全世界不同的GSM运营商对频率的使用不同，比如中国常用的是900 Band, 1800 Band,这也是比较常用的Band，很多GSM手机都支持4个Band，即850,900,1800,1900。手机上电后，Stack会在各个Band里搜索BCCH 载频(Carrier)，他要搜寻出信号功率最大的一个ARFCN(The Absolute Radio Frequency Channel Number)。什么事ARFCN呢？这正是我将要说的，在每个Band里都有若干ARFCN，ARFCN其实就是一个频率点，Stack一般在搜索一个Band的时候，会遍历这个Band里的所有频率点，那这些频率点的具体频率是多少呢？下面有一个表格：

![alt text](gsm_bands_table.png "Title")

以及计算ARFCN的公式：

### GSM 900 Band ###

Up = 890.0 + (ARFCN * 0.2) 
Down = Up + 45.0

解释一下，GSM规定的在900 Band下，相邻频点之间频率空间为200KHZ。从代码看来，900 Band的ARFCN值域应该是[0,124], 而且对于每一个ARFCN，下行频率比上行频率高了45MHZ，比如下图所示：

![alt text](gsm_uplink_downlink.png "Title")

而1800 Band有些不同上行下行相差95MHZ，公式入下：

### GSM 1800 Band ###

Up = 1710.0 + ((ARFCN - 511) * 0.2) 
Down = Up + 95.0

# 初始化代码分析 #

上电运行layer1.bin后，首选运行main函数。其实在嵌入式系统中，程序基本都是从一个制定地址开始运行，运行一个类似jump的命令跳到另外一个地方，如果想从我们定义的main函数运行，必须需要一些初始的汇编代码跳到main函数的地址，这就涉及到一些CPU体系结构，芯片手册的一些规定，更多的时候还需要自己写连接文件，给程序中的函数与数据布局，以上内容不在本文涉猎，我们的main函数如下：

    int main(void)  
    {  
        board_init();  
   
        puts("\n\nOSMOCOMLayer 1 (revision " GIT_REVISION ")\n");  
        puts(hr);  
   
        /*Dump device identification */  
        dump_dev_id();  
        puts(hr);  
        //设置键盘处理函数  
        keypad_set_handler(&key_handler);  
   
        /*Dump clock config after PLL set */  
        calypso_clk_dump();  
        puts(hr);  
   
        display_puts("layer1.bin");  
   
        layer1_init();//此函数是很重要的初始化函数  
   
        display_unset_attr(DISP_ATTR_INVERT);  
        //开启ARM core和DSP core的中断使能  
        tpu_frame_irq_en(1,1);  
        //下面在l1a_compl_execute中连续执行一些在layer1_init中设定好的回调函数  
        //在运行中起到重要作用。  
        while(1) {  
            l1a_compl_execute();  
            update_timers();  
        }  
   
        /*NOT REACHED */  
        twl3025_power_off();  
    }

其他函数略过，着重看layer1_init函数：

    void layer1_init(void)  
    {  
    #ifndef CONFIG_TX_ENABLE  
        printf("\n\nTHISFIRMWARE WAS COMPILED WITHOUT TX SUPPORT!!!\n\n");  
    #endif  
   
       /*initialize asynchronous part of L1 */  
       l1a_init();  
       /*initialize TDMA Frame IRQ driven synchronous L1 */  
       l1s_init();  
       /*power up the DSP */  
       dsp_power_on();  
   
       /*Initialize TPU, TSP and TRF drivers */  
       tpu_init();  
       tsp_init();  
   
       rffe_init();  
   
    #if 0 /* only if RX TPU window isdisabled! */  
       /*Put TWL3025 in downlink mode (includes calibration) */  
       twl3025_downlink(1,1000);  
    #endif  
   
       /*issue the TRF and TWL initialization sequence */  
       tpu_enq_sleep();  
       tpu_enable(1);  
       tpu_wait_idle();  
   
       /*Disable RTC interrupt as it causes lost TDMA frames */  
       irq_disable(IRQ_RTC_TIMER);  
   
       /*inform l2 and upwards that we are ready for orders */  
       l1ctl_tx_reset(L1CTL_RESET_IND,L1CTL_RES_T_BOOT);  
    }        

其中l1a_init是用来初始化异步运行的部分，比如从串口发来的数据， socket过来的数据与消息，l1s_init是用来初始化同步运行的部分，比如中断调用。其他都是硬件的一些初始化。在l1s_init中，通过：

    irq_register_handler(IRQ_TPU_FRAME,&frame_irq);

注册了当有frame收到的时候需要执行的中断调用函数，所以frame_irq是个很重要的函数，下行的相关事宜都在此处进行。且看异步的部分,在l1a_init里调用了l1a_l23api_init函数：

    void l1a_l23api_init(void)  
    {  
        sercomm_register_rx_cb(SC_DLCI_L1A_L23,l1a_l23_rx_cb);  
    }  

sercomm_register_rx_cb给封装串口的实体注册回调函数来接收来自串口的消息，这个实体的数据结构如下:

    static struct {  
        int initialized;  
        /* transmitside */  
        struct {  
            struct llist_head dlci_queues[_SC_DLCI_MAX];  
            struct msgb *msg;  
            enum rx_state state;  
            uint8_t *next_char;  
        } tx;  
  
        /* receive side */  
        struct {  
            dlci_cb_tdlci_handler[_SC_DLCI_MAX];  
            struct msgb *msg;  
            enum rx_state state;  
            uint8_t dlci;  
            uint8_t ctrl;  
        } rx;  
      
    } sercomm;

sercomm结构体分两部分，有接收的rx，有发送的tx，通过注册rx部分的dlci_cb_t dlci_handler[_SC_DLCI_MAX]来接收来自串口的不同种类的消息，以uint8_t dlci来索引回调函数，他的取值也代表优先级，定义在一个枚举变量里：

    enum sercomm_dlci {  
        SC_DLCI_HIGHEST = 0,  
        SC_DLCI_DEBUG   = 4,  
        SC_DLCI_L1A_L23 = 5,  
        SC_DLCI_LOADER  = 9,  
        SC_DLCI_CONSOLE = 10,  
        SC_DLCI_ECHO    = 128,  
        _SC_DLCI_MAX  
    };

如果处理来自上层(layer2 ,layer3)的消息，则用SC_DLCI_L1A_L23。他的调用也是通过初始化串口时候注册的中断调用，如果串口有数据，他就会被调用。

也许有人会问，同样都是中断调用，凭什么串口中断是异步，而frame中断是同步？我也想知道，不过我猜，由于frame中断很重要，做的事情也很多，而且stack基本是围绕着这个中断在转，所以就比较像同步调用了。

# TDMA_sched机制 #

### TDMA概念介绍 ###

在介绍代码之前，我们先了解下TDMA的概念。我们已经知道GSM是由FDMA和TDMA综合而成的，上文提到过，在通常我们所用的GSM的四个频率带(Band)里，每个band里都是以200kHZ为间隔分成了若干个ARFCN频率点，通信中，根据某种规则还会有跳频，这是我们所说的FDMA。那什么是TDMA呢？在每一个ARFCN中，将会以时间轴将其分成8个分段，称作时隙(Time Slot)，如下图： 

![alt text](gsm_time_slot.png "Title")

其中每个时隙持续576.9 µs，如果以GMSK为调制方法，一个时隙可传输156.25bit的数据，时隙是最基本的无线资源。根据基站的设置，每个时隙都会分配给不同的手机使用。每个时隙所传输的数据叫做Data Burst,总共有四种Data Burst:

* Normal Burst (NB)
* Frequency Correction Burst(FB)
* Synchronization Burst (SB)
* Access Burst (AB)

他们都有不同的用途，比如FB与SB就是用来和基站同步的，他们都是下行(downlink)的，而AB是手机用来向基站请求资源的，他是上行(uplink)的。这4种Burst都与逻辑信道有一定关系，我们以后会详细叙述。这8个时隙加起来称作一个TDMA frame，每个TDMA frame都有一个编号，即FN(Frame Number)，这个编号是基站分发的。FN是一个很重要的信息，他是手机与基站同步必须需要知道的信息，以及同步后的其他活动。前文提到过，对于FDMA的上下行是以45Mhz分隔开，即下行的频率减去45Mhz的间隔，就是上行的频率。而对于TDMA亦有类似的分别，为了上下行不互相干扰，上行一般落后下行3个时隙，如图：

![alt text](gsm_tdma_frame.png "Title")

### 代码解读 ###

TDMA知识的介绍先告一段落，接下来我们来看看OsmocomBB是如何实现TDMA的机制的，主要的代码在Tdma_sched.h,Tdma_sched.c中。Tdma_sched.h 中定义了几个数据结构:

    typedef int tdma_sched_cb(uint8_t p1, uint8_t p2, uint16_t p3);  
  
    /* A single item in a TDMA scheduler bucket */  
    struct tdma_sched_item {  
        tdma_sched_cb *cb;  
        uint8_t p1;  
        uint8_t p2;  
        uint16_t p3;  
        int16_t prio;  
        uint16_t flags;     /* TDMA_IFLG_xxx */  
    };  
  
    /* A bucket inside the TDMA scheduler */  
    struct tdma_sched_bucket {  
        struct tdma_sched_item item[TDMASCHED_NUM_CB];  
        uint8_t num_items;  
    };  
  
    /* The scheduler itself, consisting of buckets and a current index */  
    struct tdma_scheduler {  
        struct tdma_sched_bucket bucket[TDMASCHED_NUM_FRAMES];  
        uint8_t cur_bucket;  
    };

结构体 tdma_scheduler 中包括了25个tdma_sched_bucket,而每个 tdma_sched_bucket 有8个 tdma_sched_item,我们可以用图来直观的了解下:

![alt text](tdma_sched.png "Title")

这里面基本的数据结构是 tdma_sched_item,他里面包括几个变量,以及一个函数指针,那几个变量亦是为调用函数指针所指向的函数所需要的参数。Tdma_sched 的主要作用是在TDMA frame 中断来的时候,通过调用配置的回调函数来给基带子系统配置参数,或者获取基带子系统返回的结果。说得很抽象,但是主要处理的还是那四种 Data Burst。

首先看设置，通过调用tdma_schedule_set来设置相关的回调函数组合，其实就是一个tdma_sched_item数组，这个函数定义如下：

    /* Schedule a set of items starting from 'frame_offset' TDMA frames in the future */  
    int tdma_schedule_set(uint8_t frame_offset, const struct tdma_sched_item *item_set, uint16_t p3); 

参数frame_offset表示在当前bucket的位置往后移的位移，当然要确保不能越界，参数item_set表示一个tdma_sched_item数组，数组里面存放的根据时序分布的回调函数的指针。p3是传递进来的其他的有用的附加数据，p3赋给了tdma_sched_item的p3，tdma_schedule_set代码如下：

    int tdma_schedule_set(uint8_t frame_offset, const struct tdma_sched_item *item_set, uint16_t p3)  
    {  
        struct tdma_scheduler *sched = &l1s.tdma_sched;  
        uint8_t bucket_nr = wrap_bucket(frame_offset);  
        int i, j;  
  
        for (i = 0, j = 0; 1; i++) {  
            const struct tdma_sched_item *sched_item = &item_set[i];  
            struct tdma_sched_bucket *bucket = &sched->bucket[bucket_nr];  
  
            if (sched_item->cb == &tdma_end_set) {  
                /* end of scheduler set, return */  
                break;  
            }  
  
            if (sched_item->cb == NULL) {  
                /* advance to next bucket (== TDMA frame) */  
                bucket_nr = wrap_bucket(++frame_offset);  
                j++;  
                continue;  
            }  
            /* check for bucket overflow */  
            if (bucket->num_items >= ARRAY_SIZE(bucket->item)) {  
                puts("tdma_schedule bucket overflow\n");  
                return -1;  
            }  
        
            /* copy the item from the set into the current bucket item position */  
            memcpy(&bucket->item[bucket->num_items], sched_item, sizeof(*sched_item));  
            bucket->item[bucket->num_items].p3 = p3;  
            bucket->num_items++;  
        }  
  
        return j;  
    }

先根据frame_offset和当前的bucket的位置，定位到将要操作的bucket上去，然后开始遍历输入的tdma_sched_item数组，主要以每个tdma_sched_item的回调函数来判断如何设置tdma_sched_item，比如，当sched_item->cb为null时，则直接转移到下一个bucket, 如果sched_item->cb为&tdma_end_set，则设置结束，因为tdma_end_set是在tdma_sched_item数组中最后一个元素里出现。其他的说明是正常的拥有回调函数的tdma_sched_item，他们可以正常的拷贝到相应的bucket里。先举个代码里的例子，比如在Prim_pm.c中，设置寻找最大功率点：

    static const struct tdma_sched_item pm_sched_set[] = {  
        SCHED_ITEM_DT(l1s_pm_cmd, 0, 1, 0), SCHED_END_FRAME(),  
                         SCHED_END_FRAME(),               
        SCHED_ITEM(l1s_pm_resp, -4, 1, 0),  SCHED_END_FRAME(),   
        SCHED_END_SET()  
    };

在叙述设置的细节之前，我们先了解下上述代码中的神秘的宏定义，他们都定义在tdma_sched.h中：

    #define SCHED_ITEM(x, p, y, z)      { .cb = x, .p1 = y, .p2 = z, .prio = p, .flags = 0 }  
    #define SCHED_ITEM_DT(x, p, y, z)   { .cb = x, .p1 = y, .p2 = z, .prio = p, \  
                      .flags = TDMA_IFLG_TPU | TDMA_IFLG_DSP }  
    #define SCHED_END_FRAME()       { .cb = NULL, .p1 = 0, .p2 = 0 }  
    #define SCHED_END_SET()         { .cb = &tdma_end_set, .p1 = 0, .p2 = 0 }

SCHED_ITEM与SCHED_ITEM_DT类似，都是实实在在去设置回调函数及其参数的，不同的是SCHED_ITEM_DT的多加了flags，即TDMA_IFLG_TPU | TDMA_IFLG_DSP。简而言之，就是说明这个回调函数是和时间处理以及基带的DSP相关的，再简单的说就是给基带子系统设置一些参数。而SCHED_ITEM的flags为0，一般就是查看基带子系统的返回结果。我们看到SCHED_END_FRAME是给cb赋值为null，所以他是要告诉tdma_sched放到下一个bucket里去，直接的结果就是下一个tdma_sched_item的回调函数要等待下一个TDMA frame来的时候才能调用。最后一个SCHED_END_SET，他是要告诉tdma_sched这一次设置结束了。

上述pm_sched_set数组是文章开头所说的寻找最大功率频率点所需要对基带子系统所做的设置，在设置前tdma_scheduler 没有设置过任何数组，所以当前bucket数组都是空白的，tdma_scheduler 的cur_bucket也为0，指向第一个bucket元素, 如下图的表格，代表了bucket数组，蓝色的列代表当前的bucket索引，也就是cur_bucket所指的bucket，每列的第一个格子里的数字表示每个bucket里num_items的数值，也就是当前bucket已经设置回调函数的数量，他的数值不能超过8。现在tdma_scheduler还未进行过设置，所以都是0，cur_bucket也指向第一个bucket。(当然26个bucket太多不容易显示，所以只显示了8个。)

<pre>
<table border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="top" style="background:#00B0F0">
<p>0 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</p>
</td>
<td valign="top">
<p>0 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</p>
</td>
<td valign="top">
<p>0 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</p>
</td>
<td valign="top">
<p>0 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</p>
</td>
<td valign="top">
<p>0 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</p>
</td>
<td valign="top">
<p>0 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</p>
</td>
<td valign="top">
<p>0 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</p>
</td>
<td valign="top">
<p>0 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</p>
</td>
</tr>
<tr>
<td valign="top" style="background:#00B0F0">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
</tr>
<tr>
<td valign="top" style="background:#00B0F0">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
</tr>
<tr>
<td valign="top" style="background:#00B0F0">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
</tr>
<tr>
<td valign="top" style="background:#00B0F0">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
</tr>
<tr>
<td valign="top" style="background:#00B0F0">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
</tr>
<tr>
<td valign="top" style="background:#00B0F0">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
</tr>
<tr>
<td valign="top" style="background:#00B0F0">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
<td valign="top">
<p>&nbsp;</p>
</td>
</tr>
</tbody>
</table>
</pre>

在调用tdma_schedule_set之后，bucket数组变成了如下的样子：

<pre>
<table border="1" cellspacing="0" cellpadding="0" style="background:white">
<tbody>
<tr>
<td valign="top" style="background:white">
<pre>1&nbsp; SCHED_ITEM_DT(l1s_pm_cmd, 0, 1, 0)&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;</pre>
</td>
<td valign="top">
<p align="left">0 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</p>
</td>
<td valign="top">
<pre>1&nbsp; SCHED_ITEM(l1s_pm_resp, -4, 1, 0)&nbsp; &nbsp;</pre>
</td>
<td valign="top" style="background:#00B0F0">
<p align="left">0 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</p>
</td>
<td valign="top">
<p align="left">0 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</p>
</td>
<td valign="top">
<p align="left">0 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</p>
</td>
<td valign="top">
<p align="left">0 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</p>
</td>
<td valign="top">
<p align="left">0 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</p>
</td>
</tr>
<tr>
<td valign="top" style="background:white">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top" style="background:#00B0F0">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
</tr>
<tr>
<td valign="top" style="background:white">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top" style="background:#00B0F0">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
</tr>
<tr>
<td valign="top" style="background:white">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top" style="background:#00B0F0">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
</tr>
<tr>
<td valign="top" style="background:white">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top" style="background:#00B0F0">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
</tr>
<tr>
<td valign="top" style="background:white">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top" style="background:#00B0F0">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
</tr>
<tr>
<td valign="top" style="background:white">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top" style="background:#00B0F0">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
</tr>
<tr>
<td valign="top" style="background:white">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top" style="background:#00B0F0">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
<td valign="top">
<p align="left">&nbsp;</p>
</td>
</tr>
</tbody>
</table>
</pre>

了解了设置之后，我们再看看TDMA_sched的执行，且看tdma_sched_execute的代码：

    int tdma_sched_execute(void)  
    {  
        struct tdma_scheduler *sched = &l1s.tdma_sched;  
        struct tdma_sched_bucket *bucket;  
        int i, num_events = 0;  
        int seq[TDMASCHED_NUM_CB];  
  
        /* determine current bucket */  
        bucket = &sched->bucket[sched->cur_bucket];  
  
        /* get sequence in priority order */  
        _tdma_sched_bucket_sort(bucket, seq);  
  
        /* iterate over items in this bucket and call callback function */  
        for (i = 0; i < bucket->num_items; i++) {  
            struct tdma_sched_item *item = &bucket->item[seq[i]];  
            int rc;  
  
            num_events++;  
  
            rc = item->cb(item->p1, item->p2, item->p3);  
            if (rc < 0) {  
                printf("Error %d during processing of item %u of bucket %u\n",  
                    rc, i, sched->cur_bucket);  
                return rc;  
            }  
            /* if the cb() we just called has scheduled more items for the  
             * current TDMA, bucket->num_items will have increased and we  
             * will simply continue to execute them as intended. Priorities  
             * won't work though ! */  
        }  
  
        /* clear/reset the bucket */  
        bucket->num_items = 0;  
  
        /* return number of items that we called */  
        return num_events;  
    }

因为是每一次TDMA frame中断就会调用一次tdma_sched_execute，所以每一个bucket的回调函数组合只在同一个TDMA frame来到的时候运行，而且还要根据每一个tdma_sched_item所设置的优先级来排序执行。每一次执行完，这个bucket的数据不在有效，所以将当前bucket的num_items清零。

TDMA_sched最主要的就是设置与执行，我们再看看其他函数的功能：

* 每次TDMA frame中断调用完tdma_sched_execute，会调用tdma_sched_advance来将cur_bucket推进到下一个bucket，为下一次TDMA frame中断做准备.
* 上文提到的SCHED_ITEM_DT宏里会设置flags，在TDMA frame中断里会调用tdma_sched_flag_scan来查看当前bucket中所设置的flags情况.
* 在有些情况下，由于时序要求，需要对tdma_scheduler重置，可以调用tdma_sched_reset.
* 有时需要调试tdma_sched，可以调用tdma_sched_dump来检查bucket状况.

# 尾声 #

这篇拙劣的文章先后荒废半年多时间后才写完，差一点又输给自己的懒惰，不管怎样，还是写完了。由于本人对GSM，通信以及OsmocomBB了解肤浅，希望高手能够将一些模糊的地方解答的更清楚些，本人不甚感激。下一篇，我计划介绍介绍BCCH decoding的东西，即SCH，FCCH的处理。尽管我还是个菜鸟，但是我要坚持写这些文章，因为我坚信发布是最好的学习方法。

# 参考资料 #

1. [bb.osmocom.org/](http://bb.osmocom.org/).
2. [GSM for dummies](http://gsmfordummies.com/).