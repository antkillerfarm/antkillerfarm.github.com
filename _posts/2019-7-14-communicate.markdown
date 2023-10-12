---
layout: post
title:  通信协议, 超算
category: resource 
---

* toc
{:toc}

# 通信协议

## HTTPS

在HTTP时代，网络运营商有多种方式进行嗅探以搜寻用户访问的网站，这是因为HTTP没有进行加密，网络运营商可以轻松劫持HTTP流量并在流量里插入垃圾信息，包括但不限于将用户下载的应用替换为推广应用、在用户访问的任意网站里插入流量套餐或者广告信息。

最近十年HTTP快速向HTTPS过渡，HTTPS对网站内容进行了加密，运营商无法再随意劫持流量，但这并不意味着运营商无法再嗅探用户访问哪些网站。

当我们使用浏览器访问某个地址时，DNS请求也会泄露地址、SNI信息也会暴露域名，于是业界又推出DNS over HTTPS/TLS，用来加密DNS查询请求。

然而SNI问题仍然没有解决，因为SNI信息仍然会暴露给网络运营商。

在CloudFlare托管的网站若启用ECH后，则外部部分使用的是cloudflare-ech.com，这是个共享的SNI，运营商最多只能知道用户访问的网站使用的是CloudFlare和ECH标准，并不知道具体的网站域名。

VSCode在最近添加了万恶的ECH，让运营商看不到明文的域名信息。他看不到你访问的域名，就联想你在访问非法网站，于是就一股脑封了。

https://mp.weixin.qq.com/s/C68icGtwh3IzuUbANaRXEg

也许，这样理解HTTPS更容易

https://mp.weixin.qq.com/s/ewI7sHDFj1AHaNAtBDM8uA

HTTPS到底加密了什么？

https://mp.weixin.qq.com/s/IcpaoCU6WXYRTqOuC4AmKQ

用信鸽来解释HTTPS

https://mp.weixin.qq.com/s/Yn2yJ-tQOIvwr1YJZabFuw

HTTPS协议

https://mp.weixin.qq.com/s/oydBIAEqAswppme6ku70fQ

https加密那点事

https://mp.weixin.qq.com/s/iN8NQd8SuWL4gCWhRbY6tg

图解基于HTTPS的DNS

https://mp.weixin.qq.com/s/Yj5NqRJG48kKX2kNnmj9yw

20张图彻底弄懂HTTPS的原理！

https://segmentfault.com/a/1190000023936425

为什么HTTPS是安全的

https://mp.weixin.qq.com/s/BIPbe_jJr3Xm0tBi-YEAEQ

可怕，原来HTTPS也没用

## TCP

所谓粘包就是连续给对端发送两个或者两个以上的数据包，对端在一次收取中可能收到的数据包大于1个，大于1个，可能是几个（包括一个）包加上某个包的部分，或者干脆就是几个完整的包在一起。当然，也可能收到的数据只是一个包的部分，这种情况一般也叫半包。

https://mp.weixin.qq.com/s/sbBhBowAfhnaydErmoTxDg

TCP协议如何解决粘包、半包问题

---

https://mp.weixin.qq.com/s/zU1Mw3yaNmk4D5pP9vxxaw

TCP三次握手和SYN攻击

https://mp.weixin.qq.com/s/3VqdjEK4QkER4Q05JgfjhQ

详解TCP之滑动窗口

https://mp.weixin.qq.com/s/yH3PzGEFopbpA-jw4MythQ

TCP三次握手原理，你真的理解吗？

https://www.zhihu.com/question/50646354

TCP中断可以用3次挥手吗？

https://mp.weixin.qq.com/s/ct3wknW9b9oZBoXwZTh-Fg

面试官问我：一个TCP连接可以发多少个HTTP请求？我竟然回答不上来...

https://zhuanlan.zhihu.com/p/101702312

详解TCP超时与重传机制

https://mp.weixin.qq.com/s/dPgA9HKkkqDo_W7Q1qVCoQ

肝完这篇TCP/IP，我就去面试去

https://mp.weixin.qq.com/s/bt0vGltG9OAIWMk8iEQzew

15张图，把TCP/IP讲得一清二楚！

https://mp.weixin.qq.com/s/q6WWwNoXMDA3UI8XacYsBQ

TCP：一个悲伤的故事

https://mp.weixin.qq.com/s/dRojnN6BqCiTVG5edUvtMg

面试官：换人！他连TCP这几个参数都不懂

https://mp.weixin.qq.com/s/GmaZMPPUh20PEuV2n2mdPA

我们说TCP是流式协议究竟意味着什么？

https://www.zhihu.com/question/53672815

TCP链接之间的网络物理断开了然后有连上，这个TCP链接是否断开？

## DNS

https://mp.weixin.qq.com/s/DDk5YZv9Q7CaqdiKvQxZGQ

从理论到实践，全方位认识DNS（理论篇）

https://mp.weixin.qq.com/s/rZU35kVTXs5ebf7k6lnheQ

从理论到实践，全方位认识DNS（实践篇）

https://mp.weixin.qq.com/s/dVY0tPgkUq3W5waVtITFUA

一文看懂：网址，URL，域名，IP地址，DNS，域名解析

https://mp.weixin.qq.com/s/cSWjUlO-TaBrqnkt-IK6hA

一文讲懂什么是vlan、三层交换机、网关、DNS、子网掩码、MAC地址

https://mp.weixin.qq.com/s/1x1tUuPJ0fkiPj0G_uTSmg

美国如果把根域名服务器封了，中国会从网络上消失？

## URI

URI = Universal Resource Identifier

URL = Universal Resource Locator

URN = Universal Resource Name

个人的身份证号就是URN，个人的家庭地址就是URL，URN可以唯一标识一个人，而URL可以告诉邮递员怎么把货送到你手里。

参考：

https://blog.csdn.net/koflance/article/details/79635240

URL和URI的区别

## QUIC

Quic全称quick udp internet connection，“快速UDP互联网连接”，是由google提出的使用udp进行多路并发传输的协议。

Quic相比现在广泛应用的http2+tcp+tls协议有如下优势：

1.减少了TCP三次握手及TLS握手时间。

2.改进的拥塞控制。

3.避免队头阻塞的多路复用。

4.连接迁移。

5.前向冗余纠错。

参考：

https://mp.weixin.qq.com/s/vpz6bp3PT1IDzZervyOfqw

QUIC协议原理分析

https://mp.weixin.qq.com/s/Bm_4M-QCcWYRqv1V8a-J-A

QUIC协议原理浅解

https://mp.weixin.qq.com/s/_RAXrlGPeN_3D6dhJFf6Qg

让互联网更快的协议，QUIC在腾讯的实践及性能优化

https://zhuanlan.zhihu.com/p/68012355

HTTP/3竟然基于UDP，HTTP协议这些年都经历了啥？（HTTP/3之前名为HTTP-over-QUIC）

https://mp.weixin.qq.com/s/fy84edOix5tGgcvdFkJi2w

一文读懂HTTP/1 HTTP/2 HTTP/3

https://mp.weixin.qq.com/s/unzge23Aw0jrvBST-iW0Qw

HTTP/3的过去、现在和未来

https://mp.weixin.qq.com/s/MHYMOYHqhrAbQ0xtTkV2ig

HTTP/3原理实战

https://mp.weixin.qq.com/s/DiaruzlgPPKmJCiBg19_og

为什么HTTP3.0使用UDP协议

https://mp.weixin.qq.com/s/uNXxyPZ5IssZfgFFD-m5dQ

什么是HTTP简史

https://mp.weixin.qq.com/s/A4OYoKFVu2nZCyDJcoFrQA

从HTTP到HTTP/3的发展简史

https://mp.weixin.qq.com/s/Mwom82Wwzo4pK9uGSNdfSQ

xxxxHub都用上了HTTP/2，它牛逼在哪？

https://mp.weixin.qq.com/s/E4H3iQRoH0baybSPuGS7Fw

我的HTTP/1.1好慢啊！

https://mp.weixin.qq.com/s/gx5UCRcxG00YZq2Go_xsxQ

HTTP请求之合并与拆分技术详解

## UDP

UDP实现的可靠协议，基本都会对TCP的某一部分进行加强，另外一部分进行削弱。因为：

“实时性+可靠性+公平性”三者不能同时保证，因此可以牺牲TCP的局部公平性来换取更好的实时性，或者更浪费点带宽，来实现更低的延迟。

https://github.com/skywind3000/kcp

这是某牛实现的快速可靠协议，使用比tcp多浪费15%的带宽的代价，换取了平均延迟降低30%-40%，最大延迟降低两倍的传输效果。它已经被《原神》等游戏所采用。

## 参考

https://www.cnblogs.com/maybe2030/p/4781555.html

计算机网络基础知识总结

https://mp.weixin.qq.com/s/_bWOYpZNjA4p7ZWM1J_4mw

通信协议之序列化

https://mp.weixin.qq.com/s/CP_Egs4LUx7I8BXSqrS_dQ

互联网协议入门（一）

https://mp.weixin.qq.com/s/KoqN7Dq1aLhqEFAJP8V0KA

66条计算机网络的知识点

https://mp.weixin.qq.com/s/Lee4DRNCQuGCTTAsSYR8ng

高性能网络编程之accept建立连接

https://mp.weixin.qq.com/s/PAuijOCx8TLCsyU_RGs11w

从TCP协议的原理来谈谈rst复位攻击

https://mp.weixin.qq.com/s/W_JjnDuinmLrahIpPY8h-g

tcp服务端一直sleep，客户端发送数据问题总结

https://mp.weixin.qq.com/s/bUrTDj1atJSZvFeWA8H-SA

正向代理和反向代理

https://mp.weixin.qq.com/s/YQfk9lqHbepG2om7mEGTSg

如何给女朋友解释什么是反向代理？

https://zhuanlan.zhihu.com/p/32615963

什么是IPFS?(一)

https://zhuanlan.zhihu.com/p/32633235

什么是IPFS?(二)

https://zhuanlan.zhihu.com/p/32651288

什么是IPFS?(三)

https://mp.weixin.qq.com/s/F6hZpIDqxSCs0ZZ5_6fsMg

HTTP/2协议的优点解析

https://mp.weixin.qq.com/s/3-6T0YS0UsbWndu1xgrW_A

白话Session与Cookie：从经营杂货店开始

https://mp.weixin.qq.com/s/JyPuFauAycot6bvvaunNlw

把cookie聊清楚

https://mp.weixin.qq.com/s/Btlj2A329KUbRU9It79OKQ

面试环节：在浏览器输入URL回车之后发生了什么？

https://mp.weixin.qq.com/s/Mc7vpJw5r_vcbsxLXwfeNA

看完这篇HTTP，跟面试官扯皮就没问题了

https://mp.weixin.qq.com/s/LKB66Ap_L4LedJ72DcXZig

图解ARP协议（一）

https://mp.weixin.qq.com/s/5J_8hD0DnCaX4ksgsgTjHA

大量的TIME_WAIT状态连接怎么处理？

https://mp.weixin.qq.com/s/urwS1dFAZsBG0Kyl8YgLpQ

GET和POST请求的本质区别是什么

https://mp.weixin.qq.com/s/o9zxzklBOFyNkOPMR4gdnA

想避免重复请求/并发请求？这样处理才足够优雅

https://mp.weixin.qq.com/s/_wUoGwcSlJeaMH0JSjNbeA

直接用IP访问知乎，我发现了一个秘密···

https://mp.weixin.qq.com/s/5Fs_bY9zNs47hfN8PBd_CQ

网络编程基础知识

https://mp.weixin.qq.com/s/EuOap3a9Ceo31yZbHoRLCg

动图：秒懂各种常用通信协议原理

http://ruanyifeng.com/blog/2016/04/cors.html

跨域资源共享CORS详解

https://blog.csdn.net/cuixiaogang110/article/details/81948173

前后端分离与跨域的解决方案（CORS的原理）

# 超算

Massively Parallel Processor

![](/images/img3/power.aug.gif)

![](/images/img3/Supercomputers-history.png)

Top 500超算之间的差距竟有3个数量级，从榜首到落榜差不多要10年时间。OS从2015年开始全都是Linux了。Windows在超算领域从来没有风光过，之前没钱，自然斗不过UNIX，后来又被Linux打趴下了。

Sunway TaihuLight和Sierra的算力相当，但core的数量竟是后者的6.7倍，功耗是后者的2.06倍。差距明显啊！

https://www.top500.org/

超算排名网站

https://zhuanlan.zhihu.com/p/33956771

超算排名之中的地区和架构之争

---

![](/images/img3/SC.png)

https://mp.weixin.qq.com/s/gJWTiMCovGMQ8ye_TovdOw

富士通的这颗芯片凭啥让日本走向了世界之巅？

https://www.zhihu.com/question/404217836

如何看待全球超级计算机TOP 500榜单日本登顶，中国跌出前三？近年中国超算发展现状如何？

---

传统的排名是基于涉及64位浮点计算的基准，除此之外还有其他基准。

2021年7月，由国防科技大学研制，部署在国家超级计算天津中心的“天河”E级计算机关键技术验证系统在国际Graph500排名中，获得SSSP Graph500（单源最短路径）榜单世界第一和BIG Data Green Graph500（大数据图计算能效）榜单世界第一的成绩。

---

https://www.zhihu.com/answer/2512513124

如何看待美国新的超级计算机Frontier成为超算榜全球第一，超过2–8名计算能力之和？
