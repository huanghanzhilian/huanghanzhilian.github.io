---
layout: post
title: 基本概念和用法-MVC 2-1
subtitle: 基本概念和用法-MVC 2-1
author: 继小鹏
date: 2017-03-11 23:47:13 +0800
category: AngularJS
tag: AngularJS
---

###为什么需要MVC？


1. 代码规模越来越大，切分职责是大势所趋


2. 为了复用，很多逻辑是一模一样的，这个时候会把他抽出来形成公共的代码，这个时候如果不适应MVC这样的手段是没有办法把这些逻辑抽出来。

3. 为了后期维护的方便，修改一块功能不影响其他功能


>MVC只是手段，终极目的是模块化和复用。


###前端MVC的困难在哪里



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-2be230d9a86970f1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




####如何使用Controller

```html
<!doctype html>
<html ng-app>
    <head>
        <meta charset="utf-8">
    </head>
    <body>
        <!-- ng-controller控制器 -->
        <div ng-controller="HelloAngular">
            <p>{{greeting.text}},Angular</p>
        </div>
    </body>
    <script src="js/angular-1.3.0.js"></script>
    <script src="HelloAngular_MVC.js"></script>
</html>


```



```javascript

function HelloAngular($scope) {
    $scope.greeting = {
        text: 'Hello'
    };
}

```


ng-controller控制器是由一个函数来充当的，
AngularJS是通过ng-controller这样一个指令来实现他的控制器的，


>控制器基本思想和理论



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-722e3c9398e1d42c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


MVC是模型-视图-控制器的意思，在一般经典的状态下，控制器是负责和视图进行双向交互，也负责跟数据模型进行双向交互，但是视图和数据模型之间是没有交互的，

在最简单的情况下，一个控制器是可以控制多个视图的，但是这样一来会有个问题，如果视图1和视图2没有任何的关系，或者说根本没有业务上的的逻辑关系，这个时候控制器的角色就会很尴尬，

因为我们会把视图1和视图2的代码放到控制器，控制器就成了大杂烩。


第一种这种实现方式实际上在一些小型的项目中可以这样去实现，很显然是没有办法去应对大型项目，很自认就有了第二种方式



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-d6fd6a333cb4b9fc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



也就是说我们一个控制器只负责跟一个视图进行双向交互，这两个试图如果他们需要公用数据模型，那么我们就在控制器里面来共同使用同一份数据模型就可以了，这是改进的MVC实现


这个时候又出现一个问题如果我们控制器1和控制器2里面会出现相同的内容，这个时候应该怎么做




![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-c90583a9e031f24b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



有人会说我可以把他抽出来实现一个同用的控制器继承通用的控制器这样就能让公共部分放到公共的控制器里，这种方式是部队的



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-8b99efd8b3c3eaf8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


我们可以通过Service去做，这个是AngularJS官方的实现方式，首先会把公用的东西抽成一个服务让控制器去调用他，而不是让控制器去继承，


```html
<!doctype html>
<html ng-app>
    <head>
        <meta charset="utf-8">
    </head>
    <body>
        <div ng-controller="Commoncontroller">
            <!-- ng-controller控制器 -->
            <div ng-controller="Controller1">
                <p>{{greeting.text}},Angular</p>
                <button ng-click="test1()">test1</button>
                <button ng-click="commonFn()">通用</button>
            </div>
            <div ng-controller="Controller2">
                <p>{{greeting.text}},Angular</p>
                <button ng-click="test2()">test2</button>
                <button ng-click="commonFn()">通用</button>
            </div>
            <button ng-click="commonFn()">通用</button>
        </div>
    </body>
    <script src="js/angular-1.3.0.js"></script>
    <script src="HelloAngular_MVC3.js"></script>
</html>
```

```javascriot
function Commoncontroller($scope) {
    $scope.commonFn = function(){
    	alert("这里是通用功能")
    }
}

function Controller1($scope) {
    $scope.greeting = {
    	test:'hello1'
    }
    $scope.test1=function(){
    	alert("test1")
    }
}

function Controller2($scope) {
    $scope.greeting = {
    	test:'hello2'
    }
    $scope.test2=function(){
    	alert("test2")
    }
}

```



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-749caf45f76d06e4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)















###AngularJS语境下的MVC是如何实现的
