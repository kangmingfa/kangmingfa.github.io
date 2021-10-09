---
layout: post
title: "Linux 内存管理 notes"
author: "kangmingfa"
categories: sample
tags: [os]
image: nature-1.jpg
---
![mem](/assets/img/截屏2021-09-29 上午10.57.35.png)

#### 分配内存，伙伴系统，涉及到内存分割

分配内存【只能按2^n页面申请】
alloc_pages->alloc_pages_current->__alloc_pages_nodemask
->get_page_from_freelist，快速分配路径，尝试直接分配内存
->__alloc_pages_slowpath，慢速分配路径，尝试回收、压缩后，再分配内存，如果有OOM风险则杀死进程->实际分配时仍会调用get_page_from_freelist
->->所以无论快慢路径，都会到rmqueue
->->->如果申请一个页面rmqueue_pcplist->__rmqueue_pcplist
1、如果pcplist不为空，则返回一个页面
2、如果pcplist为空，则申请一块内存后，再返回一个页面
->->->如果申请多个页面__rmqueue_smallest
1、首先要取得 current_order【指定页面长度】 对应的 free_area 区中 page
2、若没有，就继续增加 current_order【去找一个更大的页面】，直到最大的 MAX_ORDER
3、要是得到一组连续 page 的首地址，就对其脱链，然后调用expand函数对内存进行分割
->->->->expand 函数分割内存
1、expand分割内存时，也是从大到小的顺序去分割的
2、每一次都对半分割，挂载到对应的free_area，也就加入了伙伴系统
3、直到得到所需大小的页面，就是我们申请到的页面了

#### SLAB 分配器的原理和实现

![mem](/assets/img/截屏2021-09-30 下午3.40.07.png)
