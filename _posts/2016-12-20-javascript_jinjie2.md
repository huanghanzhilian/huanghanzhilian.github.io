---
layout: post
title: JavaScript 精粹 基础 进阶(2)表达式和运算符
subtitle: JavaScript 精粹 基础 进阶(2)表达式和运算符
author: 继小鹏
date: 2016-12-20 11:54:05 +0800
category: JavaScript
tag: proto
---

表达式是指能计算出值得任何可用程序单元。——Wiki

####表达式和运算符

>JavaScript 表达式

** 表达式是指能计算出值得任何可用程序单元。——Wiki**

**表达式是一种JS短语，可使JS解释器用来产生一个值。——《JS权威指南》**



![图片.png](http://upload-images.jianshu.io/upload_images/3877962-6919f7e22ae1bb76.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![图片.png](http://upload-images.jianshu.io/upload_images/3877962-3ab28e638286a3aa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![图片.png](http://upload-images.jianshu.io/upload_images/3877962-e80ce19d6f1a3f10.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![图片.png](http://upload-images.jianshu.io/upload_images/3877962-1e624122622bf128.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



![图片.png](http://upload-images.jianshu.io/upload_images/3877962-72f96222c9135c06.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![图片.png](http://upload-images.jianshu.io/upload_images/3877962-cdce00cca295adaa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![图片.png](http://upload-images.jianshu.io/upload_images/3877962-766d0f239887b430.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![图片.png](http://upload-images.jianshu.io/upload_images/3877962-3f85e4e4f73a4cd1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>JavaScript 运算符


![图片.png](http://upload-images.jianshu.io/upload_images/3877962-7140a60ac8368ce6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**三元运算符**

    c ? a : b
    var val = true ? 1 : 2; // val = 1
    //val值为true就会返回冒号前面的值，如果是false就会取冒号右边的值。

**逗号运算符**

    a, b
    var val = (1, 2, 3); // val = 3
    //非常少见的，它会从左到右依次去计算表达式的值，最后会取最右边的值。


**delete 运算符**

    delete obj.x;
    var obj = {x : 1};
    obj.x;                      // 1
    delete obj.x;
    obj.x;                      // undefined
    //delete 运算符就是删除对象上的属性，变量obj，obj.x被删除了。

并不是对象上的所有属性都可以成功的被delete 掉的。

    var obj = {};
    Object.defineProperty(obj, 'x', {
	  configurable : false, 
	  value : 1
    });
    delete obj.x; // false
    obj.x;            // 1

只有`configurable : true`, 为true，才可以被删除。

**`in`运算符**

    window.x = 1;  //创建全局变量x为1
    x’ in window; // true   判断是否win下有x

**instanceof, typeof运算符 **

    {} instanceof Object                // true  判断对象类型，基于原型链去判断的
    typeof 100 === ‘number’ // true  返回字符串，常用语原始类型，或者函数对象。

**new运算符**

    function Foo(){}     //创建函数构造器，或者说创建空函数
    Foo.prototype.x = 1;     //prototype属性x
    var obj = new Foo();   //创建一个新的对象obj
    obj.x;  // 1   现在就能在prototype属性x拿到1
    obj.hasOwnProperty('x'); // false     来判断这个属性到底是这个对象上的还是这个对象原型链上，这个x当然不是属于直接对象上的属性，
    obj.__proto__.hasOwnProperty('x'); // true    拿到对象原型。可以发现x是对象原型上的属性，而不是这个对象本身上的属性。

**this运算符**

    this;  // window (浏览器)  在全局下this会指向win
    var obj = {
      func : function(){return this;}
    }; 
    obj.func(); // obj
    //如果在对象值如果是个函数的话那么在这样的函数里，this会指向对象本身。

**void运算符**

void运算符是一元运算符，

    void 0  // undefined
    void(0) // undefined
    //不管值是多少都会返回undefined



![图片.png](http://upload-images.jianshu.io/upload_images/3877962-5e9759be8cdba5d3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



![图片.png](http://upload-images.jianshu.io/upload_images/3877962-227317d72f8e3d08.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)