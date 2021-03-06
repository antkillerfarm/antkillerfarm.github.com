---
layout: post
title:  Linux学习心得（三）
category: linux 
---

* toc
{:toc}

# 环境变量

设置环境变量的方法：

1）临时的：使用export命令声明即可，变量在关闭shell时失效。示例如下：

`export PATH=/home/xyz/bin:$PATH;`

2）永久的：需要修改配置文件，变量永久生效。

在/etc/profile文件中添加变量（对所有用户生效）。修改文件后要想马上生效，还要运行`source /etc/profile`，不然只能在下次重进此用户时生效。

在用户目录下的.bash_profile文件中增加变量（对该用户生效）。同样需要source才能马上生效。

重要的环境变量：

PATH：可执行文件路径。

LD_LIBRARY_PATH：动态链接库文件路径

# minicom

1.查看串口设备

`ls -l /dev`

2.连接串口设备

`minicom -D /dev/ttyS0`

3.菜单

Ctrl+A Z

4.退出

Ctrl+A X

# MobaXterm

MobaXterm是一个远程终端登录软件。

http://mobaxterm.mobatek.net/

# tmux

你是不是经常需要SSH或者telent远程登录到Linux服务器？你是不是经常为一些长时间运行的任务而头疼，比如系统备份、ftp传输等等。

通常情况下我们都是为每一个这样的任务开一个远程终端窗口，因为他们执行的时间太长了。必须等待它执行完毕，在此期间可不能关掉窗口或者断开连接，否则这个任务就会被杀掉，一切半途而废了。

这个问题的解决办法是安装一个会话管理工具。原先主要使用screen：

https://www.gnu.org/software/screen/

这是一个有30年历史（1987年）的软件。

现在的话，一般推荐使用tmux：

https://github.com/tmux/tmux/

安装：

`sudo apt install tmux`

参考：

https://linuxtoy.org/archives/from-screen-to-tmux.html

从screen切换到tmux

http://mingxinglai.com/cn/2012/09/tmux/

tmux的使用方法和个性化配置

http://chengjin.li/2017/08/09/tmux-using-tutorial/

终端复用工具---tmux的安装及使用

# 设置随机的MAC地址

1.设置MAC地址

`ifconfig eth0 hw ether 477265656e00`

其中eth0是网口的名称，477265656e00是要设置的MAC地址（十六进制）。

2.生成随机数

随机数的生成在Linux中有多种方法，这里使用openssl。因为它和MAC都属于网络编程的范畴，同时使用的概率较大。

`openssl rand -hex 6`

3.SIOCSIFHWADDR: Cannot assign requested address错误

MAC地址的某些位有特定的含义，并不能随意设置。仍以477265656e00为例，第一个字节0x47的最后两位含义如下：

（00）统一管理的单播MAC

（01）统一管理的多播MAC

（10）本地管理的单播MAC

（11）本地管理的多播MAC

由于针对ADSL路由等这样的网络终端，一般使用的都是统一管理的单播MAC。

# 设置网卡eth0的IP地址和子网掩码

`sudo ifconfig eth0 192.168.2.1 netmask 255.255.255.0`

# 无线设置

查看无线网卡状态：

`iwconfig`

`wpa_cli`

扫描周围的wifi信号：

`iwlist scanning`

# 下载工具

wget和curl是最常见的下载工具。这里推荐使用axel，该工具支持多路http下载。

示例：

`wget -c <URL>`

`curl -C -o <file name> <URL>`

`axel <URL>`

参考：

http://os.51cto.com/art/201605/511423.htm

Linux用户宝典：用于下载的十大命令行工具

# NFS

## 客户端

`mount -t nfs 192.168.0.1:/tmp /mnt/nfs`

挂载NFS。挂载点（即上例中的/mnt/nfs）必须事先创建。

`mount: /bak: bad option; for several filesystems (e.g. nfs, cifs) you might need a /sbin/mount.<type> helper program`

出现上面的问题，需要：

`sudo apt install nfs-common libnfs-utils`

## 服务器端

```bash
sudo apt install nfs-server
cd /
sudo mkdir nfs-server
sudo chmod 777 nfs-server
sudo chmod 666 /etc/exports
echo "/nfs-server *(rw,sync,no_root_squash)">>/etc/exports
sudo service nfs-server restart
```

参考：

https://www.cnblogs.com/tu13/p/ubuntu_nfs.html

ubuntu18.04搭建NFS服务器

# OpenGrok

OpenGrok是一个阅读源码的Web系统。

官网：

http://oracle.github.io/opengrok/

代码：

https://github.com/oracle/opengrok

参考：

http://mazhuang.org/2016/12/14/rtfsc-with-opengrok/

搭建大型源码阅读环境——使用OpenGrok

# ELF & PE

![](/images/img4/exe.png)

https://zhuanlan.zhihu.com/p/254882216

深入浅出ELF

https://mp.weixin.qq.com/s/vnqANjTMNqdjkHXl203eRg

逆向工程基础：从PE文件到进程地址空间

https://mp.weixin.qq.com/s/_RD_jY-gJFxlT9DX2jGDAA

只有170字节，最小的64位Hello World程序这样写成

---

elf格式除了代码和数据外还有很多有用的信息，可以被标准boot loader识别，你如果不想自己花费10+天去自己实现stage1和stage2的bootloader的话，elf文件可以帮你省很多时间，因为grub可以直接加载，qemu也可以直接运行。

https://www.zhihu.com/question/49580321

C如何编译出一个不需要操作系统的程序？

# 同步锁

read-write lock、RCU lock、spin lock

对于悲观锁，开发者认为数据发送时发生并发冲突的概率很大，所以每次进行读操作前都会上锁。

对于乐观锁，开发者认为数据发送时发生并发冲突的概率不大，所以读操作前不上锁。

到了写操作时才会进行判断，数据在此期间是否被其他线程修改。如果发生修改，那就返回写入失败；如果没有被修改，那就执行修改操作，返回修改成功。

乐观锁一般采用Compare And Swap（CAS）算法进行实现。

比较并交换(compare and swap, CAS)，是原子操作的一种，可用于在多线程编程中实现不被打断的数据交换操作，从而避免多线程同时改写某一数据时由于执行顺序不确定性以及中断的不可预知性产生的数据不一致问题。

RCU（Read-Copy Update）机制读取数据的时候不对链表进行耗时的加锁操作。这样在同一时间可以有多个线程同时读取该链表，并且允许一个线程对链表进行修改（修改的时候，需要加锁）。

RCU适用于需要频繁的读取数据，而相应修改数据并不多的情景，例如在文件系统中，经常需要查找定位目录，而对目录的修改相对来说并不多，这就是RCU发挥作用的最佳场景。

https://mp.weixin.qq.com/s/mTOzcdjaak-z6ypL9MR2Lw

小白科普：悲观锁和乐观锁

https://mp.weixin.qq.com/s/t-jZ9GoqW46rU3t9ahHqCQ

mysql悲观锁总结和实践

https://mp.weixin.qq.com/s/6MRi_UEcMybKn4YXi6qWng

操作系统中锁的实现原理

https://mp.weixin.qq.com/s/yHSraIAYsjYWPaGXA3PijA

一张图读懂非公平锁与公平锁

https://mp.weixin.qq.com/s/qnZL4ENAbTvVMVcImVTtYw

深入浅出CAS

https://mp.weixin.qq.com/s/g6TK1QiZgqPBDCE7gDJ3dA

面试必问的CAS原理你会了吗？

https://mp.weixin.qq.com/s/T_z2_gsYfs6A-XjVTVV_uQ

说说无锁(Lock-Free)编程那些事（上）

https://mp.weixin.qq.com/s/h75n7sHnrmoLJ4DVAW5AUQ

说说无锁(Lock-Free)编程那些事（下）

https://blog.csdn.net/zqz_zqz/article/details/70233767

java中的锁--偏向锁、轻量级锁、自旋锁、重量级锁

https://mp.weixin.qq.com/s/muSUJuE1A45UTXpWTeFxOA

24张图带你彻底理解Java中的21种锁

https://mp.weixin.qq.com/s/gbCshU5eEn4Gefduk1zdCQ

浅谈Java并发下的乐观锁

https://mp.weixin.qq.com/s/czLqzt0cRwPZeQ4WDaKiJQ

无锁队列的实现

https://mp.weixin.qq.com/s/rpufplXGYFo3P3mdIyY6vA

24张图带你彻底理解Java中的21种锁

https://www.cnblogs.com/linhaostudy/p/8463529.html

Linux RCU机制详解

https://zhuanlan.zhihu.com/p/67520807

Linux内核中的RCU

# 动态库版本设置

linux动态库使用soname来设定动态库的版本兼容性。

`g++ -fPIC -shared -Wl,-soname,libbar.so.1 -o libbar.so.1.1.0`

这个命令生成的动态库的名字为`libbar.so.1.1.0`，soname为`libbar.so.1`。

soname的意思是：形如`libbar.so.1.x.y`的动态库，都可以兼容。

`SET_TARGET_PROPERTIES(hello PROPERTIES VERSION 1.2 SOVERSION 1)`

这是cmake中的用法。

参考：

https://www.jianshu.com/p/931a814083ce

Linux动态库soname的使用

https://tldp.org/HOWTO/Program-Library-HOWTO/shared-libraries.html

Shared Libraries

# popen

popen()函数通过创建一个管道，调用fork产生一个子进程，执行一个shell以运行命令来开启一个进程。也就是说这个函数可以执行shell命令，而且还可以用fread或fgets来获取命令执行后的输出结果。

例子如下：

```c
int8_t strcmd[256];
memset(strcmd, 0 , sizeof(strcmd));
sprintf(strcmd, "cat /etc/resolv.conf | awk '{printf $2}'");
pfile = popen(strcmd, "r");
if (pfile != NULL){
	int8_t str[64];
	bzero(str, sizeof(str));
	fgets(str, sizeof(str), pfile);
	pclose(pfile);
}
```

# 调整交换文件大小

```bash
fallocate -l 16G /swapfile
chmod 600 /swapfile
ls -lh /swapfile
mkswap /swapfile
swapon /swapfile
swapon --show
# Add this line to /etc/fstab to mount swap at boot
/swapfile swap swap defaults 0 0
swapoff /swapfile
```

# SSH

## keygen

```bash
cd ~/.ssh
ssh-keygen
cat ~/.ssh/id_rsa.pub
```

>`ssh-keygen`命令会生成两个文件id_rsa和id_rsa.pub，前者是私钥，后者是公钥，不要弄错了。

使用SSH有的时候会update失败。

解决办法：

修改~/.ssh/config，添加:

`User XXX`

参考：

https://git-scm.com/book/en/v2/Git-on-the-Server-Generating-Your-SSH-Public-Key

Generating Your SSH Public Key

## X Server

假设客户端的ip是1.1.1.1，而ssh服务器的ip是2.2.2.2。

Client:

```bash
xhost +2.2.2.2
ssh -X root@2.2.2.2
```

Server:

```bash
export DISPLAY=1.1.1.1:0.0
xclock
```

参考：

https://www.cnblogs.com/-9-8/p/5365105.html

ssh & display

## 参考

https://mp.weixin.qq.com/s/u3VSyEtdcIgp8dCbwCaavA

SSH只能用于远程Linux主机？那说明你见识太小了！

# Linux知识图谱

![](/images/article/linux_perf_tools_full.png)

![](/images/img3/linux_kernel_map.png)

原图地址：

http://www.brendangregg.com/linuxperf.html

![](/images/img4/Linux-storage-stack-diagram_v4.10.png)

参考：

https://mp.weixin.qq.com/s/-iCuxQjSghtDGaPMqaDSgQ

Linux思维导图整理

https://mp.weixin.qq.com/s/sLyD6z2xBXRxfBZnImTgtQ

40+张最全Linux/C/C++思维导图，收藏！

# Linux参考资源

https://www.kernel.org/doc/html/latest/

Linux官方文档

---

https://mp.weixin.qq.com/s/n6D5_6K9TrnuXg3h6AiFNA

华为“鸿蒙”所涉及的微内核到底是什么？一文带你认识微内核

![](/images/img3/Monolithic_vs_Micro.jpg)

![](/images/img3/UNIX.jpg)

---

https://mp.weixin.qq.com/s/I7C7cXFgxO7RO0Wpjjj3xQ

一篇文章带你“重新认识”线程上下文切换怎么玩儿

https://www.cnblogs.com/liqiuhao/p/9450093.html

关于TOCTTOU攻击的简介

https://mp.weixin.qq.com/s/2QAm6F109LV2koC64xIpqA

简直不要太硬了！一文带你彻底理解文件系统

https://mp.weixin.qq.com/s/0jR4Y3sT8RRW7FYEOmBsXg

一口气搞懂文件系统，就靠这25张图了
