---
layout: post
title: "『转载整理』javascript prototype 详解"
date: 2013-03-10 00:05
comments: true
categories: javascript
---
没有系统学过JavaScript语言，就要开始对JavaScript引擎做些改动，因此不可避免地撞上了JavaScript的一些独特的特性，例如prototype。

只是最基本的概念理清，不包含复杂的编程模式之类。

<!--more-->

##`__proto__`属性和原型链
- `__proto__`是每个JavaScript对象的私有属性，指向对象的原型。原型也是对象。
- 原型链是指对象上的`__proto__`链。
- 属性查找通过原型链进行。

这是一个简单的对象定义。

``` javascript
var foo = {  
  x: 10,  
  y: 20  
};  
```
这个结构定义了两个显式属性和一个隐式属性 `__proto__`，它指向foo的原型
{% img /images/javascript-prototype-explanation-proto.gif 'foo对象' '图片加载失败 :('  %}

设想两个对象有一些属性不同而其他所有属性都相同的情况，很明显，设计良好的系统应当代码重用，而不是在一个对象中重复定义。
ECMAScript没有类的概念，它是通过原型链来达到同样的效果的。

``` javascript
var a = {
  x: 10,
  calculate: function (z) {
    return this.x + this.y + z
  }
};
 
var b = {
  y: 20,
  __proto__: a
};
 
var c = {
  y: 30,
  __proto__: a
};
 
// call the inherited method
b.calculate(30); // 60
c.calculate(40); // 80
```

b和c可以访问a中定义的calculate方法，这就是通过原型链达到的。
规则很简单：如果一个方法或者属性不在一个对象本身里面，那么就尝试在原型链中查找。如果在原型中没有找到，那么就在原型的原型中查找，以此类推查找整个原型链。最先发现的属性或者方法最先使用。如果在整个原型链中都没有找到，将返回undefined值。

注意，那个在继承方法中的this值是原始的对象（b和c），而不是定义这个方法的对象（a），在这个例子中，this.y是b和c中的y，而不是a中的。然而this.x来自于a，但这是通过原型链机制得到的（x本来应该是从b和c中获得，但他们本身没有）

如果一个对象没有显式指定原型，那么默认的`__proto__`值是Object.prototype。Object.prototype本身也有一个`__proto__`，它是原型链中的最后一个，其值为null
{% img /images/javascript-prototype-explanation-prototype-chain.gif '原型链' '图片加载失败 :('  %}

##prototype和`__proto__`
- prototype是JavaScript函数具有的属性
- new的时候会把函数的prototype赋给对象的`__proto__`属性。

以下是一个简单的new的过程
``` javascript
var Person = function () { };
var p = new Person();
```

可以把new的过程拆分成三步：

``` javascript
var p={}; 
p.__proto__=Person.prototype;
Person.call(p);
```

证明一下第二步。

``` javascript
var Person = function () { };
var p = new Person();
alert(p.__proto__ === Person.prototype);//true,ie不支持访问此私有属性
```

##prototype

在学习prototype(原型)的时候，我们首先要搞明白这样两个规则：

- 使用原型可以大量减少每个对象对内存的需求量，因为对象可以继承许多属性。
- 即使属性在对象被创建之后才被添加至原型中，对象也能够继承这些属性。

``` javascript
//创建一个构造函数 “用户”
function User(name)
{
    this.name = name;
}

//实例化一个用户——>老张
var 老张 = new User('老张');

//弹窗显示老张的名字
alert(老张.name);

//定义原型，注意这里对象“老张”已经被实例化了
User.prototype.favchannel = 'CCAV';

//弹窗显示老张最喜欢的电视台
alert(老张.favchannel);
```
读属性时，顺序是：老张——>User.prototype——>Object.prototype

只有读属性的时候才会使用prototype，写属性的时候是不会用到prototype的。

``` javascript
//老李是一个新的用户
var 老李 = new User('老李');

//老李喜欢小电影
老李.favchannel = 'JAPANTV';

//弹窗显示JAPANTV
alert(老李.favchannel);

//弹窗显示老张还是中意CCAV
alert(老张.favchannel);
```

复杂点的原型链
``` javascript
var Person = function () { };
Person.prototype.Say = function () {
    alert("Person say");
}
Person.prototype.Salary = 50000;

var Programmer = function () { };
Programmer.prototype = new Person();
Programmer.prototype.WriteCode = function () {
    alert("programmer writes code");
};
Programmer.prototype.Salary = 500;

var p = new Programmer();
p.Say();
p.WriteCode();
alert(p.Salary);
```
详细推导：
`var p=new Programmer()` => `p.__proto__=Programmer.prototype`;

`Programmer.prototype=new Person()` => `Programmer.prototype.__proto__=Person.prototype`;

=> `p.__proto__.__proto__=Person.prototype`

##Next
我还看到一张更复杂的图，不过要理解下constructor属性，下次继续
***
参考链接：

[V8引擎实现标准ECMA-262（二）](http://blog.csdn.net/wanghui499917270/article/details/7170207)

[Javascript学习笔记7——原型链的原理](http://www.cnblogs.com/kym/archive/2010/01/09/1643062.html)

[JavaScript prototype原型对象](http://www.xiaoxiaozi.com/2009/06/29/995/)