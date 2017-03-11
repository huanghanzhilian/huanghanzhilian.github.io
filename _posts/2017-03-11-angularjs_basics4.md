---
layout: post
title: 基本概念和用法-MVC 2-2
subtitle: 基本概念和用法-MVC 2-2
author: 继小鹏
date: 2017-03-11 23:49:13 +0800
category: AngularJS
tag: AngularJS
---


###如何复用Model?


数据模型也就是Model，在AngularJS里面是怎么实现的

```html
<!DOCTYPE html>
<html lang="en" ng-app>
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <div>
        <input type="text" ng-model="greeting.text"/>
        <p>{{greeting.text}},Angular</p>
    </div>
</body>
<script src="js/angular-1.3.0.js"></script>
</html>
```


这段代码写了一个input输入框，这个里面通过ng-model这样一个指令生成一个数据模型，这个数据模型他的结构是greeting.text，在下面的p标签里面通过花括号来取值，获得greeting.text这个模型的值，
这个是Angular JS里面实现数据模型的一种方式，运行的效果是input进行输入的时候p标签会实时的去更新，p标签里面的内容实际上是绑定了ng-model相同的内容上面，这个例子里面实际上是没有写任何一段js代码的。


Angular JS 首先会在Angular JS加载完成之后他会启动，启动完了以后首先去找ng-app这个指令，找到这个指令他就认为ng-app这个标签内部所有的内容都归Angular JS去管，这个时候她就回去找子层表情所有的指令，对他进行编译操作，这个时候他就会找到ng-model，找到ng-model后会帮我们生成一个数据模型，这个数据模型是挂在所谓的$rootscope上面，也就是根作用域上面的，所以ng-app下面的所有子层标签任意一个层级上都可以去获取{{greeting.text}}这样一个值。


Angular JS里面的数据模型一般来说是不需要你自己手动去创建的，也就是说你不需要明确的去new一个model这样一个东西，Angular JS里面的数据模型他一般都是绑定在$scope这样一个对象上面的，




```html
<!doctype html>
<html ng-app="HelloAngular">
    <head>
        <meta charset="utf-8">
    </head>
    <body>
        <div ng-controller="Commoncontroller">
            <!-- ng-controller控制器 -->
            <div ng-controller="Controller1">
                <p>{{greeting.test}},Angular</p>
                <button ng-click="test1()">test1</button>
                <button ng-click="commonFn()">通用</button>
            </div>
            <div ng-controller="Controller2">
                <p>{{greeting.test}},Angular</p>
                <button ng-click="test2()">test2</button>
                <button ng-click="commonFn()">通用</button>
            </div>
            <button ng-click="commonFn()">通用</button>
        </div>
    </body>
    <script src="js/angular-1.3.0.js"></script>
    <script src="HelloAngular_MVC3lx.js"></script>
</html>

```


```javascript
var myModule = angular.module("HelloAngular", []);

myModule.controller("Commoncontroller", ['$scope',
    function($scope){
        $scope.commonFn = function(){
            alert("这里是通用功能")
        }
    }
]);

myModule.controller("Controller1", ['$scope',
    function($scope) {
        $scope.greeting = {
            test:'hello1'
        }
        $scope.test1=function(){
            alert("test1")
        }
    }
]);

myModule.controller("Controller2", ['$scope',
    function($scope) {
        $scope.greeting = {
            test:'hello2'
        }
        $scope.test2=function(){
            alert("test2")
        }
    }
]);
```


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-cdafd059174ae05e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



上面的例子中可以看到我们获得最终的hello1和hello2在界面上显示实际上都是来自于$scope这里面对象的属性这就是AngularJS实现数据模型的方式，



###如何复用Viel视图?


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



上面代码，自己定义了一个hello标签，然后借助Angular的directive机制把他替换成div内容是Hi everyone!这样一个字符串，

Angular js的视图是需要通过指令去实现的，


Angular js的MVC是借助于$scope实现的！！！


也就是说全部借助于作用域去实现的

那么作用域是什么呢


```css
.show-scope-demo .ng-scope,.show-scope-demo .ng-scope {
	border: 1px solid red;
	margin: 3px;
}
```


```html
<!doctype html>
<html ng-app>
	<head>
		<meta charset="utf-8">
		<link rel="stylesheet" type="text/css" href="Scope1.css" />
	</head>
	<body>
		<div class="show-scope-demo">
			<div ng-controller="GreetCtrl">
				Hello {{name}}!
			</div>
			<div ng-controller="ListCtrl">
				<ol>
					<li ng-repeat="name in names">
						{{name}} from {{department}}
					</li>
				</ol>
			</div>
		</div>
	</body>
	<script src="js/angular-1.3.0.js"></script>
	<script src="Scope1.js"></script>
</html>


```


```javascript
function GreetCtrl($scope, $rootScope) {
	$scope.name = 'World';
	$rootScope.department = 'Angular';
}

function ListCtrl($scope) {
	$scope.names = ['Igor', 'Misko', 'Vojta'];
}

```



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-b4655b2aa451a87c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


以上代码有两个控制器，第一个GreetCtrl，第二个ListCtrl，

GreetCtrl里面Hello {{name}}!

ListCtrl里面使用到了`ng-repeat`的指令，这个指令是用来迭代一个数组的，我们有一个names数组放了很多的姓名，

使用`ng-repeat`的指令可以将数组一个个输出，



```javascript
function GreetCtrl($scope, $rootScope) {
	$scope.name = 'World';
	$rootScope.department = 'Angular';
}

function ListCtrl($scope) {
	$scope.names = ['Igor', 'Misko', 'Vojta'];
}

```


GreetCtrl控制器中$scope.name设置了 'World';的字符串

然后在$rootScope上面创建了一个属性叫做department 

$rootScope也就是根作用域，$rootScope是处于最顶层的作用域对象，

整个作用域就像html标签一样是一个树型的结构，$rootScope就类似于根标签html。



ListCtrl控制器中有一个names数组，数组有三个内容，在页面上通过`ng-repeat`的指令把他输出


页面显示结果告诉我们department 是根作用于上的属性，也就是说作用域是有一个层次结构的，在内层的作用域上面如果获得不到的属性他就会一一向上查找，这个过程其实和js里面的原型继承或者说原型查找机制是一模一样的。






例子



```html
<!doctype html>
<html ng-app>
	<head>
		<meta charset="utf-8">
		<link rel="stylesheet" type="text/css" href="Scope1.css" />
	</head>
	<body>
		<div ng-controller="EventController">
			Root scope
			<tt>MyEvent</tt> count: {{count}}
			<ul>
				<li ng-repeat="i in [1]" ng-controller="EventController">
					<button ng-click="$emit('MyEvent')">
						$emit('MyEvent')
					</button>
					<button ng-click="$broadcast('MyEvent')">
						$broadcast('MyEvent')
					</button>
					<br>
					Middle scope
					<tt>MyEvent</tt> count: {{count}}
					<ul>
						<li ng-repeat="item in [1, 2]" ng-controller="EventController">
							Leaf scope
							<tt>MyEvent</tt> count: {{count}}
						</li>
					</ul>
				</li>
			</ul>
		</div>
	</body>
	<script src="js/angular-1.3.0.js"></script>
	<script src="Scope2.js"></script>
</html>
```


```javascript
function EventController($scope) {
	$scope.count = 0;
	$scope.$on('MyEvent', function() {
		$scope.count++;
	});
}

```




![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-89829b5d8e760d43.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-ccb6f14408557f5c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




ng-repeat="i in [1]"是内联表达式。



```javascript
function EventController($scope) {
	$scope.count = 0;
	$scope.$on('MyEvent', function() {
		$scope.count++;
	});
}

```


这里的js，先初始化一个$scope.count = 0;

当收到MyEvent的时候$scope上的count就会加1




###神奇的$scope


**$scope是一个POJO(Plain Old Javascript Object)**

$scope是一个普通的js对象


**$scope提供了一些工具方法$watch()/$apply()**

$watch()/$apply()是用来实时监控一些属性的变化的一般来说不会手动去调用这些方法。他会在内部帮我们去监控


**$scope是表达式的执行环境（或者叫作用域）**


**$scope是一个树型结构，与DOM标签平行**


**子$scope对象会继承父$scope上的属性和方法**


**每一个AngularJS应用只有一个根$scope对象，（一般位于ng-app上）**


**$scope可以传播事件，类似于DOM事件，可以向上也可以向下**


**$scope不仅是MVC的基础，也是后面实现双向数据绑定的基础**


**可以用angular.element($0).scope()进行调试**





![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-b407f798e7435176.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


安装调试插件