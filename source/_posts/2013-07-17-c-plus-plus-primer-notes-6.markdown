---
layout: post
title: "C++ Primer 读书笔记 6"
date: 2013-07-17 23:43
comments: true
categories: [c++, notes]
---
##第六章
<!--more-->
###6.3
复合语句，通常称为块，是用一对花括号括起来的语句序列，标识了一个作用域。

###6.4
变量的作用域
```c
for(int i = 0; i <10; i++)
{//新的作用域，包含在for语句作用域内
	sum += i;
}
//外部不能访问i
```

###6.6
`break`语句中，`case`标号必须是整型常量表达式

程序从匹配的`case`标号开始执行，并跨越`case`边界继续执行其他语句直到结束，所以不要忘记`break`
```c
switch(ch) {
	case 'a':
		++aCnt;
	case 'e':
		++eCnt;
	case‘i’:
		++iCnt;
}
//如果ch是'e',eCnt和iCnt都会加1

switch(ch) {
	case 'a':
	case 'e':
	++Cnt;
	break;
}
//对两个case执行同样操作

switch(flag) {
	case true:
		string file_name = "a.txt";//错误！
		break;
	case false:
		...
}
//只能在最后一个case标号或default标号后面定义变量，此规则是为了避免代码跳过变量的定义和初始化
switch(flag) {
	case true:
		{
			string file_name = "a.txt";//在块语句内定义，可以
			//...
		}
		break;
	case false:
		...
}

```

###6.8
`for`语句
```c
for (initializer; condition; expression)
	statement
//循环开始时执行一次initializer，如果第一次求解condition就得false，则expression和statement都不会执行

//可以在initializer定义多个对象，但必须具有相同的一般类型
for (int ival = 0, *pi = ia, &ri = val; ival != 5; ++ival, ++pi, ++ri)
```

###6.9
`do while` 保证循环体至少执行1次

###6.11
`continue`语句导致最近的循环语句的档次迭代提前结束，对于`while`和`do while`，继续求解循环条件，对于`for`，求解`expression`

###6.12
`goto`语句
```c
goto end;
{
	int ix =10;//goto语句不能跨越变量的定义语句向前跳转，必须放在块语句内
}
end: return;

begin:
	int sz = get_size();
	if(sz <= 0) {
		goto begin;//向后跳过是合法的，会使系统撤销这个变量，然后再重新创建
	}
```

###6.13
`try catch`语句
```c
try{
	...
	throw runtime_err("Data must be intergers");
} catch （runtime_error err) {
	cout << err.what();//每个标准库异常类都有what函数，返回C风格字符串
}
```
抛出异常时，按执行路径回退，寻找匹配的`catch`，如果不存在，则转到名为`terminate`的标准库函数。在程序中出现的异常如果没有`try`块定义，也将调用`terminate`。

标准异常类在`stdexpect`头文件中，见下表  
{% img /images/c-plus-plus-primer-notes-6-exception.jpg '标准异常类' '图片加载失败 :(' %}

`exception`, `bad_alloc`, `bad_cast`只定义了默认构造函数，无法提供初值。其他异常类使用string参数初始化，这个string被`what`函数返回

###6.14
`assert`语句
```c
int main()
{
	#ifndef NDEBUG
	cerr << "starting main" << endl;
	#endif
}

CC -DNDEBUG main.C
//大多数编译器都提供定义NDEBUG的命令行选项，这样的命令行等效于在main.c开头#define NDEBUG

__FILE__
__LINE__
__TIME__
__DATE__
//预处理器还提供了这些常量，方便调试

#include <cassert>
assert(expr);
//如果expr为false，则输出信息且终止程序。若为非0值，则不做操作
```
如果NDEBUG定义，则assert语句不运行。

- [具体1](http://book.51cto.com/art/201306/400313.htm)
- [具体2](http://www.cnblogs.com/valuel/archive/2009/11/15/1603362.html)

