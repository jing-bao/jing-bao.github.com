---
layout: post
title: "编程之美 读书笔记 1"
date: 2013-05-16 23:29
comments: true
categories: [interview, notes]
---
书籍信息：编程之美——微软技术面试心得；《编程之美》小组著；电子工业出版社

从第二章开始看起的，毕竟是件很功利的事情。
<!--more-->
###2.1
用右移达到除2的效果，效率很高，可以多想想是否可以利用

求1的个数：var = var & (var - 1)

用查表法，以空间换时间

我写的代码
```c
int findOne(unsigned var) {
	int cnt = 0;
	while(var != 0) {
		var = var & (var - 1);
		cnt++;
	}
	return cnt;
}
```
代码修改-注意注释
```c
#include <iostream>
using namespace std;

typedef unsigned char BYTE;//常用的宏定义，unsigned int范围不进行检查的话不符合题意

int findOne(BYTE var) {
	int cnt = 0;
	while(var) {//var!=0冗余
		var &= (var - 1);//复合赋值更高效简洁
		cnt++;
	}
	return cnt;
}
int main() {
	unsigned a;//直接输入给BYTE会得到ASCII码值
	BYTE b;
	int c;
	while(cin>>a) {
		if(a>0xff) {
    //0x前缀表示16进制数，x不区分大小写。0前缀表示8进制数。没有提供直接写2进制数的方法.
    //8进制和16进制只能表达无符号的正整数
			cout<<"error"<<endl;
			continue;
		}
		b = (BYTE)a;//【？？？】是否有更恰当的方法，将输入以数字的值读入BYTE？
		c = findOne(a);
		cout<<c<<endl;
	}
	return 0;
}
```

###2.2
在比较运算符两边（如<），有符号数和无符号数要匹配

不小于N的数中出现能被5整除的个数：Z=[N/5]+[N/25]+[N/125]+...每5个一数，有一个数中含有因子5。而每25个一数，有一个数中含有因子25，即2个5，因为之前每5个一数时已经算过一次，所以再加一次

二进制最低位1的位置决定于含有因子2的个数

【？？？】问题2的解法二想不到

###2.3
把问题转化为同等性质的更小规模的问题。如果某元素个数超过整个数组的一半，那么删除一对不同的元素后，此元素依然超过一半。

【？？？】如何传递数组和指针作为实参，引用传递，const传递等

###2.4
找规律的题目，我所不擅长的。这里是分别考虑个位、十位、百位...

我写的代码
```c
unsigned long long calc(unsigned long long num) {
	unsigned long long less;
	unsigned long long sum = 0;
	unsigned long long factor = 1;
	unsigned long long temp = num;
	while(temp != 0) {
		less = temp - temp / 10 * 10;
		temp = temp/10;
		if(less == 0)
			sum += num * factor;
		else if(less == 1)
			sum += temp * factor + num - (num / factor * factor) + 1;
		else
			sum += (temp + 1) * factor;
		factor *= 10;
	}
	return sum;
}
```
代码修改
```c
unsigned long long calc(unsigned long long num) {
	unsigned long long sum = 0;
	unsigned long long factor = 1;
	unsigned long long lowerNum = 0;
	unsigned long long currNum = 0;
	unsigned long long higherNum = 0;
	//改用这些变量后程序可读性增强了很多
	//写程序时还是要抓住解决方案的本质，用简明的方法表达出来
	//如果写的时候觉得很难表达清楚，一定是还没有理清思路，不要急着开始写程序

	while(num / factor != 0) {
		lowerNum = num - num / factor * factor;
		currNum = (num / factor) % 10;
		higherNum = num / factor / 10;

		if(currNum == 0)
			sum += higherNum * factor;
		else if(currNum == 1)
			sum += higherNum * factor + lowerNum + 1;
		else
			sum += (higherNum + 1) * factor;
		factor *= 10;
	}
	return sum;
}
```
【？？？问题2实在是太绕了，没看明白】