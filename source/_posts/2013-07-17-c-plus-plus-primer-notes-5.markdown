---
layout: post
title: "C++ Primer 读书笔记 5"
date: 2013-07-17 23:43
comments: true
categories: [c++, notes]
---
##第五章
<!--more-->
###5.1
`%`求余符号，只支持整型操作数，`bool` `char` `short` `int` `long`

###5.2
短路求值：`expr1 && expr2` 逻辑与，只有expr1为true时，才会计算expr2。逻辑或类似

###5.3
位操作符将整型操作数视为二进制位的集合，为每一位提供检验和设置的功能，也可用于bitset类型的操作数

操作符   |  功能
:---:   |  :---:
~       |  位求反
<<      |  左移，填入0
>>      |  右移，无符号数填入0，有符号数填入符号位副本或者0值
&       |  位与
^       |  位异或
“竖”打不出来 | 位或

大多数二元操作符都是左结合：`( cout<< "hi" ) << "there";`

###5.4
赋值操作符具有右结合性  
复合赋值操作符：`+=` (减 / 乘 / 除) `<<=` `>>=` `&=` (位异或 / 位或)。  
使用复合赋值操作符时，左操作数只计算了一次

###5.5
前自增返回修改后的值作为表达式结果，所以返回对象本身，是左值。后自增返回未修改的值，是右值

###5.7
`cond ? expr1 : expr2`cond总是要计算的，true则计算expr1,false则计算expr2
###5.8
sizeof返回size_t类型
```c
sizeof(type name);
sizeof(expr);
sizeof expr;

sizeof（char) //保证得1 （推断：单位为byte）
sizeof(引用）//返回存放此引用类型对象所需的内存空间的大小
sizeof(指针）//返回存放指针所需的内存大小
sizeof(数组）//等效于sizeof（元素类型）* 元素个数
```

###5.9
逗号操作符的表达式从左向右计算，结果是最右边表达式的值

###5.10
`if (ia[index++] < ia[index])`此表达式行为未定义，因为<不保证先求解左操作数还是右操作数

###5.11
```c
string *ps = new string(10, '9'); //初始化为999999999
string *ps2 = new string();       //值初始化为空string
int x();//声明了函数。值初始化的()必须放在类型名后面，而非变量后
```

###5.12
[整型提升](http://mwtx.blog.163.com/blog/static/3893912920117246365145/)  
两个操作数都不是三种浮点类型之一，它们一定是某种整值类型。在确定共同的目标提升类型之前，编译器将在所有小于int的整值类型上施加一个被称为整型提升(integral promotion)的过程。整型提升就是char、short int和位段类型（无论signed或unsigned）以及枚举类型将被提升为int， 前提是int 能够完整地容纳原先的数据，否则将被转换为unsigned int。wchar_t和枚举类型被提升为能够表示其底层类型(underlying type)所有值的最小整数类型。   
一旦整型提升执行完毕，类型比较就又一次开始。如果一个操作数是unsigned long型，则第二个也被转换成unsigned long型。  
如果该类型所有可能值都包容在int内（取决于机器），则被提升为int。否则被提升为unsigned int。  
如果涉及到int提升为unsigned int且int恰好为负数，则结果是该负数对该类型的取值个数求模后的值.  

`while(cin>>s)`将istream类型转换为bool类型，意味着要检验流的状态，如果最后一次尝试成功，则true；否则false

####[强制类型转换](http://blog.csdn.net/geeeeeeee/article/details/3427920)
```c
static_cast<int>(a)
dynamic_cast<>()      //支持运行时识别指针或引用所指向的对象
const_cast<>()        //转换掉const性质
reinterpret_cast<>()  //为操作数的位模式提供较低层次的重新解释

(char*)a
int(a)
//旧式强制类型转换
//在合法使用static_cast或const_cast的地方，提供相应功能，如果两者皆不合法，执行reinterpret_cast
```
