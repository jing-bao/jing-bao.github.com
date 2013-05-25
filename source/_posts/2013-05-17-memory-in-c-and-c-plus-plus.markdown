---
layout: post
title: "『转载整理』C/C++中内存机制"
date: 2013-05-17 00:15
comments: true
categories: [interview, c++]
---
##概述
在多任务操作系统中的每一个进程都运行在一个属于它自己的内存沙盘中。这个沙盘就是虚拟地址空间（virtual address space），在32位模式下它总是一个`4GB`的内存地址块。这些虚拟地址通过页表（page table）映射到物理内存，页表由操作系统维护并被处理器引用。每一个进程拥有一套属于它自己的页表，只要虚拟地址被使能，那么它就会作用于这台机器上运行的所有软件，包括内核本身，因此一部分虚拟地址必须保留给内核使用
<!--more-->

{% img /images/memory-in-c-and-c-plus-plus-block.png '程序内存块' '图片加载失败 :('  %}

在Linux中，内核空间是持续存在的，并且在所有进程中都映射到同样的物理内存。内核代码和数据总是可寻址的，随时准备处理中断和系统调用。与此相反，用户模式地址空间的映射随进程切换的发生而不断变化

{% img /images/memory-in-c-and-c-plus-plus-switch.png '程序切换' '图片加载失败 :('  %}

下面是一个Linux进程的标准的内存段布局

{% img /images/memory-in-c-and-c-plus-plus-layout.png '内存布局' '图片加载失败 :('  %}

- 进程地址空间中最顶部的段是栈，大多数编程语言将之用于存储局部变量和函数参数。进程中的每一个线程都有属于自己的栈。
- 在栈的下方是内存映射段。此处，内核将文件的内容直接映射到内存。任何应用程序都可以通过Linux的mmap()或Windows的CreateFileMapping() / MapViewOfFile()请求这种映射。内存映射是一种方便高效的文件I/O方式，所以它被用于加载动态库。
- 堆用于运行时内存分配。但不同点是，堆用于存储那些生存期与函数调用无关的数据。在C语言中，堆分配的接口是malloc()系列函数，而在具有垃圾收集功能的语言（如C#）中，此接口是new关键字。
- BSS（未初始化数据区）保存的是未被初始化的静态变量内容
- 数据段保存在源代码中已经初始化了的静态变量内容
- 代码段是只读的，保存了你全部的代码外加零零碎碎的东西，比如字符串字面值。
  
下图展示了这些段以及我们例子中的变量
{% img /images/memory-in-c-and-c-plus-plus-example.png '例子展示' '图片加载失败 :('  %}


##C语言中的内存机制

- 栈(Stack)  
位于函数内的局部变量（包括函数实参），由编译器负责分配释放，函数结束，栈变量失效。

- 堆(Heap）  
由程序员用malloc/calloc/realloc分配，free释放。如果程序员忘记free了，则会造成内存泄露，程序结束时该片内存会由OS回收。

- 全局区/静态区(Global Static Area)  
全局变量和静态变量存放区，程序一经编译好，该区域便存在。程序结束时释放。  
在C语言中，初始化的全局变量和静态变量和未初始化的放在相邻的两个区域。

- C风格字符串常量存储区  
专门存放字符串常量的地方，程序结束时释放。（字符串常量一般放在 .text段 或单独的 .rodata段）【？？？似乎跟C++的常量存储区类似。const常量是否也放在这里？】

- 程序代码区

##C++中的内存机制
与C相同部分不再描述

- 栈(Stack)

- 堆（Heap)  
由new申请的内存，由delete或delete[]负责释放。

- 自由存储区(Free Storage)  
malloc/calloc/realloc分配的内存。对象的生存周期可以比内存被分配的时间短，即可以用void*访问这块内存，而没有具体对象。自由存储区和堆的本质区别有争议，具体可见[这里](http://hi.baidu.com/yangdgjy/item/23a3c28daa7f495c840fabb2)

- 全局区/静态区(Global Static Area)  
在C++中，由于全局变量和静态变量编译器会给这些变量自动初始化赋值，所以没有区分了初始化变量和未初始化变量了

- 常量存储区  
存储字符串常量和其他在编译时已知的数据。整个程序运行期间可见。只读。

- 程序代码区  

***
参考链接:

[剖析程序的内存布局](http://blog.csdn.net/drshenlei/article/details/4339110)  
[c/c++内存机制（一）（原）](http://www.cnblogs.com/ComputerG/archive/2012/02/01/2334898.html)  
[Memory Management - Part I](http://www.gotw.ca/gotw/009.htm)
