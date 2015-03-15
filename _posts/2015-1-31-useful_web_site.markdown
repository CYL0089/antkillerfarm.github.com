---
layout: post
title:  有用的网址集合 & IT杂谈
category: technology 
---
# 有用的网址集合

1.http://packages.ubuntu.com/

使用apt-get获取软件虽然方便，但是从ubuntu的源获得的软件包和直接使用源码编译安装的包相比，包中的各个文件被分散在好多个文件夹中，查找起来很不方便。这时可以到这个网址，去查找软件包里的文件清单，以弄清楚XX软件官网上所说的YY文件在ubuntu中到底放在哪里。

2.http://softwaredev.blog.sohu.com/105412003.html

C++库大全

3.http://linux.die.net/man/

Linux手册（相当于Linux的MSDN）

4.http://www.alldatasheet.com/

可以查找各类芯片的手册。

5.http://www.hellogcc.org/

一个有关GCC和GDB的博客

6.makefile

简单的规则可以查看《GNU makefile指南》一文，Goerge Foot写的。地址就不贴了，中文版基本到处都是。但是该文只是入门级的，最专业的还是GNU官方的手册：

http://www.gnu.org/software/make/manual/html_node/index.html

应该说make的语法，除了规则依赖之外，大多数与bash相同。但是make也有一些内置函数，它们的用法就不在bash的范畴了，比如call函数。

# IT杂谈

这篇文章太短，所以加些料谈一些技术之外的东西，就不新起一篇了。不排除将来写的多了，将至单独为一篇的可能，现在就这样吧。

## Bill Gates和MS-DOS

众所周知，MS-DOS的最初版本，是西雅图电脑公司的Tim Paterson写的。但这是否和Bill Gates一点关系都没有呢？

其实不然。Tim Paterson写86-DOS只花了6个星期，就算他是个大牛，也断没有这么快的道理。这只能说明他在做这个东西的时候，手里已经有了相当多的素材，使得他只需要做少量的工作即可。

素材是什么呢？Wiki上给出了答案：CP/M和FAT。

前者是Digital Research公司的产品，也就是那个由于价钱高，而被IBM在选择PC OS时放弃的公司。CP/M 8-bit是70年代中期最流行的OS。但到了1978年以后，随着技术的扩散和相关书籍的出现，这在当时的技术圈里已经是大路货了。

后者和Bill Gates可就大有关系了。Wiki上告诉我们，Bill Gates是FAT的两个发明人之一。

从以上的信息可以分析出，Tim Paterson虽然不在MS，但是和Bill Gates的关系可不是一般的密切。事实上Tim Paterson在编写86-DOS之前的工作，就是帮MS的软件设计硬件卡。（当时的PC，处理能力有限，很多软件都被设计成了硬件卡的形式。老的PC用户应该对286时代的汉字卡和386时代的音视频解压卡还有些印象吧。史玉柱就是靠卖汉卡起家的。）所以这也成为Bill Gates能够知道86-DOS存在，也敢于花大价钱买的重要原因。此外，两人的岁数相当，Bill Gates只比Tim Paterson大两岁，基本上就是同一个战壕的兄弟。我想这也是Tim Paterson三进三出MS的重要原因。

顺便提一句，2.5W美元的价格其实是个公道价，微软只买走了使用权，没买走版权，这也为之后西雅图电脑公司和MS之间的官司埋下了伏笔。相比之下，两年前Steve Jobs只花了1.3W美元就搞定了OS。

归根到底，西雅图电脑公司只是个硬件公司，它并不重视软件的价值。所以虽然它的软件技术比MS这样的软件公司还强，但是却不懂得如何利用软件。因此没有把握好时代的脉搏，让MS占了便宜，也就不足为奇了。

## CP/M和BeOS

这两个东西的共同点，相信熟悉那段历史的人都能看的出来。没错，这就是：

1.两者都是技术上的先行者和领先者，而且直到最后失败，也不是由于技术被反超导致的。

2.两者都有很好的机会，能够改写历史。

3.两者都是由于要价过高，而被机会所放弃。

4.他们的对手尽管有种种不足，但是最终改写了历史。他们是MS-DOS和Mac OS X。
