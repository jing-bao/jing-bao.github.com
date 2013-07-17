---
layout: post
title: "C++ Primer 读书笔记 3"
date: 2013-07-17 23:42
comments: true
categories: [c++, notes]
---
之前一直在看，但好久没更博客了，所以这次会一次性有很多笔记
<!--more-->
##第三章
###3.1
在头文件中，不要放置`using`声明，否则相当于在每个包含该头文件的程序中都放置了同一`using`声明，不管程序是否需要。

###3.2 string类型
```c
#include <string>
using std::string
```

####构造函数
```c
string s1;
string s2(s1);
string s3("value");
string s4(n,'c');//n个c
```
####读写
`cin>>s`会读取并忽略开头所有空白字符（空格、换行、制表），读取字符直到再次遇到空白字符

`getline(cin, line)`从输入流的下一行读取，并保存读取的内容到string对象中，但不包括换行符，与`>>`的区别是不忽略开头的换行符，假设输入的第一个是换行符，则`line`为空string

####string相关操作
`string.size()`返回`string::size_type`类型，属于配套类型，可以使string类的使用与机器无关。不要将`size()`返回值赋给int，因为容易溢出。  
string类的关系比较符区分大小写，通常大写字母小于任意小写字母。  
`string s3 = s1 + s2;`或`s1 += s2;`可以连接字符串。当进行string和字符串字面值混合连接时，必须保证至少有一个操作数时string的，否则非法。

####字符的处理
```c
isalnum(c)//是否是字母或数字
isalpha(c)//是否是字母
iscntrl(c)//是否是控制字符
isdigit(c)//是否是数字
ispunct(c)//是否是标点符号
islower(c)//是否是小写字母
toupper(c)//如果是小写字母，返回其大写字母形式，否则直接返回c
```

name.h是C标准库头文件，而相应的cname则是对应得c++版本，cname中定义的名字都在std空间内，而name.h中不是

###3.3 vector类型
vector之前也有过[一些笔记](/blog/2013/03/14/data-structures-notes-3/)

vector以及其他标准库容器对象的重要属性就在于可以在运行时高效地添加元素。虽然可以对给定元素个数的vector对象预先分配内存，但更有效的方法是先初始化一个空vector对象，再动态增加元素。

###3.4 迭代器
除了使用下标`vec[n]`来访问元素外，还可以用迭代器`vector<int>::iterator`。所有容器都支持迭代器，但只有少数容器支持下标。  
`iter1 - iter2`得到的类型是`difference_type`，可正可负  
`vi.begin() + vi.size()/2`可以直接指向正中间的元素

###3.5 bitset类型
```c
#include <bitset>

bitset<32> b;//尖括号内是bitset的长度，必需参数
bitset<128> bitvec1(Oxffff);//不足的位被置为0
//string对象下标最大的字符用来初始化bitset下标为0的位。具体见后图
string str = "1101";
bitset<32> bitvec4(str);//第2和第3位置为1，其余为0
bitset<32>bitvec5(str, 5, 4);//使用从str[5]开始的4个字符的子串来初始化，子串也是从右向左

b.any()//是否存在1
b.none()//是否不存在1
b.count()//1的个数
b[pos]
b.test(pos)//pos处是否为1
b.set(pos)//若没有pos参数，所有位都置1
b.reset(pos)
b.flip(pos)//取反
b[pos].flip()//也可以
b.to_ulong()//返回unsigned long,bitvec4转化为13
```