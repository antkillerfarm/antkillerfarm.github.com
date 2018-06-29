---
layout: post
title:  深度学习（三十八）——深度强化学习（2）, Multi-agent, CNN进阶, 图像变换
category: DL 
---

# 深度强化学习（续）

## 参考

https://mp.weixin.qq.com/s/K2DW_ntSWrlySpxgorF9dA

Python强化学习实战，Anaconda公司的高级数据科学家讲解

https://zhuanlan.zhihu.com/p/32089849

概要：NIPS 2017 Deep Learning for Robotics Keynote

https://mp.weixin.qq.com/s/CCQOHRCAolsorm8FEPdjoQ

什么时候强化学习未必好用？

https://mp.weixin.qq.com/s/I8IwPCY6-zocJKFXMr6rUg

深度强化学习的18个关键问题

https://mp.weixin.qq.com/s/gFHbLF-q91sddMAX1CRbEQ

俞扬：“审时度势”的高效强化学习

https://mp.weixin.qq.com/s/lstCIiNs_qA6k7GCYUBv2w

阿尔伯塔大学提出新型多步强化学习方法，结合已有TD算法实现更好性能

https://mp.weixin.qq.com/s/af10gqXNfohPEISSnpJ0tQ

UCB吴翼&FAIR田渊栋等人提出强化学习环境Hourse3D

https://mp.weixin.qq.com/s/ybyZpaHr-JJg7CCdXGOl5A

Seq2seq强化学习实战

https://mp.weixin.qq.com/s/TUk1PWT9CfPGEW77UKxpjw

三招武林绝学带你玩转“强化学习”

https://mp.weixin.qq.com/s/_dskX5U8gHAEl6aToBvQvg

从Q学习到DDPG，一文简述多种强化学习算法

https://mp.weixin.qq.com/s/p2hlc2PsLgrvxOF8wBZANg

李飞飞高徒范麟熙解析强化学习在游戏和现实中的应用

http://mp.weixin.qq.com/s/EPbKE-TAnAPugJDhXHEyNA

DeepMind开源Psychlab平台——搭建AI和认知心理学的桥梁

https://mp.weixin.qq.com/s/4qeHfU9GS4aDWOHsu4Dw2g

记忆增强蒙特卡洛树搜索细节解读

https://mp.weixin.qq.com/s/Vdt5h8APAFoeVxFYKlywpg

用DeepMind的DQN解数学题，准确率提升15%

https://mp.weixin.qq.com/s/1zJyw67B6DqsHEJ3avbsfQ

DeepMind推出分布式深度强化学习架构IMPALA，让一个Agent学会多种技能

https://mp.weixin.qq.com/s/xJ_g3BvbM-WaIyLthHdhEw

DeepMind发布通用强化学习新范式，自主机器人可学会任何任务

https://mp.weixin.qq.com/s/_lmz0l1vP_CQ6p6DdFnHWA

谷歌大脑工程师的深度强化学习劝退文

https://mp.weixin.qq.com/s/To3pnx1hVq_4p7UnQVMw9A

斯坦福大学&DeepMind联合提出机器人控制新方法，RL+IL端到端地学习视觉运动策略

https://mp.weixin.qq.com/s/U0K79ELLj4wsOR4sd5G4Vw

Vicarious详解新型图式网络：赋予强化学习泛化能力

https://zhuanlan.zhihu.com/p/29019246

基于策略的增强学习

https://mp.weixin.qq.com/s/C8hsGkHGtoaS9Vzm6Ub4tw

Berkeley提出“随机搜索”训练线性策略，提高RL的性能

https://mp.weixin.qq.com/s/SckTPgEw7KWmfKXWriNIRg

浅谈强化学习的方法及学习路线

https://mp.weixin.qq.com/s/vYDb1rTdPxO1sIS38VX5xA

DeepMind的AI学会了画画，利用强化学习完全不需人教：SPIRAL

https://mp.weixin.qq.com/s/rpPN2rgru6krRz2fr1RhsQ

模拟世界的模型：谷歌大脑与Jürgen Schmidhuber提出“人工智能梦境”

https://simoninithomas.github.io/Deep_reinforcement_learning_Course/

老外写的简易深度强化学习入门

https://mp.weixin.qq.com/s/AelAD57G4GOh7qm-_rvYsg

伯克利提出DeepMimic：使用强化学习练就18般武艺

https://zhuanlan.zhihu.com/p/35567591

强化学习在关系抽取、QA场景的应用

https://mp.weixin.qq.com/s/oOslkEklaZSbRb8eDDCRBw

天津大学、东京大学等研究：用深度强化学习检测模型缺陷

https://mp.weixin.qq.com/s/DNT9rMynbN4Th0AVDHeY_w

BAIR提出人机合作新范式：教你如何高效安全地在月球着陆

https://zhuanlan.zhihu.com/p/36375292

最前沿：当我们以为Rainbow就是Atari游戏的巅峰时，Ape-X出来把Rainbow秒成了渣！

https://mp.weixin.qq.com/s/KqLCTSYk1C0wYpJw-hpc1g

论强化学习和概率推断的等价性：一种全新概率模型

https://mp.weixin.qq.com/s/FROyReDu7i5amGv-J4cmtg

“世界模型”实现，一步步让机器掌握赛车和躲避火球的技能

https://mp.weixin.qq.com/s/curzH8WrFyl5WWT4lDfpwQ

Chrome暗藏的恐龙跳一跳，已经被AI轻松掌握了

https://mp.weixin.qq.com/s/1b3_AiFhwXqxb7FozdRYIQ

最in强化学习+NLP技术分享会

https://mp.weixin.qq.com/s/RKQb7-mQ-ELRRq18db02Pg

DeepMind大突破！AI模拟大脑导航功能，学会像动物一样“抄近路”

https://mp.weixin.qq.com/s/qBwszD9rn4gKazXdwqexSQ

MIT提出使用“深度强化学习”帮助智能体在运动中做出“动作决策”

https://mp.weixin.qq.com/s/i-udn1M4kiJpF8U7u5Uepg

专家解读DeepMind最新论文：深度学习模型复现大脑网格细胞

https://mp.weixin.qq.com/s/AS1VFjBFnSk19QJ28tBVWA

NIPS 2017斯坦福赛题大公开：强化学习模拟人类肌肉骨骼模型

https://mp.weixin.qq.com/s/TWjFWe6-dZWDoTi5gN1BxA

深度强化学习在指代消解中的一种尝试

https://mp.weixin.qq.com/s/YnMgJDAh3XhyyNdI8RXmtw

腾讯知文等提出新型生成式摘要模型：结合主题信息和强化训练生成更优摘要

https://mp.weixin.qq.com/s/LZbprcnben6vPqsoC1DgDA

DeepMind提出元梯度强化学习算法，显著提高大规模深度强化学习应用的性能

https://mp.weixin.qq.com/s/JbCIBFDRvg5qcpZ11g58dw

DeepMind提出关系性深度强化学习：在星际争霸2任务中获得最优水平

https://mp.weixin.qq.com/s/SpCorsAiTWGYGiohT_gZtg

如何训练智能体Agent玩毁灭战士ViZDoom？

https://mp.weixin.qq.com/s/ewC2_8O29Gg0blxK7mHsYA

比TD、MC、MCTS指数级快，性能超越A3C、DDQN等模型，这篇RL算法论文在Reddit上火了

https://mp.weixin.qq.com/s/Q9QzXo_M3jYKjBWSsLYwgQ

强化学习+树搜索：一种新型程序合成方法

https://mp.weixin.qq.com/s/2SOHQElaYbplse3QqG9tYw

强化学习介绍

# Multi-agent

https://mp.weixin.qq.com/s/bQx-ydEAfijgK1Ii5ilPFQ

UCL汪军团队新方法提高群体智能，解决大规模AI合作竞争

https://mp.weixin.qq.com/s/4lj_G2z0Lbfk5mOgvxLC6w

对抗式协作：一个框架解决多个无监督学习视觉问题

https://mp.weixin.qq.com/s/0v57oHMEDcJuUivs8D5pnQ

善于单挑却难以协作，构建多智能体AI系统为何如此之难？

https://mp.weixin.qq.com/s/tME5GsKEXveVcgWb-Zbx_A

乔明达ICML 2018论文提出协作学习的鲁棒性方法

https://mp.weixin.qq.com/s/0Xuxr7CXqsxrssW2I6NCBQ

通用智能化：BAIR简述人类-机器人协作新技术

https://mp.weixin.qq.com/s/GNbCu1lbOmwJDCU3vgMbtQ

OpenAI发布多智能体深度强化学习新算法LOLA

https://mp.weixin.qq.com/s/5Go20MyBxdVI1r5SkwA6lw

面向星际争霸：DeepMind提出多智能体强化学习新方法

https://mp.weixin.qq.com/s/TlcVxjhHXXGSpIvptC4H8g

使用Gym和CNN构建多智能体自动驾驶马里奥赛车

https://mp.weixin.qq.com/s/zRcq1LMtK72Cl4T3877tgQ

牛津教授吐槽DeepMind心智神经网络，还推荐了这些多智能体学习论文

https://mp.weixin.qq.com/s/qU6XD45RGeMI-T7GI0dg2Q

OpenAI竞争性自我对抗训练：简单环境下获得复杂的智能体

https://mp.weixin.qq.com/s/rPsxvzOLuoo_IRtgH94Ecw

OpenAI自学习多智能体5v5团队战击败人类玩家

https://mp.weixin.qq.com/s/VPVzXzBYD_nYO1SjTMNGVA

Dota2团战AI击败人类最全解析：能团又能gank，AI一日人间180年

https://mp.weixin.qq.com/s/cUFSaHWGKhyZhEvRdX09hw

谷歌大脑QT-Opt算法，机器人探囊取物成功率96%

https://mp.weixin.qq.com/s/xxB1lsiQxSfaHPKwZUI9bw

拔掉机器人的一条腿，它还能学走路？——三次元里优化的DRL策略

# CNN进阶

https://mp.weixin.qq.com/s/gwH9s1ggMTj2dJkad9wUuw

从VGG到NASNet，一文概览图像分类网络

https://mp.weixin.qq.com/s/hIAIbpqItS09KDOSFxaeqg

从Inception v1到Inception-ResNet，一文概览Inception家族的“奋斗史”

https://mp.weixin.qq.com/s/r143qYj8bziu_N-27RWRRw

机器学习5年大跃进，可能是个错觉

# 图像变换

https://mp.weixin.qq.com/s/88tBoAVziGEFXIGfvZZp8g

深度学习图像处理项目集锦：生成可爱的动漫头像，骡子变斑马等入选

https://mp.weixin.qq.com/s/iV-OXiKF1jgAhSmX4QUIXw

综述：图像风格化算法最全盘点

https://mp.weixin.qq.com/s/Zq3ngbTAObQuIr86nkTwjQ

视频换脸技术，女神都下海了吗？

https://mp.weixin.qq.com/s/eqI5fVuF68RQWb1a5O219w

腾讯研发“一键卸妆” ,让女神秒变路人!

https://mp.weixin.qq.com/s/joRcvCQwLYgRd29mfPd7XA

中科大&微软提出立体神经风格迁移模型，可用于3D视频风格化

https://mp.weixin.qq.com/s/73mkWlqJsVdu9m1kPDvfbQ

用AI让静图变动图：CVPR热文提出动态纹理合成新方法

https://mp.weixin.qq.com/s/gEFzogsteK_1VeywbQxbgQ

(GAN)延时摄影视频的生成

https://zhuanlan.zhihu.com/p/34042498

深度解密换脸应用Deepfake

https://mp.weixin.qq.com/s/gvPTNtrd5Du9Oablp3jZYw

如何使用DeepFake实现视频换脸

https://mp.weixin.qq.com/s/BTzV7ulweqFQokdQ-AX2Rg

三位一体的纯正视频换脸术，拒绝别人的嘴替我说话

https://mp.weixin.qq.com/s/rPDvLnG4MBDRUMCWs2fjcQ

最新StarGAN对抗生成网络实现多领域图像变换

https://mp.weixin.qq.com/s/97Uj-ATLToy1bNhnSUO8Jw

非监督任意姿势人体图像合成

https://mp.weixin.qq.com/s/cfw8mRsmzE1lU8PRM8UC0w

秒变莫扎特、贝多芬，Facebook提出完美转换音乐风格的神经网络

https://mp.weixin.qq.com/s/TiXILy4l6Q3dxVwapyd9KQ

二维码太丑？用风格迁移生成个性二维码了解一下

https://mp.weixin.qq.com/s/7GHBH79kWIpEBLYX-VEd7A

CycleGAN：图片风格，想换就换

https://mp.weixin.qq.com/s/tzPCU1bxQ7NWtQ7o2PjF0g

BAIR提出MC-GAN，使用GAN实现字体风格迁移

https://mp.weixin.qq.com/s/RMz5fwt72j2ufrGpdD6POw

带自注意力机制的生成对抗网络，实现效果怎样？

https://mp.weixin.qq.com/s/PVM7wMsT6TpJkQUlt7d2Aw

用风格迁移搞事情！超越艺术字：卷积神经网络打造最美汉字

https://mp.weixin.qq.com/s/12_Gl4snq-LdMHJSZn4oOA

换脸AI升级版：面部表情、身体动作、视线方向都能实时迁移

https://mp.weixin.qq.com/s/BBg04rDtiqU-XrWortufNA

康奈尔&英伟达提出多模态无监督图像转换新方法

https://mp.weixin.qq.com/s/A2VhfO3CkyQGCs5GqBWzOg

实景照片秒变新海诚风格漫画：清华大学提出CartoonGAN

https://mp.weixin.qq.com/s/lPzPfjYiAsNvVcVWdL08dA

照片闭眼也无妨，Facebook黑科技完美补全大眼睛

https://mp.weixin.qq.com/s/h-mp7_oO9aZ1yYxiJMLziQ

作画、写诗、弹曲子，AI还能这么玩？

https://mp.weixin.qq.com/s/txJAnu4FOOjmLhbtGTM-BQ

还敢吹「毫无PS痕迹」？小心被Adobe官方AI打脸

https://mp.weixin.qq.com/s/a1Qg1Hl5NMvEJPXhJR-2BA

效果惊艳！北大团队提出Attentive GAN去除图像中雨滴

https://mp.weixin.qq.com/s/iK7XR0tHV_dE0p1grNQIHw

只需一张照片，运动视频分分钟伪造出来

https://mp.weixin.qq.com/s/-j4p7nUF-rCGk6yK0nccvw

机器人也会画漫画

https://mp.weixin.qq.com/s/3Aq1HXpBzgNdcB130tCKbQ

GAN网络图像翻译机：图像复原、模糊变清晰、素描变彩图

https://mp.weixin.qq.com/s/fMtuJbWG_d9zyCZ0oYyX_w

经得住考验的“假图片”：用TensorFlow为神经网络生成对抗样本

