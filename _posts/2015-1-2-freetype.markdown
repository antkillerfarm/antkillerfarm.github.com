---
layout: post
title:  FreeType使用指南
category: technology 
---

FreeType是一套跨平台的字体文件编程开发包。它的官网是www.freetype.org，你可以到这个网站获得最新的源代码。

网上的关于FreeType的文献很多。写得较好的有以下两篇：

http://www.cppblog.com/wlwlxj/archive/2006/11/08/14843.aspx   （文献A）

这篇文章比较概括，且有demo，适合入门之用。

http://hi.baidu.com/guopeng717/blog/item/f371088287532a95f603a6db.html   （文献B）

这篇文章深入一些，但代码片段不可运行，需要对FreeType有根底的才看的明白。

本着原创的精神，上面文献提到过的，我不再复述，仅结合自己近来的研究，对一些关键点做一下说明。

## 1.编译FreeType库

由于我只用VC编过，所以这里只谈VC编库心得。这是我见到过的最友好的跨平台库，VS的工程文件都很齐全，不像其他的软件库往往只有make文件。而且FreeType库中，不但有PC的工程，还有Wince的工程。这里唯一要注意的是，在VS2005或更高版本中，有时需要调整工程的参数，以满足VS不同版本之间的移植需要。

## 2.字符对齐

文献A中的代码，显示单个字是没有问题的，但显示一排字尤其是数字符号时，就有问题了。虽然我们已经使用了FT_Set_Char_Size或FT_Set_Pixel_Sizes设定了字模的大小，但返回的字模并不都是一样大的。空白字符返回的字模，大小为0，逗号、句号返回的字模只有普通字的几分之一。这时就需要用glyph->bitmap_left和glyph->bitmap_top来指定起始位置。（详见文献B）

## 3.文献A勘误

文献A的代码中

`printf( " %d " , bitmap.buffer[i * bitmap.width + j] ? 1 : 0 );`

是不准确的，应该改为

`printf( " %d " , bitmap.buffer[i * bitmap.pitch + j] ? 1 : 0 );`

pitch指字模一行所占的字节数，在ft_render_mode_normal模式（即256灰度模式）下，pitch的值和width的值相等。但在ft_render_mode_mono模式（即黑白模式）下，这两个值一般就不等了。

黑白模式下，这句应为

`printf( " %d " , (bitmap.buffer[i * bitmap.pitch + (j>>3)]<<(j & 0x7 )) & 0x80 );`

## 4.小尺寸字体

并不是所有的矢量字库都包含小字体的，例如微软的宋体就不支持小于20*20的字模，所以，使用小尺寸字体时，必须仔细选择字库。

