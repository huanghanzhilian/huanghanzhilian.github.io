---
layout: post
title: JavaScript 精粹 基础 进阶(1)数据类型
subtitle: JavaScript 精粹 基础 进阶(1)数据类型
author: 继小鹏
date: 2016-12-20 11:53:14 +0800
category: JavaScript
tag: proto
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