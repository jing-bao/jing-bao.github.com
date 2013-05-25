---
layout: post
title: "C++ Primer 读书笔记 2"
date: 2013-05-24 00:01
comments: true
categories: [c++, notes]
---
有些细节的地方综合网络作了补充
<!--more-->
##第二章
###2.1
8bits - byte，4bytes - word


 | 基本内置类型 | 
:---   | :---: | :---
char: |	1byte 	| 基本字符集<br>char/unsigned char/signed char<br>(使用unsigned或signed来表示char，由编译器决定)
wchar_t：| 2 or 4bytes | 扩展字符集（汉字和日文）
bool： |	1byte
   |    |    
short int (short)： |	2bytes | 半个word，short/unsigned short
int: |	4bytes |	一个word，int/unsigned
long int (long)： |	4bytes |	long/unsigned long
long long int:  | 8bytes
   |    |    
float： |	4bytes |	~7 digits
double： |	8bytes |	~15 digits
long double： |	8bytes |	~15 digits

- 无符号，最高位和其它位一样，用来表示该数的大小。 
- 有符号，最高位为符号位，1表示负值。

正数按原码存放，负数按补码存放。补码就是符号位不变，其余位数取反加1（从最低位开始至找到的第一个1均不变，符号位不变）。-1，原码是10000001，补码应该是11111111

很多时候，把超过取值范围的值赋给unsigned和signed类型，编译器会取模后存储。

###2.2
字面值常量literal constant。只有内置类型存在字面值。
```c
20;//十进制 
024;//八进制以0开头 
0x14;//十六进制以0X或0x开头
128u;//unsigned
1024UL or 1024Lu;//unsigned long
1L;//long
3.14159f;
.001f;
0.;
1E-3F;
L'a';//wchar_t
\0;//转义字符\可以将ASCII码转为字面值常量
L"a wide string literal";
```

###2.3
变量初始化：

```c
int ival(1024);//直接初始化
int ival = 1024;//复制初始化
double salary = 9.9, wage(salary + 0.01);//用前一个变量初始化后一个变量
```

定义未初始化的变量时：

- 内置类型：函数体外定义的自动初始化为0，函数体内定义的不自动初始化
- 类类型：必须有默认构造函数才能自动初始化，否则无法定义没有初始值的变量

变量的定义用于为变量分配存储空间，可以指定初始值。变量有且仅有一个定义。  
变量的声明用于向程序表明变量的类型和名字，可以声明多次。定义也是声明。

```c
extern int i;//声明不定义，在file2，可以使用file1里的变量i(外部存储类型)
int i;//定义，在file1

extern double pi = 3.1416;//定义，因为有初始化，要分配空间
```

变量的作用域

- 全局作用域：所有函数外部，在程序的任何地方访问
- 局部作用域：`{}`括起来的代码范围，例如函数内部
- 语句作用域：for等语句内，从它定义的位置开始，一直到for所带语句的作用域结束
- 类作用域：每个类的成员不同于任何其他类(任何其他作用域)的成员
- 命名空间作用域：namespace

###2.4
```c
const int bufSize = 512;//常量在定义时必须初始化

extern const int bufSize = fcn();//定义，在file1
//在全局作用域声明的const变量是定义该对象的文件的局部变量，不能被其他文件访问
//需要加extern，才能被其他文件访问
extern const int bufSize;//声明，在file2，可以使用file1里的变量
```

###2.5
```c
int ival = 1024;
int &refval = ival;//引用只是别名，引用不可能在初始化后绑定到其他对象
int &refVal2;//错误，引用必须初始化
int &refVal3 = 10;//错误，引用必需使用同类型的对象初始化

const int i = 1024;
const int &ref = i;//指向const对象的引用
const int &r = 42;//正确，const引用可以绑定到右值

double dval = 3.14;
const int &ri = dval;//正确，const引用可以绑定到不同但相关的类型
//编译器会做如下转换，如果允许非const引用，则对ri的修改只能修改temp，不能修改dval
//int temp = dval;
//const int &ri = temp;
```

###2.6
typedef可以用来定义类型的同义词
```c
typedef int exam_socre;
```

###2.7
枚举变量，默认第一个枚举成员赋值为0，后面比前面一个加1
```c
enum Points{point2d = 2, point2w, point3d = 3, point3w};
//point2d = 2, point2w = 3比前一个大1
//point3d = 3, 和point2w值重复了，是可以的
//point3w = 4
```

###2.8
`class`在第一个访问标号前的任何成员都隐式指定为`private`，`struct`则指定为`public`

###2.9
编译和链接多个源文件
```
CC -c main.cc Sales_item.cc
```
或者
```
CC -c main.cc
CC -c Sales_item.cc
CC main.o Sales_item.o
```

头文件中只能有声明，不能有定义。例外：

- 定义类
- 定义值在编译时就已知的const对象  
上文“const变量是定义该对象的文件的局部变量”就是为了允许这么做  
实践中，大部分编译器会用相应的常量表达式来替换对这些const变量的使用，所以不会有存储空间存储这类const变量  
不是编译时已知值的const对象不应该放在头文件
- 定义inline函数