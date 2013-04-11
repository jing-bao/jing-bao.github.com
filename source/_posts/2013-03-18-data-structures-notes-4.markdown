---
layout: post
title: "数据结构与算法分析 读书笔记 4"
date: 2013-03-18 23:53
comments: true
categories: [c++, notes]
---
第三章剩下的部分
<!--more-->
##第三章 表、栈和队列
###3.4 向量的实现
vector是基本类类型，这意味着不同于基本数组，vector可以复制并且其占用的内存可以自动回收（通过析构函数）

【？？？】程序第37行，为什么`reserve()`函数中判断的是`newCapacity < theSize`而不是`newCapacity < theCapacity`？

###3.5 表的实现
在表的前端和末尾添加额外结点作为标志，称为哨兵结点。头部结点称为表头结点，末端的结点称为尾结点。使用好处是可以去掉很多特例，简化程序代码。

List中的代码摘抄  
``` c
class List
{
private:
    struct Node{...};
public:
    class const_iterator{...};
    class iterator: public const_iterator{...};
    ...
private:
    int theSize;
    Node *head;
    Node *tail;
    ...
};
```

定义前缀和后缀`operator++`的区别方法：给前缀指定空参数表，给后缀指定一个匿名的int参数。实现指出在许多可以选择使用前缀或后缀的情况下，使用前缀形式要快于使用后缀形式  
``` c
const_iterator operator++()
{
    current = current->next;
    return *this;
}

const_iterator operator++(int)
{
    const_iterator old = *this;
    ++(*this);
    return old;
}
```

###3.6 栈ADT
栈的操作基本只有`push(),pop(),top()`，只有栈顶元素是可访问的。

栈是一个表，用数组和单向链表都可以实现，所以list和vector都支持栈，且操作为常量时间操作。

####栈的应用
后缀或逆波兰记法：将`4.99*1.06 + 5.99 +6.99*1.06`（称为中缀式）记为`4.99 1.06 * 5.99 + 6.99 1.06 * +`。   当一个表达式以后缀记法给出时，没有必要知道任何优先规则。

可以用栈实现中缀式到后缀式的转换，主要原则是入栈操作符，读取新操作符时，从栈顶弹出直到遇到优先级更低的元素为止【需复习】

尾递归：在最后一行的递归调用，是极差的使用递归的例子。  
通过将代码放到一个while循环中并用每个函数的一个参数代替递归调用，可以机械地消除尾递归。【？？？】不明白尾递归和其他递归的本质区别  
递归总能够被彻底除去（编译器的工作），但提速的同时降低了程序的清晰性。

###3.7 队列ADT
队列也是表。基本操作`enqueue(),dequeue()`

循环数组：处理front或back到达数组尾端，而数组前面有很多空位的情况。
    