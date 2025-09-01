---
layout: post
title:  推荐系统（三）——工程细节（2）
category: Recommender System 
---

* toc
{:toc}

# 推荐系统的工程细节

## 数据埋点（续）

https://mp.weixin.qq.com/s/j_8_kSQ4BrYR-UzIEIEIbw

美团开源Logan Web：前端日志在Web端的实现

https://mp.weixin.qq.com/s/Cc-8BMoJGacPCeHnMs6Ekw

滴滴开源小桔棱镜：一款专注移动端操作行为的利器

https://mp.weixin.qq.com/s/8nq6q1p5-RJPiDTEJquyNQ

无埋点核心技术：iOS Hook在字节的实践经验

https://mp.weixin.qq.com/s/C2OSscx8JzTBhyzxCqesCQ

网易云音乐宋志毅：数据凌乱，埋点差，难以归因？数据治理有妙招！

## OpenTelemetry

CNCF旗下的统一可观测性的“一次埋点，到处导出”的遥测数据流水线。

## A/B Testing

A/B Testing脱胎于版本发布中的A/B发布：通过分隔A和B两个版本，统计数据，进而看哪个版本的数据效果更好，对产品目标更有帮助。

类似的还有：

- 金丝雀发布。

人们发现金丝雀这种生物对于有毒气体很敏感。因此矿工在下井采矿之前会把金丝雀鸟儿投入或携带到矿井中，如果鸟儿能够从矿井中飞出就表示井下有氧气，矿工就可以安心下井采矿了。

部署的时候让一小部分用户先试用功能 ，通过日志监控或者服务器监控，看下新用户的反馈。如果没有严重问题，尽快部署这个新版本，否则快速会退。小代价去试错。

- 蓝绿发布。

后台服务有2个服务。一个是绿色版本，就是当前正在运行的版本。一个是蓝色版本，比当前线上版本高一个版本。就是未来要发布的版本。发布前先测试蓝色版本，保证质量一切OK。就直接切换到蓝色版本。让用户无缝衔接。

https://mp.weixin.qq.com/s/fB3AFUiS4nFmqXpSnOpf4w

如何选择上班路线最省时间？从A/B测试数学原理说起

https://cn.udacity.com/course/ab-testing--ud257

A/B测试

https://exp-platform.com/2017abtestingtutorial/

A/B Testing

https://zhuanlan.zhihu.com/p/68509372

Netflix推荐系统模型的快速线上评估方法——Interleaving

https://mp.weixin.qq.com/s/gEET3mcI4VTLLg5ICQL2xQ

如何通过A/B测试实现用户有效增长？

https://mp.weixin.qq.com/s/zBUy_k2lyK0mqMMxm01w6w

推荐系统评估

https://mp.weixin.qq.com/s/3Lvuja4fDFSiqwyEgdYxlQ

如何快速上手AB Testing？

https://mp.weixin.qq.com/s/0H2KYI3OQbmqxFQBt6-JEA

如何设计一个A/B test？

https://mp.weixin.qq.com/s/-y16GrLSMafoSF7ZT-KCiQ

A/B测试的设计和执行

https://mp.weixin.qq.com/s/45JEBLXgC6gRjAudL5fMvg

Netflix：使用A/B测试来找到最佳的插图

https://mp.weixin.qq.com/s/v3Fvp6Hed7ZGoE8FGlGMvQ

美团配送A/B评估体系建设与实践

https://mp.weixin.qq.com/s/UBAQC4xB-aPI7B7JUKE95g

携程是如何做AB实验分流的？

https://mp.weixin.qq.com/s/MwXG85RdbNqvwkjauTfk2Q

不只是A/B测试：多臂老虎机赌徒实验

https://mp.weixin.qq.com/s/ayvBYilRX0BFEYkAa2EikQ

A/B测试是好，但不适合创新

https://mp.weixin.qq.com/s/85BBYJRbvpV9CqgYjW9Ayg

推荐系统衡量：ABtest框架

https://zhuanlan.zhihu.com/p/96382982

A/B Test的统计原理和效果解读

https://mp.weixin.qq.com/s/na6hS-Mm8g-5Y4jgdqx-Eg

流量为王：ABTest流量分层分桶机制

https://mp.weixin.qq.com/s/ePKNE9r0khaLmM_zKgQh1g

没有最好，只有A/B测试！

https://mp.weixin.qq.com/s/68sMYxXu9toLAprG8S0Mmw

如何构建A/B测试系统，其核心功能有哪些？

## 参考

https://mp.weixin.qq.com/s/vpANPrl86Ou2fBVHgLXtBQ

京东电商推荐系统实践

https://mp.weixin.qq.com/s/oKayc5612vVm9k8hu50GNw

当个不“佛系”的推荐系统，CTR预估要做哪些工作？

https://mp.weixin.qq.com/s/env2bFsEWZjxB87GK8QqvQ

Hulu：视频广告系统中的算法实践

https://mp.weixin.qq.com/s/Eih4J51C8Eh-cuZ8vznESg

当推荐遇到社交：美图的推荐算法设计优化实践

https://mp.weixin.qq.com/s/8jcaMIwNJQ1408VM-ucr_g

快看漫画个性化推荐探索与实践

https://mp.weixin.qq.com/s/G5O4-ne2Ll4qJnKlQ_MWog

微博广告策略工程架构体系演进

https://mp.weixin.qq.com/s/T6eNEVJF5xLwSZ4Ui7rvZQ

推荐系统应该如何保障推荐的多样性？

https://mp.weixin.qq.com/s/lTYBrYJBnW_ZNEdsQ-lorw

透着浓浓工业风的Facebook深度学习推荐系统论文

https://mp.weixin.qq.com/s/hNz9Op4lYtaX8tznUmHd9Q

OCPC广告算法在凤凰新媒体的实践探索

https://mp.weixin.qq.com/s/WWqaCRtsSqfvO-qDLvehdg

阿里妈妈：品牌广告中的NLP算法实践

https://mp.weixin.qq.com/s/sboWpGTuf8rpRUz4G7PdKw

深度学习在美图个性化推荐的应用实践

https://mp.weixin.qq.com/s/QThtXNF9oIE5bXF5kaSg5w

如何优化大规模推荐？下一代算法技术JTM来了

https://mp.weixin.qq.com/s/Od2_u1uP5S7htjDKuEDSTw

数据质量良莠不齐？携程是这样来做多场景下的内容智能发现的

https://mp.weixin.qq.com/s/FXlxT6qSridawZDIdGD1mw

UC信息流推荐模型在多目标和模型优化方面的进展

https://mp.weixin.qq.com/s/b8DkQWZbUc5-jzWKBd8iUA

深度学习技术在美图个性化推荐的应用实践

https://mp.weixin.qq.com/s/R0GG6Kg-h50RTRU6DIrZCg

爱奇艺效果广告的个性化探索与实践

https://mp.weixin.qq.com/s/QqWGdVGVxSComuJT1SDo0Q

360展示广告召回系统的演进

https://mp.weixin.qq.com/s/zxFGxnQk016Kh72XyPDSSg

阿里文娱首次公开！AI如何对爆款内容未卜先知？

https://mp.weixin.qq.com/s/b0_o4lHOxs7v4PCCL_I4DA

Instagram推荐系统：每秒预测9000万个模型是怎么做到的？

https://mp.weixin.qq.com/s/MGeuFEQ4wvEERSrSVVihJg

推荐系统提供web服务的2种方式

https://mp.weixin.qq.com/s/BNQRi4hHnWXMDkSDbno_kg

不要犯战略性的失误——如何合理制定推荐系统的优化目标？

https://mp.weixin.qq.com/s/VFUryyuIY-cWhWK-oU9byg

推荐系统的人工调控

https://mp.weixin.qq.com/s/ZggfBVw0Z0ODxjqAC4-Lbg

淘宝搜索模型如何全面实时化？

https://mp.weixin.qq.com/s/SPtNy_1_6fiFXKukMmVPlA

深度学习在网易严选智能客服中的应用

https://mp.weixin.qq.com/s/dw988NgHl93B8sC2UFjtPg

推荐系统之数据与特征工程

https://mp.weixin.qq.com/s/TcxI-XSjj7UtHvx3xC55jg

微信读书怎么给你做推荐的？

https://mp.weixin.qq.com/s/MAfK4B2F8sPXRLodXkwnmw

Query理解和语义召回在知乎搜索中的应用

https://mp.weixin.qq.com/s/t7LYfItthkszzvRp6opG5w

机器学习在微博O系列广告中的应用

https://mp.weixin.qq.com/s/3urKkPfhi4JC7Qe2pLclsg

搜索模型核心技术公开，淘宝如何做用户建模？

https://mp.weixin.qq.com/s/0_G3jMpwJVbdJ85TajubIg

个性化海报在爱奇艺视频推荐场景中的实践

https://mp.weixin.qq.com/s/YkABleB5hlHt_Zn6M6DgBw

从零开始构建企业级推荐系统

https://mp.weixin.qq.com/s/WOXSSA5srCqaNPO-j-Ogqw

优酷：如何让视频算法为质量服务？

https://mp.weixin.qq.com/s/ewkaYPQlFZ4EvVM8e8HK-g

今日头条、抖音推荐算法原理全文详解！

https://mp.weixin.qq.com/s/NVQPv5ua9kxw1MK8UVQcuQ

广告算法在阿里文娱用户增长中的实践

https://mp.weixin.qq.com/s/QxQzGtsBUFYknvl4uOdMvw

深度学习在省钱快报推荐排序中的应用与实践

https://mp.weixin.qq.com/s/FcZ1E9EZ_nwhi1JZ5jBV0Q

机器学习加持的Airbnb体验搜索排序实践

https://mp.weixin.qq.com/s/HRGk5bfaOdj-6X4opEYA-w

美图个性化推送的AI探索之路

https://mp.weixin.qq.com/s/1TbplmZ2nMUWcft_gKX2BA

每天几十万条用户反馈，高德工程师如何提升处理效率？

https://mp.weixin.qq.com/s/NUD3i2nEJL9P-tNpcty2CQ

推荐系统的人工调控

https://mp.weixin.qq.com/s/7hvYdOTnnw5pDDMx6N66Uw

阿里文娱搜索算法实践与思考

https://mp.weixin.qq.com/s/j7j3u0YGo4DomXnJFmzoGA

万字长文讲解如何基于智能推荐的精细化运营

https://zhuanlan.zhihu.com/p/188228577

推荐系统实用分析技巧

https://mp.weixin.qq.com/s/OB2r761_8xcR2CXKS3JwYQ

在搜索引擎广告关键词生成上，算法可以做什么？

https://mp.weixin.qq.com/s/PFUjEyf3lZY_ZYL2BAxO0A

文本反垃圾在花椒直播中的应用概述

https://mp.weixin.qq.com/s/5InRjVu3naxrA_9KAD2IJA

深度学习是如何在美团点评推荐业务中实践的？

https://mp.weixin.qq.com/s/VkCio3oyoTJuiMHrpL1s4g

阿里飞猪“猜你喜欢”推荐排序实践

https://mp.weixin.qq.com/s/Q-emQCpmlibK8WAqzLTxAw

让推荐系统变得会“说话”———达观推荐理由设计实践

https://mp.weixin.qq.com/s/1Iqcx4fSCnKHALV0Z6rO3g

达观数据是如何基于用户历史行为进行精准个性化推荐的？

https://mp.weixin.qq.com/s/BffCFKpgjf-MDKMH4V_Dig

红豆Live推荐算法中召回和排序的应用和策略

https://mp.weixin.qq.com/s/b9lHY6jirAv_GKK6WM0eAg

视频推荐中用户兴趣建模、识别的挑战和解法

https://mp.weixin.qq.com/s/5LUI7ChqARFT2PUom1ue9w

今日头条推荐算法详解

https://mp.weixin.qq.com/s/824-TbpwJz9n_jT9uuOE8g

中小企业如何做好推荐系统？

https://mp.weixin.qq.com/s/6oyETsNIEdP9gz0U-UjNOA

搜索相关性算法在DiDi Food中的探索

https://mp.weixin.qq.com/s/X6xjNUMqb4en55Yd1pdOYw

外卖送达时间预估，让每位用户吃上一口热乎饭的算法

https://mp.weixin.qq.com/s/mcETNOICbabRRq9BBdL4zw

深度召回在招聘推荐中的挑战和实践

https://mp.weixin.qq.com/s/sy2GNXwwpn-yUQUIZoMuxA

淘宝at KDD 2019，解决推荐中带约束的Top-K优化问题

https://mp.weixin.qq.com/s/B_lSu5zCXFZZlbgFe7gjMQ

推荐模型为什么干不过李佳琦们

https://mp.weixin.qq.com/s/D3qxlzZgwnMprzEVuMpmgg

机器学习在高德搜索建议中的应用优化实践

https://zhuanlan.zhihu.com/p/124324598

Context-aware/Sequencial/Session-based的区别

https://mp.weixin.qq.com/s/ewb9IlqPmQgfXGEUSRNXwQ

搜索广告之自动化创意

https://mp.weixin.qq.com/s/_hGIdl9Y7hWuRDeCmZUMmg

微信“看一看”推荐排序技术揭秘

https://mp.weixin.qq.com/s/4Uw_JpGqPLNumvyar0d6Hg

万字长文读懂微信“看一看”内容理解与推荐

https://mp.weixin.qq.com/s/FLwnxDPgcziaIY94qG9jDw

社交推荐系统中的用户交互

https://www.zhihu.com/question/19662693

计算广告与推荐系统有哪些区别？

https://mp.weixin.qq.com/s/wKJ3buhA-6xtJPZ7DQ0pow

机器学习模型在携程海外酒店推荐场景中的应用

https://mp.weixin.qq.com/s/V1eKYF-5gJf7qX-I2SsJ1Q

深度CTR预估模型在应用宝推荐系统中的探索

https://zhuanlan.zhihu.com/p/221725328

推荐系统的多样性：用户视角

https://mp.weixin.qq.com/s/lDKumZHHHuA1JTKxtDdN7w

如何构建一个生产环境的推荐系统

https://zhuanlan.zhihu.com/p/259985388

从零开始了解推荐系统全貌

https://mp.weixin.qq.com/s/Hsw_IbphFotR-Q89wKpMXQ

Embedding在腾讯应用宝的推荐实践
