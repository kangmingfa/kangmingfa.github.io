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

在二级引导器中，我们要检查 CPU 是否支持 64 位的工作模式、收集内存布局信息，看看是不是合乎我们操作系统的最低运行要求，还要设置操作系统需要的 MMU 页表、设置显卡模式、释放中文字体文件

| 1  | 2   | 3  |
| -- | --- | -- |
| gg | ggg | gg |
| gg | gg  | gg |
