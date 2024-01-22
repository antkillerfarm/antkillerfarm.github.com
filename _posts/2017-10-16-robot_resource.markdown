---
layout: post
title:  机器人/无人驾驶参考资源（一）
category: resource 
---

* toc
{:toc}

# 机器人/无人驾驶参考资源

由于已经有了强化学习系列blog和资源，因此本资源中不再包含强化学习的内容。但毫无疑问，强化学习才是这方面的主角。

此外，SLAM、Apollo、传感器（主要是传感器的物理原理，不包括传感器数据的融合处理。）系列也有单独的资源，这里不再列出。

## 前言

2004年，DARPA举办了第一届“DARPA”挑战赛（无人车挑战赛）。根本没有一支参赛队伍完成这次比赛任务，这件事对来自斯坦福大学的Sebastian Thrun刺激很大。

于是，他在2005年组建了自己的无人车队，并且成为这项挑战赛上的第一支完赛队伍，获得奖金200万美元。这笔奖金虽然巨额，但事实上Sebastian Thrun在这个项目的投入远远大于200万美金。

Sebastian团队另外一大贡献就是在完成挑战赛后，发表了一篇著名的paper：

《Stanley: The Robot that Won the DARPA Grand Challenge》

## 行业

在那个马车盛行的年代里，汽车经常遭到人们的嘲笑，甚至认为它是马路上的怪物。面对众人的嘲笑和羞辱，本茨竟然不敢在实验室外公开测试自己发明的汽车。

1888年8月5日清晨，他的夫人贝尔塔·本茨为了回击社会舆论的讥讽，带领两个儿子驾驶着本茨的最新作品——奔驰3号，从曼海姆出发，途经维斯洛赫添油加水，直驶普福尔茨海姆，总共花费了12个小时才完成了这段长达106公里的旅行。

---

1894年，法国人举办了一次汽车比赛，130公里的总路程，102辆车参赛，其中7辆酒精车，30辆汽油车，4辆电动车，28辆蒸汽车，其他车辆33辆，最后只有9辆车跑完全程，轻松夺冠的是蒸汽汽车。

然而到了1899年，电动车的性能就稳稳超过了其他车辆，跑出了105码的惊人速度。操作简单无需换挡，没有噪音震动废气。同时代蒸汽车要不停加水，更夸张的是启动时，有45分钟的预热时间。而汽油车，虽没有预热时间但是要人力启动。这10年间电动车是一家独大的。

一直到1912年，内燃机车迎来自己的技术飞跃。震动和噪音大幅减少，动力足续航强，而电池技术却限于瓶颈，没有大的进步，辉煌十年匆匆退场。

---

第一辆无人地面车辆是西班牙发明家莱昂纳多·托雷斯·奎韦多（Leonardo Torres-Quevedo）于1904年制造的无线电遥控三轮车。在第一次世界大战期间，军队使用了各种小型，无线电控制的车辆来运送和引爆火药。

1912年，美国无线电控制设备专家小约翰·哈蒙德(John Hammond Jr.)和本杰明·密斯纳(Benjamin Miessner)利用一个电子回路和一对光感性硒光电管设计了一款简单的自动引导小车，并给它起了一个凶悍的名字——“战争狗”。战争狗的设计原理很简单，左右光感电管感知环境的光强差异，电子回路构成的底层控制系统根据光强信号控制小车转向，如果两侧感光存在差异，小车将向光强一侧转向；如果两侧感光均衡，小车保持直行。

1925年，Houdina无线电控制公司的创始人前美国陆军前工程师Francis P. Houdina发明了一种无线电操纵的汽车。Houdina将他发明的汽车命名为“美国奇迹”(American Wonder)，他在纽约繁忙的街道上公开展示了他的无线遥控无人驾驶汽车，当天碰巧赶上一个示威游行，他操纵汽车穿越拥挤的交通，从百老汇开到了第五大道，引起了巨大的轰动。

1958年，通用汽车公司和美国无线电公司(RCA)的研究团队合作组装出一套车辆侦测与引导系统，在内布拉斯加州林肯市郊区一条长400英尺、专门改造过的高速公路上，利用两辆1958年款雪佛兰进行了测试。测试车辆基于侦测与引导系统实现了前后车距保持以及自动转向的功能。

测试车辆前端配备有两个等距分置的金属“传感线圈”，与每个传感器线圈匹配的是一套测量设备，用于测量其中通过的电流强度。当汽车从道路上方驶过时，埋在地下的矩形回路会产生磁场，而这个磁场又会引发车载传感线圈产生电流。如果车辆正确地行驶在道路中央，两个传感线圈中产生的电流将会大致相当。

然而当汽车危险地偏向了道路的一侧，这侧的传感线圈就会产生更强的电流，对应的传感器也会记录下相较于另一侧较高的数值，接收到较强信号的传感器就会向汽车的方向盘操控系统发送指令，要求车辆轻微转向，直到两侧的传感器测量数值再次平衡。

70年代以后自动化高速公路的研究逐渐停止了，自动化高速公路的美梦破灭的主要原因之一就是——成本，安装必备的电缆和路边控制系统是一项耗资巨大却又见效缓慢的工程，装配一条短小的测试跑道所需的成本还算合理，但是对于美国或欧洲那些横跨各州的浩大公路网而言，方案就显得不切实际了。

1966年到1972年间，美国斯坦福国际研究所(SRI)成功研制了世界上第一个真正可移动和感知的机器人Shakey。研究人员为Shakey装备了电视摄像机、三角法测距仪、碰撞传感器、驱动电机以及编码器，并通过无线通讯系统由二台计算机控制。Shakey具备一定人工智能，能够自主进行感知、环境建模、行为规划和控制，这也成了后来机器人和无人驾驶的通用框架。

1977年，日本筑波工程研究实验室的S.Tsugawa和他的同事们开发出了第一个基于摄像头来检测导航信息的自动驾驶汽车。这辆车内配备了两个摄像头，并用模拟计算机技术进行信号处理。时速能达到30公里，但需要高架轨道的辅助。

1961年斯坦福大学的博士候选人詹姆斯·亚当斯制造了一个原型车，这辆车后来被称为斯坦福推车，用于测试火星探测车的可行性。他的试验失败了，因为测试车的延迟竟然达到了2.5秒。

1977 年，还在斯坦福大学人工智能实验室(Stanford AI Laboratory)读博士的汉斯·摩拉维克（Hans Moravec）为斯坦福推车研制了一台配备立体视觉和电脑远程控制系统，电视摄像机安装在车顶栏杆上，从几个不同的角度拍摄照片，并将其传送到电脑，电脑计算小车和它周围的障碍物之间的距离，并操纵小车绕过障碍物。1979年，推车在没有人干预的情况下，花了大约5个小时成功地穿过了一个放满椅子的房间。

https://mp.weixin.qq.com/s/7rz8QXPNda4PVs3KcweQMg

自动驾驶往事（一）：从无线电控制到机器视觉

---

天天鄙视特斯拉没有核心竞争力，但对手稍一降价，立马满地打滚，哭喊撒泼，说什么价格战伤人伤己，不知道的还以为特斯拉拜了崆峒派，学会了七伤拳。

---

某同事的回顾：

我从2010年深圳先进院为起点从事机器人研发接近14年，从2008年开始做导航系统接近16年，这期间有很多机器人公司倒下，也见证了机器人从demo样机到产品成熟推向市场，从一台机器人bom成本接近百万，到bom成本进入万元以内，见证了供应链和行业的逐渐成熟，见证了语音交互从ibm的单词识别到今天的LLM，图像领域从模式识别到深度学习，到多模态大模型识别，导航系统从惯导加二维码到今天SLAM为主的激光、视觉、SINS、GNSS、超声、毫米波雷达、二维码、UWB等等组合导航系统，激光雷达从单线到多线，从国外一家独大到今天的国内百家争鸣，现在的多线激光雷达价格不到10年前国外单线激光雷达的零头，国产卫星导航系统价格更是降了几十倍。

---

Tier 1：博世，大陆，采埃孚，博格华纳。

---

智驾F4：地大华魔（地平线、大疆、华为、Momenta）

---

![](/images/img2/ADAS.jpg)

https://mp.weixin.qq.com/s/Okolok3ZLZhgw0TQ_TROKA

一文看尽国内智能驾驶格局：三条技术路线和玩家鏖战2020年

---

![](/images/img4/car.png)

![](/images/img4/car_2.png)

https://mp.weixin.qq.com/s/9huDW1RH1gjJpMkATEguug

汽车芯片分析（应用 · 规模 ·趋势）

---

https://mp.weixin.qq.com/s/oi5ME4QRTt3U526lzAHTcA

滴滴重磅发布：KDD2018大会187页人工智能+交通教程

https://mp.weixin.qq.com/s/BUwu6R83klL3E5tI7TEX9A

一文带你看懂自动驾驶

https://mp.weixin.qq.com/s/4PPiwJJWPi4qn5mBwX6wDw

ADAS处理器、芯片主流企业及相应技术梳理

https://mp.weixin.qq.com/s/xgxsmcMAqstzmhTJZFZIyw

特斯拉重磅深度：T E S L A，T-S.E.X.Y

https://mp.weixin.qq.com/s/uei9W13lOp_bbcobv4VvlQ

一张版图带你摸清全球10大自动驾驶联盟布局

https://mp.weixin.qq.com/s/fG_Oee0biudXyX-B-93THw

史上最全的自动驾驶研究报告（上）

https://mp.weixin.qq.com/s/Qe6ex06SFbI1ggq9ONZYFA

史上最全的自动驾驶研究报告（下）

https://mp.weixin.qq.com/s/iQ9vnGX7TjtMP_UOmvVjlA

特斯拉深度研究报告

https://mp.weixin.qq.com/s?__biz=MzI1ODYwOTkzNg==&mid=2247497740&idx=3&sn=eedfb5f889b80fde545d89f5fb29238d

中国汽车工业沉浮70年

https://mp.weixin.qq.com/s/myAUMq4hknEcZ7X2Xx455g

万字长文回顾智能驾驶进化史

## 课程

http://selfdrivingcars.mit.edu/

MIT 6.S094: Deep Learning for Self-Driving Cars

>主讲：Lex Fridman，MIT博士后。   
>个人主页：   
>http://lexfridman.com/

这个课程不仅提供了课件，还提供了DeepTraffic和DeepTesla两个小工具供学生验证自己的算法。

参考：

https://mp.weixin.qq.com/s/cPyWAD-t-qLHfZJT1To2wQ

自动驾驶“老司机”拼车技，MIT的这个比赛已经飙到了时速123公里

这两个工具是用ConvNetJS编写的。

ConvNetJS是Andrej Karpathy编写的基于JavaScript的DL框架。

官网：

http://cs.stanford.edu/people/karpathy/convnetjs/

代码：

https://github.com/karpathy/convnetjs

参考：

https://mp.weixin.qq.com/s/9jS7T51kMDhmzaOp9SI0oA

从Brain.js到Mind，一文收录11个移动端Javascript机器学习库

---

http://www.eecs.wsu.edu/~taylorm/16_483F/index.html

CptS 483: Introduction to Robotics

http://www.eecs.wsu.edu/~taylorm/2011_cs420/index.html

CS 420：Artificial Intelligence

http://ctms.engin.umich.edu/CTMS/index.php

这个网页提供了很多机械的控制算法——从系统、控制到仿真的全过程的教程。

https://github.com/Microsoft/AutonomousDrivingCookbook/tree/master/AirSimE2EDeepLearning

这是微软提供的自动驾驶课程

https://www.doc.ic.ac.uk/~ajd/Robotics/index.html

CS 333: Robotics

https://mp.weixin.qq.com/s/T7wI4cjKbciYC3ZNxKfReA

143页 MIT自动驾驶算法地图推理教程
