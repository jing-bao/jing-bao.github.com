---
layout: post
title: "『转载整理』javascript constructor 详解"
date: 2013-03-11 10:43
comments: true
categories: javascript
---
##constructor简介
- constructor 属性是每个具有原型的对象的原型成员。 
- 这包括除 Global 和 Math 对象之外的所有内部 JavaScript 对象。
<!--more-->
用法：  
``` javascript
// A constructor function.
function MyObj() {
    this.number = 1;
}

var x = new String("Hi");

if (x.constructor == String)
    document.write("Object is a String.");
document.write ("<br />");

var y = new MyObj;
if (y.constructor == MyObj)
    document.write("Object constructor is MyObj.");

// Output:
// Object is a String.
// Object constructor is MyObj.
```

##constructor和prototype
- `Foo.prototype.constructor`是系统(浏览器)创建的，默认指向Foo。
- 对象b的constructor属性是通过找到`b.__proto__.constructor`(即`Foo.prototype.constructor`)访问的，存在于原型链中，而非对象本身属性

``` javascript
// 构造函数
function Foo(y) {
  this.y = y;
}
 
//Foo.prototype存储了新创建对象的原型的引用，我们可以使用它来定义共享的属性或方法 
Foo.prototype.x = 10;
Foo.prototype.calculate = function (z) {
  return this.x + this.y + z;
};

var b = new Foo(20);
var c = new Foo(30);

console.log(
  // Foo.prototype 会自动创建一个特别的属性constructor指向构造函数Foo
  // 对象b和c可以通过代理找到此构造函数
  b.constructor === Foo, // true
  c.constructor === Foo, // true
  Foo.prototype.constructor === Foo // true
);
```

以上对象的关系如下图所示
{% img /images/javascript-constructor-explanation-relation.jpg '关系示意图' '图片加载失败:(' %}

##constructor易变
constructor易变，是因为函数的prototype属性容易被更改。例如：

``` javascript 
function F() {} 
F.prototype = { 
_name: 'Eric', 
getName: function() { 
return this._name; 
} 
}; 

var f = new F(); 
alert(f.constructor === F); // output false

alert(f.constructor === Object.prototype.constructor);//output true 
alert(f.constructor === Object);// also output true  
```

F的原型被开发者重写了，这种方式将原有的prototype对象用一个对象的字面量{}来代替。而新建的对象{}只是Object的一个实例，系统（浏览器）在解析的时候并不会在{}上自动添加一个constructor属性，因为这是function创建时的专属操作，仅当你声明函数的时候解析器才会做此动作。  
因为{}是创建对象的一种简写，所以{}相当于是new Object()。因为{}是Object的实例，而Object.prototype上有一个指向Object本身的constructor属性。所以可以看出f.constructor其实就是Object.prototype的constructor，

一个解决办法就是手动恢复他的constructor：

``` javascript
function F() {} 
F.prototype = { 
constructor: F, /* reset constructor */ 
_name: 'Eric', 
getName: function() { 
return this._name; 
} 
};
 
var f = new F(); 
alert(f.constructor === F); // output true this time ^^ 
```

***
参考链接：  
[constructor 属性 (JavaScript)](http://msdn.microsoft.com/zh-cn/library/c1hcx253.aspx)  
[V8引擎实现标准ECMA-262（三）](http://blog.csdn.net/wanghui499917270/article/details/7171542)  
[揭开constructor属性的神秘面纱](http://www.douban.com/group/topic/23280359/)