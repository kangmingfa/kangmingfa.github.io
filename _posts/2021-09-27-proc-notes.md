---
layout: post
title: "操作系统进程 notes"
author: "kangmingfa"
categories: sample
tags: [os]
image: city-2.jpg
---
*进程是一个应用程序运行时刻的实例*

#### 进程结构示意图

![](https://static001.geekbang.org/resource/image/72/b3/725580e605b20be40ac3e0b24d82d0b3.jpg?wh=3455x1955)

![](https://static001.geekbang.org/resource/image/65/68/6577df8ebc8323fa9f34835371a4b268.jpg?wh=3360x2805)

#### 内核提供服务的机制

内核是这样提供服务的：通过停止应用程序代码运行，进入内核地址空间运行内核代码，然后返回结果。

#### cpu组织进程示意图

![proc](/assets/img/2021-09-28 上午11.33.47.png)

```cpp

typedef struct s_THRDLST
{
    list_h_t    tdl_lsth;                //挂载进程的链表头
    thread_t*   tdl_curruntd;            //该链表上正在运行的进程
    uint_t      tdl_nr;                  //该链表上进程个数
}thrdlst_t;
typedef struct s_SCHDATA
{
    spinlock_t  sda_lock;                //自旋锁
    uint_t      sda_cpuid;               //当前CPU id
    uint_t      sda_schdflgs;            //标志
    uint_t      sda_premptidx;           //进程抢占计数
    uint_t      sda_threadnr;            //进程数
    uint_t      sda_prityidx;            //当前优先级
    thread_t*   sda_cpuidle;             //当前CPU的空转进程
    thread_t*   sda_currtd;              //当前正在运行的进程
    thrdlst_t   sda_thdlst[PRITY_MAX];   //进程链表数组
}schdata_t;
typedef struct s_SCHEDCALSS
{
    spinlock_t  scls_lock;                //自旋锁
    uint_t      scls_cpunr;               //CPU个数
    uint_t      scls_threadnr;            //系统中所有的进程数
    uint_t      scls_threadid_inc;        //分配进程id所用
    schdata_t   scls_schda[CPUCORE_MAX];  //每个CPU调度数据结构
}schedclass_t;
```
