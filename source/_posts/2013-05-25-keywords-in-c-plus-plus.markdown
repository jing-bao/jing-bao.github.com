---
layout: post
title: "『转载整理』C++关键字"
date: 2013-05-25 20:30
comments: true
categories: [c++, interview]
---
extern,const,static
<!--more-->
##编译单元(模块)
当在VC这样的开发工具上编写完代码，点击编译按钮准备生成exe文件时，VC其实做了两步工作 
 
- 第一步，将每个.cpp(.c)和相应.h文件编译成obj文件.
- 第二步，将工程中所有的obj文件进行LINK生成最终的.exe文件.

那么错误就有可能在两个地方产生，一个是编译时的错误，这个主要是语法错误，另一个是连接错误，主要是重复定义变量等。

我们所说的编译单元就是指在编译阶段生成的每个obj文件，一个obj文件就是一个编译单元，也就是说一个cpp(.c)和它相应的.h文件共同组成了一个编译单元，一个工程由很多个编译单元组成，每个obj文件里包含了变量存储的相对地址等 。

##extern
###修饰变量或函数
[前面文章](http://ziwu1216.github.io/blog/2013/05/24/c-plus-plus-primer-notes-2/)提过了，通常，在模块的头文件中,对本模块提供给其它模块引用的函数和全局变量以关键字extern声明

被extern修饰的变量（外部变量）是静态分配空间的，即程序开始时分配，结束时释放。

###extern C
`void foo( int x, int y );`函数被C编译器编译后在符号库中的名字为`_foo`，而C++编译器则会产生像`_foo_int_int`之类的名字

`extern C`告诉编译器在编译fun这个函数名时按着C的规则去翻译相应的函数名而不是C++的

目的：实现C++与C及其它语言的混合编程

- 在C++中引用C语言中的函数和变量
```c
/* c语言头文件：cExample.h */
#ifndef C_EXAMPLE_H
#define C_EXAMPLE_H
  extern int add(int x,int y);
#endif

/* c语言实现文件：cExample.c */
#include "cExample.h"
int add( int x, int y )
{
  return x + y;
}

// c++实现文件，调用add：cppFile.cpp
extern "C"
{
  #include "cExample.h"
}
int main(int argc, char* argv[])
{
  add(2,3);
  return 0;
}
```

- 在C中引用C++语言中的函数和变量
```c
//C++头文件 cppExample.h
#ifndef CPP_EXAMPLE_H
#define CPP_EXAMPLE_H
  extern "C" int add( int x, int y );
#endif

//C++实现文件 cppExample.cpp
＃include "cppExample.h"
int add( int x, int y )
{
  return x + y;
}

/* C实现文件 cFile.c
/* 这样会编译出错：＃include "cExample.h" */
extern int add( int x, int y );
int main( int argc, char* argv[] )
{
  add( 2, 3 );
  return 0;
}
```

- 常见头文件用法
```c
#ifdef __cplusplus
extern "C" {
#endif 
  /*...*/
#ifdef __cplusplus
}
#endif
```

##const
```c
const int r=100;//标准const变量声明加初始化，编译器经过类型检查后直接用100在编译时替换。
//比#define的好处在于有类型检查

char *const cp; //到char的const指针
char const *pc1; //到const char的指针
const char *pc2; //到const char的指针（后两个声明是等同的）

//类的const对象只能访问const成员函数，而非const对象可以访问任意的成员函数，包括const成员函数；
``` 
##static
###static 局部变量
生存期为这个源程序, 不过作用域仍是局部，例如函数内部的static变量

###static 全局变量
全局变量本身就是静态存储方式, 再加上static, 是改变他的作用域, 即只能本当前文件访问

- extern和static不能同时修饰一个变量
- 当你在头文件中使用static声明了全局变量后，它也同时被定义了（默认初始化为0）
- 一般定义static全局变量时，都把它放在原文件中而不是头文件，这样就不会给其他模块造成不必要的信息污染

###static 函数
函数的默认作用域是全局可见的, 即整个源程序, 如果在函数前加上static, 表示将其作用域缩小至本文件.

###static成员
一个static成员只有唯一的一份副本，而不像常规的非static成员那样在每个对象里各有一份副本。

###static成员函数
一个需要访问类成员，而不需要针对特定对象去调用的函数，也被称为一个static成员函数,它只能访问类的静态成员。

参考链接：  
[C/C++定义全局变量/常量几种方法的区别](http://hi.baidu.com/zyxins/item/41666937440833493075a1e5)   
[C++中extern “C”含义深层探索](http://blog.csdn.net/citysheep/article/details/3923073)   
[关于const,static,extern,volatile的用法](http://hi.baidu.com/laodun/item/8dcebde3d4710fadcf2d4fc7)  
[C语言易混淆关键词详解-const, static, extern, typedef, 声明](http://www.cnblogs.com/fxjwind/archive/2011/07/05/2098631.html)
