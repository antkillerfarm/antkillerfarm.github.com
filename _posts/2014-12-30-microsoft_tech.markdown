---
layout: post
title:  这些年微软相关的技术总结
category: technology 
---

# C#相关

## 1.XmlReader

XmlReader是.NET中处理XML的类。之前的W3C已经提出了DOM和SAX两种模型。作为最早的模型，这两种模型在各种语言中都有相应的实现，关于它们的讨论也有很多，在此不再赘述。

XmlReader是一种快速、无缓冲、向前并只读的轻量级XML引擎，在这一点上它和SAX是相同的。它和SAX的主要区别在于它们的设计模式是不同的。SAX是push模式的XML引擎，它会在读到相应的节点时，触发消息并将之发送给调用程序。而XmlReader则是pull式的XML引擎，它更符合人们的通常的对文件顺序处理的习惯。当然除此之外，它还有其他的好处。参看MSDN的相关章节。

通常情况下，pull模型比push模型的效率高，但push模型的灵活性要高些。例如，Dos下的编程主要是一种顺序执行的pull模型结构，而Windows下的编程则是一种基于消息的push模型结构。所以，读取特定标签内容的XML文档，采用XmlReader比较好，而对于那些结构不固定或需要遍历的XML文档用SAX较好。

PS: XmlReader提出的pull方案，后来被libxml2采用，成为了GTK+的一部分。

## 2.Microsoft.WindowsCE.Forms

这是一个Wince的类。由于.NET CF中的System.Windows.Forms.Form没有DefWndProc参数，所以要想接收窗体消息就要用到它。不过它的dll需要自己添（在.NET CF的安装路径下可以找到这个Microsoft.WindowsCE.Forms.dll），接收C++编程时的那些WM_开头的消息时需要用到这个模块中的MessageWindow类。

代码示例如下:

{% highlight c# %}
    public class MsgWindow : MessageWindow
    {

        public const int WM_CUSTOMMSG = 0x0400+3333;
        
        private Form1 msgform;//Form1是用于接收消息的那个窗体Form。

        public MsgWindow(Form1 msgform)
        {
            this.msgform = msgform;
        }

        protected override void WndProc(ref Message msg)
        {
            switch (msg.Msg)
            {
                case WM_CUSTOMMSG:
                    //do sth
                    break;
            }
            base.WndProc(ref msg);
        }
    }
{% endhighlight %}

## 3.C#中的发布功能

C#项目属性中有个publish标签是关于发布程序的功能的。用过Java的人都知道，Java的可移植性是很不错的，但Java程序的部署相对于一般 的本地程序来说就显得比较麻烦了。至少在本人的程序生涯中，就多次经历拿着JAR文件兴冲冲的跑去，却发现目标机没有JRE的情况。

C#中的发布功能除了能打包程序和.NET Framework之外，还有网络发布的功能。这里我主要讨论单机程序的打包。如果只有程序文件的话，情况比较简单，没什么难度。如果还有数据文件的话就 要注意了。在程序文件（Application Files）对话框中，每个文件有一个发布状态（Publish Status）的属性，如果你希望数据文件和程序文件在同一个文件夹下的话，最好选择包含（Include）而不是数据文件（Data File），后者会把你的数据文件部署到一个专门的数据文件夹下，这个文件夹可用 System.Deployment.Application.DataDirectory获得。

# VS2005

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

## 7.工程向导

某些SDK可能比较古老，没有匹配VS2005的向导文件，可以采用以下办法试试：找到向导对应的.vsz文件，将Wizard所在行改为Wizard=VsWizard.VsWizardEngine.8.0，但不同版本的向导之间，还是有差别的，因此这一招在某些情况下，可能无效。

## 8.

win32控制台工程，如果在运行时，不希望有控制台窗口，只要在程序中加上：

 `#pragma comment( linker, "/subsystem:\"windows\" /entry:\"mainCRTStartup\"" )`

反过来如果某个win32控制台工程运行过快，以至于看不清控制台窗口的输出时，可以在主程序的末尾加上：

`system("pause");`

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

{% highlight c %}
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
{% endhighlight %}

## 8.打电话

使用tapiRequestMakeCall函数。
