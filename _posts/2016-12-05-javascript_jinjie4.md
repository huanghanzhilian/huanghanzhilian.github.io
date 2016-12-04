---
layout: post
title: JavaScript 精粹 基础 进阶(4)对象
subtitle: JavaScript 精粹 基础 进阶(4)对象
author: 继小鹏
date: 2016-12-05 02:23:40 +0800
category: JavaScript
tag: JavaScript
---
对象中包含一系列属性，这些属性是无序的。
每个属性都有一个字符串key和对应的value。

####对象

>JavaScript 对象概述


**对象中包含一系列属性，这些属性是无序的。
每个属性都有一个字符串key和对应的value。**

    var obj = {x : 1, y : 2};    //定义obj对象有两个属性x和y
    obj.x; // 1   //访问对应obj.x属性取到对应的值
    obj.y; // 2   //访问对应obj.y属性取到对应的值

*探索对象的key*


    var obj = {};    //定义对象obj
    obj[1] = 1;      //动态赋值数字1属性值为1
    obj['1'] = 2;    //动态赋值字符串1属性值为2
    console.log(obj); // Object {1: 2}  //实际上结果指向的是同一个属性

    //每个属性都有一个字符串key和对应的value。

    obj[{}] = true;    //[{}] 空对象作为key
    obj[{x : 1}] = true;     //对象带有属性的对象作为key  //js都会把它转换成字符串然后再去处理，最终指向的是同一个属性
    console.log(obj)// Object {1: 2, [object Object]: true}