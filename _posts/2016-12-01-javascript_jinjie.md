---
layout: post
title: JavaScript 精粹 基础 进阶
subtitle: JavaScript 精粹 基础 进阶
author: 继小鹏
date: 2016-12-01 21:13:58 +0800
category: JavaScript
tag: JavaScript
---
![图片.png](http://upload-images.jianshu.io/upload_images/3877962-a5cb0943cd4c3e52.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)





####数据类型

>JavaScript六种数据类型


![图片.png](http://upload-images.jianshu.io/upload_images/3877962-98de2022473cc4a3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**JavaScript一共有六种数据类型，其中有五种原始类型，和一种对象类型。**

>JavaScript 隐式转换

    var x='The answer'+42;//The answer42
    var y=42+'The answer';//42The answer

**这里的加号可以理解为字符串的拼接**

    var x="37"-7;    //30
    var y="37"+7;    //377

**这里的减号会理解为减法运算，而加号会理解为字符串拼接**

*等于判断*

    var x="1.23"==1.23;//true  当等于一边是字符串一边是数字的时候会尝试把字符串转换为数字再进行比较
    var y=0==false;//true
    var e=null==undefined;//true
    var c=new Object()==new Object();//false
    var d=[1,2]==[1,2];//false

**类型不同，尝试类型转换和比较**

 
类型不同，尝试类型转换和比较:    
null == undefined 相等    
number == string 转number     1 == “1.0" // true    
boolean == ?  转number       1 == true  // true    
object == number | string 尝试对象转为基本类型  new String('hi') == ‘hi’ // true    
其它：false    





>严格等于

**a===b**
顾名思义，它首先会判断等号两边的类型，如果两边类型不同，返回`false`    
如果类型相同，    
类型相同，同===   

    var h=null===null;//true   
    var f=undefined===undefined;//true
    var g=NaN===NaN;//false    number类型它和任何值比较都不会相等，包括和它自己
    var l=new Object()===new Object();//false   对象应该用引用去比较，而不是用值去比较，因为它们不是完全相同的对象。所以我们定义对象x和y去比较只有这样才会是true

>JavaScript 包装对象

原始类型`number`，`string`，`boolean`这三种原始类型都有对应的包装类型。

    var a = “string”;
    alert(a.length);//6
    a.t = 3;
    alert(a.t);//undefined

**字符串，当把一个基本类型尝试以对象的方式去使用它的时候，比如访问length属性，js会很智能的把基本类型转换为对应的包装类型对象。相当于new了string。当完成访问后，这个临时对象会被销毁掉。所以a.t赋值为3后再去输出a.t值是undefined**

>JavaScript 类型检测

**类型检测有以下几种**

1. typeof
2. instanceof
3. Object.prototype.toString
4. constructor
5. duck type

**最常见的`typeof`它会返回一个字符串，非常适合函数对象和基本类型的判断**

    typeof 100 === “number”
    typeof true === “boolean”
    typeof function () {} === “function”

    typeof(undefined) ) === “undefined”
    typeof(new Object() ) === “object”
    typeof( [1， 2] ) === “object”
    typeof(NaN ) === “number”
    typeof(null) === “object”

*`typeof`判断一些基本类型，函数对象的时候非常方便。但是对于其他对象的类型判断就会没有办法了。比如说想判断一个对象是不是数组，如果用`typeof`会返回`object`显然不是我想要的。*

**对于判断对象类型的话，常用`obj instanceof Object`**    

    [1, 2] instanceof Array === true
    new Object() instanceof Array === false

    function person(){};
    function student(){};
    student.prototype=new person();
    var bosn =new student();
    console.log(bosn instanceof student)//true
    var one =new person();
    console.log(one instanceof person)//true
    console.log(bosn instanceof person)//true

**Object.prototype.toString**

**IE6/7/8 Object.prototype.toString.apply(null) 返回”[object Object]”
**

    Object.prototype.toString.apply([]); === “[object Array]”;
    Object.prototype.toString.apply(function(){}); === “[object Function]”;
    Object.prototype.toString.apply(null); === “[object Null]”
    Object.prototype.toString.apply(undefined); === “[object Undefined]”


`typeof`
适合基本类型及function检测，遇到null失效。

`[[Class]]`
通过{}.toString拿到，适合内置对象和基元类型，遇到null和undefined失效(IE678等返回[object Object])。

`instanceof`
适合自定义对象，也可以用来检测原生对象，在不同iframe和window间检测时失效。


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


>未完！！！










































































