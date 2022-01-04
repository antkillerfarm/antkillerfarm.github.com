---
layout: post
title:  这些年微软相关的技术总结, WxWidget, WebGL
category: technology 
---

* toc
{:toc}

# VS

## 1.选择CPU的类型
使用过EVC的朋友都知道，EVC支持诸如ARMV4、ARMV4I、MIPS、X86等多种CPU类型。但是除了STANDARD SDK之外，其他的SDK通常都是限定了CPU类型的。例如PPC2003是ARMV4的，而Mobile5是ARMV4I的。

大家都知道，ARMV4、ARMV4I是两套颇有渊源的指令集，前者编的程序可以运行在支持后者的机器上，但反过来则不行。这就带来了一个新的问题。

最近我负责向一个PPC2003的程序添加新功能，该功能是由第三方以静态库的方式提供的，那个库是ARMV4I的。在默认情况下，是不能链接到PPC2003程序中的。由于这个程序比较复杂，用Mobile5重新编过，需要对代码作较大修改。所以可以考虑修改链接器的CPU类型。当然这种方法只是一种偷懒的方法，常会产生一些深层次的链接错误，并不推荐。

方法：

1）EVC

Project->Settings->Link->General->Project Options，将/MACHINE:ARM改为/MACHINE:THUMB。

2）VS2005

Project->Properties->Linker->Command Line->Additional options,将/MACHINE:ARM改为/MACHINE:THUMB。

类似的我们还可以修改链接内核版本，例如我们用Windows Mobile 5 SDK编译程序，如果想让它的行为与PPC 2003相同的话，可以将/subsystem:windowsce,5.01改为/subsystem:windowsce,4.20

这个方法在VGA编程时是很有用的，PPC 2003下VGA是320 * 240的分辨率，而WM5下是640 * 480的分辨率。有时可能需要用这个方法移植一些老程序。

## 2.LNK2005问题

http://www.cnblogs.com/hyamw/archive/2007/01/11/618021.html

http://topic.csdn.net/t/20050525/17/4035191.html

当然还有终极大招——命令行的/FORCE选项，不过这是不得已而为之。明知有问题，却采用此暴力流，链接是没问题了，但链接后的东西是否可用，那就只有天知道了。

## 3.

最近做一个嵌入式的项目，需要做一个全屏的MFC对话框。刚接到手时，着实没怎么在意这个小东西。岂料刚开始做，问题就出来了。VC的对话框编辑器使用DLU作为长度单位，而我的项目需要以像素为单位。经过无数实践和查找资料后，我终于找到了一个方法。

1）创建一个Bitmap资源，图片的大小与你所需大小一致。

2）在对话框中添加该图片，按照图片的大小调整对话框的大小即可。

在EVC4下我试过该方法，但不成功。网上有的人说VC 6和VC 2005下，同样的字体、大小，DLU和像素之间的换算值是不同的，估计这也是微软在新形势下对工具的一种调整。DLU这种东西，问世的年代比较久远，它主要解决的是在分辨率较小的年代，如何清晰显示字体的问题。在当时这是个首要问题，但现在UI设计的关键已经转移到窗口的贴图上，对于图片而言，像素才是标准单位。

这里需要注意的是1 DLU在横向和纵向上对应的像素值，随设备不同而不同，在我实验的几款设备中，用上面的方法基本横向都没问题，但纵向就不一定正确了。

最后再提一下控件的Z值问题。虽然在对话框编辑器中，没有明显的地方设置Z值（Z值决定当两个控件重叠时，谁在上面），但其实是可以修改的，调整一下控件的TAB值即可。

## 4.

自绘按钮时，发现即使使用了DT_VCENTER来绘制字体，字体在按钮上的位置也不居中，后来发现还需要添加DT_SINGLELINE才行。

## 5.

在MFC中要实现换行效果，除了要在字符串中添加\n之外，还需要将DrawText设置成~DT_SINGLELINE。

## 6.

执行.bat文件时，如果不想让它运行完后直接关闭窗口，可以为该文件创建快捷方式，右键点击该快捷方式，在“属性”的“快捷方式”页的“目标”栏的最前面添加`%comspec% /k`。

## 7.如何在bat文件中编写脚本，使得启动命令行之后，能在命令行下执行命令？

`cmd /k dir`

如上所示的命令，在启动命令行之后，会在命令行下执行dir命令。

## 8.工程向导

某些SDK可能比较古老，没有匹配VS2005的向导文件，可以采用以下办法试试：找到向导对应的.vsz文件，将Wizard所在行改为Wizard=VsWizard.VsWizardEngine.8.0，但不同版本的向导之间，还是有差别的，因此这一招在某些情况下，可能无效。

## 9.

win32控制台工程，如果在运行时，不希望有控制台窗口，只要在程序中加上：

 `#pragma comment( linker, "/subsystem:\"windows\" /entry:\"mainCRTStartup\"" )`

反过来如果某个win32控制台工程运行过快，以至于看不清控制台窗口的输出时，可以在主程序的末尾加上：

`system("pause");`

## 10.自定义宏

环境变量的有效范围是整个系统，如果想定义仅针对项目的环境变量，可以使用自定义宏。

视图->属性管理器->在debug或release右击添加新项目属性表->在自己的属性表(PropertySheet)里面添加宏。之后在需要使用自定义宏的地方使用$()把自定义宏包裹起来即可。

# Windows

## 快速在指定文件夹打开命令行

按住Shift键不放，同时右击鼠标，这时在出来的右键菜单里会出现一个"打开命令行" 的菜单选项。

# Windows Mobile

## 1.自动打开微软蓝牙

使用BthUtil.dll中的BthSetMode函数。

## 2.改变音量

使用waveOutSetVolume函数。

## 3.设置震动、静音

使用aygshell.dll中的SndSetSound函数。

## 4.关闭输入法

使用SipShowIM函数。

## 5.隐藏/显示 输入法、任务栏

使用SHFullScreen函数。如果是MFC对话框的话，还需要添加以下代码才能实现输入法的隐藏：

`m_bFullScreen = FALSE;`

## 6.在VS2005下创建MFC工程

EVC下将MFC的工程分为PPC和wince两种，而VS2005下，不再区分这两者。但通常情况下PPC的程序在wince设备上并不能运行。这时可以采用以下方法：

1）有SDK时，在建立工程时，选择Platform即可。

2）无SDK时，在工程设置的预定义宏中，去掉WIN32_PLATFORM_WFSP或WIN32_PLATFORM_PSPC宏，前者表示Smartphone，后者表示Pocket PC。这个宏有时会定义在$PLATFORMDEFINES中。

## 7.设置全屏

```c
        // 隐藏任务栏
        HWND hWndTaskBar = ::FindWindow(TEXT("HHTaskBar"), NULL);
        if (NULL != hWndTaskBar)
        {
            ::ShowWindow(hWndTaskBar, SW_HIDE);
        }

        // 如果需要的话，隐藏输入法窗口
        SIPINFO sipInfo;
        memset(&sipInfo, 0, sizeof(SIPINFO));
        sipInfo.cbSize = sizeof(SIPINFO);
        ::SipGetInfo(&sipInfo);

        if ((sipInfo.fdwFlags & SIPF_ON) == SIPF_ON)
        {
            ::SipShowIM(SIPF_OFF);
        }

        // 隐藏“拼”按钮
        HWND hWndSipButton = ::FindWindow(TEXT("MS_SIPBUTTON"), NULL);
        if (NULL != hWndSipButton)
        {
            ::ShowWindow(hWndSipButton, SW_HIDE);                           
        }
```

## 8.打电话

使用tapiRequestMakeCall函数。

# Visio

## 如何保存成图片

1.选中打算保存的区域。

2.“文件”->“另存为”。

3.如果需要缩放的话，调整保存的大小即可。

## Visio画各种括号(大括号，中括号等)

依次点击“文件(File)”->“形状(Shapes)”->“其他Visio方案(Visio Extras)”->“标注(Callouts)”即可。

# Outlook

## “同步问题/本地故障”文件夹过大

最近Outlook提示邮箱文件过大，无法接收新消息。经查看之后发现问题出在“同步问题/本地故障”文件夹过大上。

由于这些内部文件夹都在C:\Users\xxx\AppData\Local\Microsoft\Outlook\outlook.ost中，并不能直接删除。因此我的办法是直接删除该文件。

不用担心邮件丢失，本地删除之后，Outlook会自动从服务器重新下载的。

# WxWidget

WxWidget在windows平台的安装包是个奇葩的东西，它并不是可执行文件的安装包，而是个源代码安装包。因此安装之后，还需要编译，才能使用。

以MinGW编译为例，说一下编译的步骤：

1.设置MinGW环境。这里需要强调的是MinGW和WxWidget的安装路径都不能有空格。

2.进入build/msw文件夹，执行以下命令：

`mingw32-make -f makefile.gcc BUILD=release SHARED=0 MONOLITHIC=1 UNICODE=1 CXXFLAGS=-fno-keep-inline-dllexport`

参考：

https://mp.weixin.qq.com/s/pJIuKgZC1o757iwkrt3uUQ

wxPython：Python首选的GUI库

# WebGL

WebGL可以看作是JavaScript + OpenGL ES，它为Web开发者使用显卡创建3D应用提供了方案。

类似的还有WebGPU = JS + Vulkan。

## 教程

https://developer.mozilla.org/en-US/docs/Web/API/WebGL_API/Tutorial

这是mozilla的官方教程。

https://webglfundamentals.org/webgl/lessons/zh_cn/

这是一个中文的教程，但比上个教程要深一些。

## 示例

https://github.com/mdn/webgl-examples

mozilla官方教程的示例

## gl-matrix

gl-matrix是一个矩阵运算库。除非只是绘制一个空画布，否则即使是绘制一个矩形的任务，也少不了数学运算。

官网：

http://glmatrix.net

gl-matrix 3.0以后添加了名字空间glMatrix，所以老旧的代码可能需要这样修改：

`var mat4 = glMatrix.mat4;`

## 其他库

https://deck.gl/

## 参考

https://zhuanlan.zhihu.com/p/68507311

WebGL进阶——走进图形噪声

# Machine Learning之Python篇+

https://mp.weixin.qq.com/s/6x5Wmpy4Im0x3Ddua9rvFw

用python怎样识别验证码？传统方法，非DL

https://mp.weixin.qq.com/s/Xksp457JBr2ySrtqgXYMFw

一文学会入门推荐算法库surprise

https://mp.weixin.qq.com/s/x9esWTfZOtfedjkrcKM1Qw

利用Python的混合集成机器学习

https://github.com/TheAlgorithms/Python

各种Python算法的新手入门大全

https://mp.weixin.qq.com/s/PtZ3YvFrUgbK5y3pcAeW9A

使用OpenCV和Python实现的机器学习

https://mp.weixin.qq.com/s/iT91pDPP49EuNA0KfY2njg

十大数据科学Python热门库推荐

https://mp.weixin.qq.com/s/e--IeRTRZMqhs_DSJKpgyQ

特征工程之Scikit-learn

https://mp.weixin.qq.com/s/z3N5v4H_W2sxh2XPpFbAcA

除了Python，这些语言写的机器学习项目也很牛

https://mp.weixin.qq.com/s/U2cDVRxZPVsqoL-zHn__CA

Python做机器学习之路

https://mp.weixin.qq.com/s/LgVS5N80UlCeEfrPtyUF4Q

深度学习矩阵运算的概念和代码实现

https://mp.weixin.qq.com/s/0XteuIk71qSpxrZPGVnMbg

Python3实现K-近邻算法

https://mp.weixin.qq.com/s/bGudwxu8c0LIxv1h64qZtQ

Python3实现决策树算法
