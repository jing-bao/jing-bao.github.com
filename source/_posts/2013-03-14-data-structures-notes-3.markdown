---
layout: post
title: "数据结构与算法分析 读书笔记 3"
date: 2013-03-14 23:58
comments: true
categories: [c++, notes]
---
3.1到3.3节。重点记录的是vector和list的操作函数，因为以前用的比较少
<!--more-->
##第三章 表、栈和队列
###3.1 抽象数据类型ADT

抽象数据类型（ADT）是带有一组操作的一些对象的集合。在ADT的定义中根本没有提到这组操作是如何实现的

###3.2 表ADT

- 简单数组实现：适合在末尾插入元素，之后只有数组访问  
  数组是静态分配的，但vector类允许在需要的时候将数组的大小增加一倍
- 链表实现：适合插入和删除在整个表中都发生  
  典型的链表保持至表的两端的链接，为了第一项的添加删除和末尾的新项添加为常量时间【应该是由于在两端操作数据最为常见】  
  双向链表，可以方便删除最后一项(方便找到倒数第二项)
  
###3.3 STL中的向量和表

标准模板库STL实现了公共数据结构，例如表ADT。这些数据结构称为集合（collection）或容器（container）

- vector给出了表ADT的可增长的数组实现。  
  优点：常量时间内可索引【findKth】，缺点：插入或删除代价昂贵（除非发生在末尾），查找效率低【find】
- list提供表ADT的双向链表实现。  
  优点：插入和删除代价小，缺点：不容易索引，查找效率低

常用方法：  
``` c
int size() const;
void clear();//删除容器内的所有元素
bool empty();
//以下四个都为常量时间
void push_back(const Object & x);//在表的末尾添加x
void pop_back();//删除表的末尾对象
const Object & back() const;
const Object & front() const;

//仅针对list
void push_front(const Object & x);
void pop_front();

//仅针对vector
Object & operator[](int idx);//不包含边界检测
Object & at(int idx);//包含边界检测
int capacity() const;//返回vector的内部容量
void reserve(int new Capacity);//设定vector的新容量
```

####迭代器iterator
通过内置类型iterator来标记位置，例如`list<string>::iterator`和`vector<int>::iterator`

相关方法：  
``` c
iterator begin();//返回指向容器第一项的适当迭代器
iterator end();//指向容器的终止标志，容器中最后一项的后面的位置，边界之外
itr++; ++itr;
*itr;//返回itr指定位置的对象的引用
itr1 == itr2; itr1 != itr2;//是否指向同一个位置
iterator insert(iterator pos, const Object & x);//返回插入项位置。对list常量时间，对vector不是
iterator erase(iterator pos);//返回pos下一个元素的位置，操作完成后pos失效
iterator erase(iterator start, iterator end);//删除的元素直到但不包括end
```
【注意错误】我写的对表使用erase的代码
``` c WRONG!!!
//1.erase后itr失效，不能继续使用了
//2.使用itr+2操作会超过lst.end()，越界
//...是否还有其他错误？

template <typename Container>
void removeEveryOtherItem(Container & lst)
{
    for(Container::iterator itr = lst.begin(); itr != lst.end(); itr=itr+2)
        lst.erase(itr);
}
```

STL也包含const_iterator，*const_iterator会返回常量引用，不能成为左值

``` c
iterator begin()
const_iterator begin() const
```


两个版本的begin可以在同一个类里，因为方法的定常性被认为是标号的一部分
如果对非常量容器调用begin，返回iterator的版本被调用。如果对常量容器调用，返回const_iterator
