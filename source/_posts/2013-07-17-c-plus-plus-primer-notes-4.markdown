---
layout: post
title: "C++ Primer 读书笔记 4"
date: 2013-07-17 23:42
comments: true
categories: [c++, notes]
---
##第四章
<!--more-->
###4.1
关于数组定义
```c
int size = 100;
int vals[size];//错，因为size不是const，所以只有运行时才能确定值，不能用于指定数组维数

int ia[] = {0, 1, 2};
//显示初始化的数组不需要指定维数。如果指定维数，大于提供元素个数，其余默认初始化（0或默认构造）。
//若没有显式初始化
//内置类型的数组，在函数体外元素均初始化为0，函数体内无初始化
//类类型的数组，无论在哪里定义，都调用该类的默认构造函数进行初始化。若没有默认构造函数，必须显示初始化

char ca1[] = {'C', '+', '+, '\0'};
char ca2[] = "C++";//同样有结束字符
```

数组下标的正确类型是`size_t`

###4.2
取地址符`&`只能用于左值

建议指针定义时初始化，如果还没有对象，初始化为0

指针和引用的区别：
  
- 定义引用时必须初始化，以后始终指向同一个特定对象
- 给引用赋值，修改的是该引用所关联的对象的值，而不是使引用关联另一个对象

在表达式中使用数组名时，该名字会自动转换为指向数组第一个元素的指针
```c
int ia[] = {0, 2, 4, 6, 8};
int *ip = ia; //ip指向ia[0]
int *ip2 = ia +4; //指向ia[4]
ptrdiff_t n = ip2 - ip;// 4

int *p = &ia[2];
int k = p[-2]; //ia[0]
```

只要定义的多个变量具有相同的类型，就可以在for循环的初始化语句中同时定义它们
```c
for (int *pbegin = int_arr, *pend = int_arr + arr_sz; pbegin != pend; ++pbegin)
	dosomething;
```

关于const
```c
const double *cptr;//指向const对象的指针

const double pi = 3.14;
double *ptr = &pi; //错误！不能把const对象的地址赋给指向非const对象的指针
//但允许把非const对象的地址赋给指向const对象的指针，常见于函数形参

int *const curErr = &errNum;//const指针，定义时初始化

typedef string *pstring;
const pstring cstr; //等价于string *const cstr;

string const s1; //等价于const string s1;
```

###4.3
字符串字面值是const char类型的数组，是C风格字符串（以null结束的字符数组）的实例。
```c
const char *cp = "C++"; //指向第一个元素的指针

while(*cp)              //null字符为false
{
	do something;
	cp++;
}
```

关于char指针的输出
```c
const char *cp = "C++";
cout<<*cp<<endl;       //C
cout<<cp<<endl;        //C++
cout<<(void*)cp<<endl; //0x804e739

char ch = 'c';
char *p = &ch;
cout<<*p<<endl;        //c
cout<<p<<endl;         //c
cout<<(void*)p<<endl;  //0xbf7481fb
```

标准库函数
```c
#include <cstring>
strlen(s)   //不包括null的长度
strcmp(s1, s2)
strncpy(s1, s2, n)
strncat(s1, s2, n)

char largeStr[16+18+2];      //记住预留存放null的空间
strncpy(largeStr, cp1, 17);  //比使用相应简化版本strcpy(largeStr, cp1)安全
strncat(largeStr, " ", 2);   //包括null的大小
strncat(largeStr, cp2, 19);

//对应C++标准库类型string，增强安全性，且提高效率
string largeStr = cp1;
largeStr += " ";
largeStr += cp2;
```

动态数组
```c
int *pia = new int[10];  //内置类型无初始化。类类型使用默认构造函数初始化
int *pia2 = new int[10]; //所有元素初始化为0。不能像数组变量一样提供非默认值的初始化
const int *pci_bad = new const int[10];//错误！const必须初始化
char *p = new char[0];   //合法
char arr[0];             //错误！
```

- 可以把C风格字符串用在任何可以使用字符串字面值的地方，例如对string对象进行初始化或赋值
- 不可直接使用string对象在要求C风格字符串的地方，但可以使用`const char *str = st2.c_str();`
- 可以使用数组初始化vector。`vector<int> ivec(int_arr, int_arr + arr_size);`

###4.4
严格的说，C++没有多维数组，只是数组的数组
```c
int ia[3][4] = {
	{0, 1, 2, 3},
	{4, 5, 6, 7},
	{8, 9, 10, 11};   //大小为3的数组，其中每个元素是一个大小为4的数组,3行，4列
int ia[3][4] = {0,1,2,3,4,5,6,7,8,9,10,11};//等价

int（*ip）[4] = ia;//多维数组名其实是指向第一个内层数组的指针.从内向外理解，指向含有4个元素的数组的指针

typedef int int_array[4];//更易理解
int_array *ip = ia;
```

