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

### 第一个 C 函数 hal_start

##### 初始化HAL层

```

void init_hal()
{
    //初始化平台
    //初始化内存
    //初始化中断
    return;
}
```

##### 初始化中断

x86 cpu 和设备之间的中断控制器是8259A

![](https://static001.geekbang.org/resource/image/4d/09/4d81f7feb668abf30c5cced619549709.jpg)

上面直接和 CPU 连接的是主 8259A，下面的是从 8259A，每一个 8259A 芯片都有两个 I/O 端口，我们可以通过它们对 8259A 进行编程。主 8259A 的端口地址是 0x20，0x21；从 8259A 的端口地址是 0xA0，0xA1。

##### vmlinuz

我们在**/boot**目录下会发现 vmlinuz 文件，这个文件是怎么来的呢？vmlinux 文件就是编译整个 Linux 内核源代码文件生成的,Linux 的代码分布在各个代码目录下，这些目录之下又存在目录，Linux 的 kbuild（内核编译）系统，会递归进入到每个目录，由该目录下的 Makefile 决定要编译哪些文件。在编译完具体文件之后，就会在该目录下，把已经编译了的文件链接成一个该目录下的 built-in.o 文件，这个 built-in.o 文件也会与上层目录的 built-in.o 文件链接在一起。再然后，层层目录返回到顶层目录，所有的 built-in.o 文件会链接生成一个 vmlinux 文件，这个 vmlinux 文件会通过前面的方法转换成 vmlinux.bin 文件。但是请注意，vmlinux.bin 文件它依然是 ELF 格式的文件。

##### 操作系统的启动流程

操作系统的启动分为两个阶段：引导boot和启动startup，本节主要还是boot过程：
BIOS->GRUG1->GRUB1.5->GRUB2->Linux内核【环境硬盘引导、MBR分区】

1、按电源键，系统加电

2、主板通电
CPU加电时，会默认设置[CS:IP]为[0XF000:0XFFF0]，根据实模式下寻址规则，CPU指向0XFFFF0
这个地址正是BIOS启动程序位置，而BIOS访问方式与内存一致，所以CPU可以直接读取命令并执行

3、BIOS执行
3.1、BIOS首先执行POST自检，包括主板、内存、外设等，遇到问题则报警并停止引导

3.2、BIOS对设备执行简单的初始化工作

3.3、BIOS 会在内存中：
建立中断表（0x000000x003FF）
构建 BIOS 数据区（0x004000x004FF）
加载了中断服务程序（0x0e05b~0x1005A）

3.4、BIOS根据设备启动顺序，依次判断是否可以启动
比如先检查光驱能否启动
然后依次检查硬盘是否可以启动【硬盘分区的时候，设置为活动分区】

4、硬盘引导
4.1、先说下寻址方式，与扇区编号的事情
最传统的磁盘寻址方式为CHS，由三个参数决定读取哪个扇区：磁头（Heads）、柱面(Cylinder)、扇区(Sector)
磁头数【8位】，从0开始，最大255【微软DOS系统，只能用255个】，决定了读取哪个盘片的哪个面【一盘两面】
柱面数【10位】，从0开始，最大1023【决定了读取哪个磁道，磁道无论长短都会划分为相同扇区数】
扇区数【6位】，从1开始，最大数 63【CHS中扇区从1开始，而逻辑划分中扇区从0开始，经常会造成很多误解】
每个扇区为512字节

4.2、然后说下引导方式
BIOS在发现硬盘启动标志后，BIOS会引发INT 19H中断
这个操作，会将MBR【逻辑0扇区】，也就是磁盘CHS【磁头0，柱面0，扇区1】，读取到内存[0:7C00h]，然后执行其代码【GRUB1阶段】，至此BIOS把主动权交给了GRUB1阶段代码
MBR扇区为512字节，扇区最后分区表至少需要66字节【64字节DPT+2字节引导标志】，所以这段代码最多只能有446字节，grub中对应的就是引导镜像boot.img
boot.img的任务就是，定位，并通过BIOS INT13中断读取1.5阶段代码，并运行

5、Grub1.5阶段
5.1、先说一下MBR GAP
据说微软DOS系统原因，第一个分区的起始逻辑扇区是63扇区，在MBR【0扇区】和分布表之间【63扇区】，存在62个空白扇区，共 31KB。
Grub1.5阶段代码就安装在这里。

5.2、上面提到，boot.img主要功能就是找到并加载Grub1.5阶段代码，并切换执行。
Grub1.5阶段代码是core.img，其主要功能就是加载文件系统驱动，挂载文件系统， 位加载并运行GRUB2阶段代码。
core.img包括多个映像和模块：
diskboot.img【1.5阶段引导程序】，存在于MBR GAP第一个扇区；【这里是硬盘启动的情况，如果是cd启动就会是cdboot.img】
lzma_decompress.img【解压程序】
kernel.img【grub核心代码】，会【压缩存放】
biosdisk.mod【磁盘驱动】、Part_msdos.mod【MBR分区支持】、Ext2.mod【EXT文件系统】等，会【压缩存放】

其实boot.img只加载了core.img的第一个扇区【存放diskboot.img】，然后控制权就交出去了，grub阶段1代码使命结束。
diskboot.img知道后续每个文件的位置，会继续通过BIOS中断读取扇区，加载余下的部分并转交控制权，包括：
加载lzma_decompress.img，从而可以解压被压缩的模块
加载kernel.img，并转交控制权给kernel.img
kernel.img的grub_main函数会调用grub_load_modules函数加载各个mod模块
加载各个mod后，grub就支持文件系统了，访问磁盘不需要再依靠BIOS的中断以扇区为单位读取了，终于可以使用文件系统了

##### 第一个用户进程

Linux 内核的第一个用户态进程是在 kernel_init 线程建立的，而 kernel_init 线程执行的就是 kernel_init 函数

![启动流程](/assets/img/linux_boot_process.jpg)

---
