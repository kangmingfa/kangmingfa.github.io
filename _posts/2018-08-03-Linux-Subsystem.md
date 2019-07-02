---
layout: post
title: "Windows linux subsystem"
author: "fmitk"
categories: sample
tags: [sample]
image: city-2.jpg
---
<!-- ![img](/assets/img/cuba-1.jpg) -->
**本人第一次撰写博客，主要整理自己所学所好奇的知识点，次要为传播。*

此文适合即将了解linux基础及windows下linux子系统的人

正如我们所知道的，linux系统能够更好的支持开源(open source)工具，例如gcc,g++,git，ssh等，包括语言C/C++,C#,python,ruby等。可能正因为linux对于开发人员的便捷，微软在2016年推出WSL(windows subsystem for linux),当然现在一直在开发维护当中，这相当于在windows的基础上，创建了一个适合各种linux发行版的子系统，linux发行版在Microsoft store上包括流行的ubuntu,debian,opensuse,kali等。有了linux子系统，我们就可以通过cmd(也叫console)输入一些简单的指令进行windows和linux系统间的切换，个人觉得利用好两个系统的优势可以很好的提高效率，而且windows上的linux子系统在所占的内存和cpu使用率比VM workstation或者VM Box安装的虚拟机小很多，所以对于电脑配置不是很高的特别适合。

下面这张图告诉我们有了linux子系统能做什么

![img](/assets/post_imgs/img01.PNG)

**这里有一些安装的主要步骤：**

首先我们打开win10的powershell,然后**右击**，选择以管理员的方式运行
![img02](/assets/post_imgs/img02.png)


进入powershell界面后输入以下命令：

```shell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

执行完后，会提示是否要重启，输入yes即可(重启需要一定的时间)。

重启之后，进入微软商店找到[ubuntu](https://www.microsoft.com/zh-cn/p/ubuntu/9nblggh4msv6?rtc=1&activetab=pivot%3aoverviewtab)，[Debian](https://www.microsoft.com/zh-cn/p/debian-gnu-linux/9msvkqc78pk6?activetab=pivot%3aoverviewtab),[kali](https://www.microsoft.com/zh-cn/p/kali-linux/9pkr34tncv07?activetab=pivot%3aoverviewtab)等，选择想要下载的系统和版本就行。

下载完之后，需要进行系统初始化，在开始菜单点击我们所下载子系统的图标即可。如下图：
![img03](/assets/post_imgs/img03.png)

点击图标后会进入shell(也叫做壳)，一般linux系统默认为bash shell,在初始化过程中，会提示创建用户账户，和该用户对应的密码，用户名可以为以后进入shell的默认用户名。

自此初始化结束，会进入linux指令输入终端，即可按照正常linux指令操作,从cmd输入bash即可进入linux操作界面(再次输入cmd.exe即可重新切换到windows命令行界面)，或者从开始菜单图标直接进入也可以
如下图：
![img04](/assets/post_imgs/img04.png)

**注：**
以上支持Win10系统一系列操作。
操作系统版本要在1493.0以上

贴图：
![system](/assets/post_imgs/system.png)
