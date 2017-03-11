---
layout: post
title: AngularJS快速入手1-1
subtitle: AngularJS快速入手1-1
author: 继小鹏
date: 2017-03-11 23:45:13 +0800
category: AngularJS
tag: AngularJS
---


###AngularJS简介

目前最新版本1.3.0

放弃了ie8

引入了单向数据绑定

删掉了一些过时的api (据说是为了AngularJS2.0做准备)





###AngularJS-实例演示4大核心特性

**1 .MVC**



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-7706890b00d0fa54.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


```html
<!doctype html>
<html ng-app>
    <head>
        <meta charset="utf-8">
    </head>
    <body>
        <!-- ng-controller我们的控制器  ng-controller赋值成HelloAngular -->
        <div ng-controller="HelloAngular">
            <!-- p标签充当了视图的功能 -->
            <!-- {{greeting.text}}取值这个地方就是充当数据模型 -->
            <p>{{greeting.text}},Angular</p>
        </div>
    </body>
    <!-- 导入了angular-1.3.0.js文件 -->
    <script src="js/angular-1.3.0.js"></script>
    <script type="text/javascript">
        //HelloAngular这个函数充当了控制器
        function HelloAngular($scope) {
            $scope.greeting = {
                text: 'Hello'
            };
        }
    </script>
</html>
```

运行效果


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-65e2b7e3decbbd7f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




**2. 模块化**

html

```html
<!doctype html>
<html ng-app="HelloAngular">
    <head>
        <meta charset="utf-8">
    </head>
    <body>
        <div ng-controller="helloAngular">
            <p>{{greeting.text}},Angular</p>
        </div>
    </body>
    <script src="js/angular-1.3.0.js"></script>
    <script src="HelloAngular_Module.js"></script>
</html>
```


javascript

``` javascript
//这里调用了angular.module方法
var myModule = angular.module("HelloAngular", []);

myModule.controller("helloAngular", ['$scope',
    function HelloAngular($scope) {
        $scope.greeting = {
            text: 'Hello'
        };
    }
]);
```


运行效果


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-275f189bedad7699.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



在HelloAngular_Module.js文件中调用了angular.module方法，然后给他一个字符串“HelloAngular”，后面还传了方括号空数组，通过字面量看出这是在定义一个模块，var myModule是一个模块，定义完模块后在模块上面调用了一个controller方法很显然这是告诉angular要生成一个控制器"helloAngular"是控制器的名称，后面方括号里面第一个参数'$scope'是告诉angularjs帮我注入'$scope'。



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-011b390978faa7f1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



AngularJS概念有Module，有Directive，有Filter，其实一点要抓住一个点，一切都是从模块开始的，在AngularJS开发中首先想到的是模块也就是**Module**，其他所有的东西其实都是挂在**Module**下面的，因为只有把模块创建后你才能在模块上去调用Service,controller等方法，所以首先想到的是模块，



**3. 指令系统**


html

```html
<!doctype html>
<html ng-app="MyModule">
	<head>
		<meta charset="utf-8">
	</head>
	<body>
		<hello></hello>
	</body>
	<script src="js/angular-1.3.0.js"></script>
	<script src="HelloAngular_Directive.js"></script>
</html>
```


javascript

``` javascript
var myModule = angular.module("MyModule", []);
myModule.directive("hello", function() {
    return {
        restrict: 'E',
        template: '<div>Hi everyone!</div>',
        replace: true
    }
});
```


运行效果



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-d688b09b01e01ac8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



看看body表情内的<hello></hello>标签，很显然浏览器是不认识这个标签的，不认识他在默认情况下浏览器会忽略他，Angular js就会想一下怎样能让浏览器认识这个标签呢，这就需要借助Angular js的Directive这个特性，

一切都是从模块开始的，所以我们需要创建模块



``` javascript
var myModule = angular.module("MyModule", []);

```


``` javascript
var myModule = angular.module("MyModule", []);
myModule.directive("hello", function() {
    return {
        restrict: 'E',
        template: '<div>Hi everyone!</div>',
        replace: true
    }
});
```

模块创建好再调用模块上面directive方法，这个方法也有两个参数`"hello"`是指令的名称，也就是对应的标签名，后面是一个函数，这个函数就是生成标签的， template: '<div>Hi everyone!</div>',



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-15bff53254dc9fec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


最终hello是被替换成模板template，



这个就是angular的指令
我们可以自定义一大堆的指令然后做一些封装我们在调用的时候会非常方便，当然指令不只是定义标签这么简单，还有很多其他功能，比如说


```html
<!doctype html>
<html ng-app="MyModule">
	<head>
		<meta charset="utf-8">
	</head>
	<body>
		<hello></hello>
	</body>
	<script src="js/angular-1.3.0.js"></script>
	<script src="HelloAngular_Directive.js"></script>
</html>
```


我们在html里面ng-app="MyModule",这个实际上就是一个指令，这里复制ng-app等于"MyModule"

"MyModule"在var myModule = angular.module("MyModule", []);js里，很显然是告诉Angular js要去使用我们这个模块，ng-app意思和C语言里的main或者是java里面的main方法。


Angular js检测到ng-app这个指令的时候就知道我从ng-app这个指令开始内部的标签内容就归我Angular js来管了，也就是说Angular js从ng-app这个地方启动的，可以想象既然是main函数main方法，一个应用里面显然只能有一个，所以在任意一个单页Angular js应用里面ng-app这个指令只能出现一次。



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-2969be9fb6c72e5a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)








**4. 双向数据绑定**






Angular js 是实现了双向数据绑定，其他的前端框架都没有实现这样一个特性，目前大多数前端框架都是实现单向数据绑定。


来看看单向数据绑定的流程


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-4f67727e7c975367.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


首先是我们把模板写好，再加上数据，数据可能是从后台服务端读进来的，模板和数据结合在一起，通过数据绑定机制生成一段html标签然后把这段标签插入到文档流里面，这是经典单向数据绑定的处理流程。

html一旦生成完以后就没法再变了，当有新的数据的时候我们只能重新再来一遍。


Angular js认为单向数据绑定的过程实在是不怎么优雅，所以他觉得我应该实现双向数据绑定。



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-ee3f8ae4464562b3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


他的核心想法是这样的，

视图，数据是对应的当视图上的内容发生变化的时候他希望数据模型里面立刻发生变化，当数据模型发生变化的时候视图自己自动会去更新，很显然这里需要  借助一个事件机制。

在html里有什么样的视图会发生变化？表单。在很多的页面中会出现很多的表单，表单是来收集用户的输入的，这些数据是非常容易变化的，在数据发生变化就会通过Angular js机制同步到数据模型上面。


html

```html
<!doctype html>
<html ng-app>
    <head>
        <meta charset="utf-8">
    </head>
    <body>
        <div>
            <input ng-model="greeting.text"/>
            <p>{{greeting.text}},AngularJS</p>
        </div>
    </body>
    <script src="js/angular-1.3.0.js"></script>
</html>
```



运行效果



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-8df2bddb9e767ce3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



上面代码实现效果在input输入框输入任何东西底下的显示框都会立刻显示，

这是什么原理呢



```html
<input ng-model="greeting.text"/>
<p>{{greeting.text}},AngularJS</p>
```

首先有一个输入框
然后绑定了一个ng-model="greeting.text",
这个是时候底下有个p标签有个双花括号来获取greeting.text的值。
双花括号是什么意思呢？
在Angular js里面是取值的意思，是一个取值表达式。