---
layout: post
title: "『转载整理』new和malloc的区别"
date: 2013-05-25 17:25
comments: true
categories: [c++,interview]
---
new可以认为是malloc加构造函数的执行
<!--more-->
主要区别列表如下：

new		|	malloc
-------		|	------
建立对象	|	分配内存
c或c++		|	c
运算符		|	标准库函数
保留字,不需要头文件支持		|	需要头文件库函数支持
分配内存+调用类的构造函数	|	只分配内存，不会进行初始化
自动计算需要分配的空间		|	手动传递字节数
返回某种数据类型指针		|	返回void指针

参考链接：  
[new与malloc](http://wmnmtm.blog.163.com/blog/static/38245714201203313587/)

