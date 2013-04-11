---
layout: post
title: "数据结构与算法分析 读书笔记 1"
date: 2013-02-09 13:16
comments: true
categories: [c++, notes]
---
看扫描pdf真心累啊，可是家里没有纸质书。。。

书籍信息：数据结构与算法分析 C++描述（第3版）Mark Allen Weiss 著；张怀勇等译；人民邮电出版社

<!--more-->
##第一章 引论
###1.1 本书讨论的内容
问题1：选择问题。设有一组N个数，要确定其中第k个最大的数

问题2：输入由一个二维字母数组和一个单词表组成，找出水平、垂直或对角方向放置的单词

###1.2 数学知识复习
- 几何级数：{% img /images/data-structures-notes-1-jihe.PNG '几何级数' '图片加载失败 :('  %}
- 算术级数：{% img /images/data-structures-notes-1-suanshu.PNG '算术级数' '图片加载失败 :('  %}
- {% img /images/data-structures-notes-1-gaojie.PNG '高阶' '图片加载失败 :('  %}
- 调和数：{% img /images/data-structures-notes-1-tiaohe.PNG '调和数' '图片加载失败 :('  %}

归纳法：第一步证明基准情形（对于某个或某些值），接着归纳假设（k），然后证明k+1。必须k有限。

###1.3 递归的简单介绍
递归：需要处理基准情况base case和递归调用。

递归的基本准则

- 基准情形
- 不断推进：递归调用将一直进行到基准情形出现为止，若永远不能到达基准情形，则出错。
- 设计法则：假设所有的递归调用都能运行。不追踪实际的递归调用序列。
- 合成效益法则：在求解一个问题的同一实例时，切勿在不同的递归调用中做重复性的工作。

###1.4 C++类
构造函数中，const数据成员和自定义类数据成员，必须在初始化列表里初始化

所有单参数的构造函数都必须是explicit的，以避免后台的类型转换

C++标准定义了两个类vector和string，意在替代内置数组和字符串，增加了=复制、大小确定、==比较等，使用他们几乎总是较好的选择

###1.5 C++细节
指针最好进行初始化，可以初始化为NULL

按值调用适用于不被函数更改的小对象，按常量引用调用适用于不被函数更改的大对象，引址调用适用于所有可以被函数更改的对象

如果返回值是类类型的，可以使用按常量引用返回以节省复制的开销，但必须保证返回语句中的表达式在函数返回时不被销毁。

三个默认特殊函数：

- 析构函数
- 复制构造函数：IntCell B = C； IntCell B(C); 按值调用传递时；按值返回时。  
每个数据成员依次复制，若有类对象的数据成员，调用数据成员的复制构造函数
- operator=

有指针数据成员时，默认特殊函数会造成内存泄露和浅复制

###1.6 模板
函数模板可以应需要而自动扩展-->代码膨胀

模板实参可以使用任何类类型，因此尽量使用常量引用

类模板：例如数据成员类型不确定

函数对象：定义一个包含零个数据和一个成员函数的类。使用operator(),调用cmp.operator()(x,y)可以简写为cmp(x,y)，看上去就像函数调用，因此operator()被称为函数调用操作符

``` c++
class CaseInsensitiveCompare
{
public:
    bool operator()(const string & lhs, const string & rhs) const
	{ return stricmp(lhs.c_str(), rhs.c_str()) < 0; } 
};

template <typename Object, typename Comparator>
const Object & findMax(const vector<Object> & arr, Comparator isLessThan)
{
    ...
    isLessThan(arr[maxIndex], arr[i])
    ...
}

int main()
{
    ...
    cout << findMax(arr, CaseInsensitiveCompare()) << endl;
    ...
}
```

###1.7 使用矩阵
矩阵matrix类的构造：使用向量的向量，重载operator[]。需要有两个版本的operator[],因为`to[i] = from[i]`,需要返回一个常量引用给from，返回一个普通引用给to。

【C++这样重载可以正常工作吗？为什么知道调用哪个operator[]？】  
【更新：我在[第三章](/blog/2013/03/14/data-structures-notes-3/)里看到了解释】

``` c++
public:
    const vector<Object> & operator[](int row) const
	{ return array[row]; }
    vector<Object> & operator[](int row)
	{ return array[row]; }
    
private:
    vector< vector<Object> > array;
```