---
layout: post
title: "操作系统笔迹"
author: "kangmingfa"
categories: sample
tags: [sample]
image: blog_001.jpeg
---
### 二级引导器

C语言 ：保护模式        Bios中断 : 16位实模式,为了获取计算机硬件信息

The bootloader transfers the control to the operating system kernel.The operating system or kernel is loaded into the memory by the secondary boot loader. After that, the control of the machine is transferred to the operating system.

在二级引导器中，我们要检查 CPU 是否支持 64 位的工作模式、收集内存布局信息，看看是不是合乎我们操作系统的最低运行要求，还要设置操作系统需要的 MMU 页表、设置显卡模式、释放中文字体文件。

其实 VBE 是显卡的一个图形规范标准，它定义了显卡的几种图形模式，每个模式包括屏幕分辨率，像素格式与大小，显存大小。调用 BIOS 10h 中断可以返回这些数据结构。

我们在调用 Cosmos 第一个 C 函数之前，我们依然要写一小段汇编代码，切换 CPU 到长模式，初始化 CPU 寄存器和 C 语言要用的栈。因为目前代码执行流在二级引导器中，进入到 Cosmos 中这样在二级引导器中初始过的东西都不能用了。
