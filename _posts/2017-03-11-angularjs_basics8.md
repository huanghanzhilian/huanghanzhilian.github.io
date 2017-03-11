---
layout: post
title: 基本概念和用法-指令 2-6
subtitle: 基本概念和用法-指令 2-6
author: 继小鹏
date: 2017-03-11 23:55:13 +0800
category: AngularJS
tag: AngularJS
---


###解析最简单的指令hello：匹配模式restrict


```html
<!doctype html>
<html ng-app="MyModule">
	<head>
		<meta charset="utf-8">
	</head>
	<body>
		<hello></hello>
		<div hello></div>
		<div class="hello"></div>
		<!-- directive:hello -->
		<div></div>
	</body>
	<script src="framework/angular-1.3.0.14/angular.js"></script>
	<script src="HelloAngular_Directive.js"></script>
</html>
```


```javascript
var myModule = angular.module("MyModule", []);
myModule.directive("hello", function() {
    return {
        restrict: 'AEMC',
        template: '<div>Hi everyone!</div>',
        replace: true
    }
});
```



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-aeca8e4842100d37.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)





**restrict**表示匹配模式

A表示属性
E表示元素
M表示注释
C表示样式类



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-2e1b8c9033ab7db2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)






###解析最简单的指令hello：template，templateUrl，$templateCache


在上面最简单的指令里面有一个配置项**template**这是最总要显示的html标签

使用这种方式编写的内容比较少，

AngularJS提供了**templateUrl**配置项，使用这个就不需要把模板写道js代码里面


把模板切成独立的html去编写，可以编写很多内容。



```html
<!doctype html>
<html ng-app="MyModule">
	<head>
		<meta charset="utf-8">
		<link rel="stylesheet" href="css/bootstrap-3.0.0/css/bootstrap.css">
	</head>
	<body>
		<hello></hello>
	</body>
	<script src="framework/angular-1.3.0.14/angular.js"></script>
	<script src="templateUrl.js"></script>
</html>
```


```javascript
var myModule = angular.module("MyModule", []);
myModule.directive("hello", function() {
    return {
        restrict: 'AECM',
        templateUrl: 'hello.html',
        replace: true
    }
});
```


hello
```html
<div class="panel panel-primary">
    <div class="panel-heading">
        <div class="panel-title">双向数据绑定</div>
    </div>
    <div class="panel-body">
        <div class="row">
            <div class="col-md-12">
                <form class="form-horizontal" role="form" ng-controller="UserInfoCtrl">
                    <div class="form-group">
                        <label class="col-md-2 control-label">
                            邮箱：
                        </label>
                        <div class="col-md-10">
                            <input type="email" class="form-control" placeholder="推荐使用126邮箱" ng-model="userInfo.email">
                        </div>
                    </div>
                    <div class="form-group">
                        <label class="col-md-2 control-label">
                            密码：
                        </label>
                        <div class="col-md-10">
                            <input type="password" class="form-control" placeholder="只能是数字、字母、下划线" ng-model="userInfo.password">
                        </div>
                    </div>
                    <div class="form-group">
                        <div class="col-md-offset-2 col-md-10">
                            <div class="checkbox">
                                <label>
                                    <input type="checkbox" ng-model="userInfo.autoLogin">自动登录
                                </label>
                            </div>
                        </div>
                    </div>
                    <div class="form-group">
                        <div class="col-md-offset-2 col-md-10">
                            <button class="btn btn-default" ng-click="getFormData()">获取Form表单的值</button>
                            <button class="btn btn-default" ng-click="setFormData()">设置Form表单的值</button>
                            <button class="btn btn-default" ng-click="resetForm()">重置表单</button>
                        </div>
                    </div>
                </form>
            </div>
        </div>
    </div>
</div>

```


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-cf3c066e122727d4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




```html
<!doctype html>
<html ng-app="MyModule">
	<head>
		<meta charset="utf-8">
	</head>
	<body>
		<hello></hello>
	</body>
	<script src="framework/angular-1.3.0.14/angular.js"></script>
	<script src="$templateCache.js"></script>
</html>
```


```javascript
var myModule = angular.module("MyModule", []);

//注射器加载完所有模块时，此方法执行一次
myModule.run(function($templateCache){
	$templateCache.put("hello.html","<div>Hello everyone!!!!!!</div>");
});

myModule.directive("hello", function($templateCache) {
    return {
        restrict: 'AECM',
        template: $templateCache.get("hello.html"),
        replace: true
    }
});

```


**$templateCache**这个配置项是缓存的意思。

`  template: $templateCache.get("hello.html"),`就可以使用


也就是说将模板缓存起来去让多个指令去使用他



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-1f01cdf23ded59fb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)








###解析最简单的指令hello：replace与trabsclude



```html
<!doctype html>
<html ng-app="MyModule">
	<head>
		<meta charset="utf-8">
	</head>
	<body>
		<hello>
			<div>这里是指令内部的内容。</div>
		</hello>
	</body>
	<script src="framework/angular-1.3.0.14/angular.js"></script>
	<script src="replace.js"></script>
</html>
```


```javascript
var myModule = angular.module("MyModule", []);
myModule.directive("hello", function() {
    return {
    	restrict:"AE",
    	template:"<div>Hello everyone!</div>",
    	replace:true
    } 
});
```






hello作为一个元素当然是可以嵌套的，


如果使用**replace**配置项内部写的内容就会被替换掉，


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-fbd8f70d905e37e3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




但是有一个div

```html
<hello>
			<div>这里是指令内部的内容。</div>
		</hello>
```

使用**trabsclude**方式


**trabsclude**表示变换，




```html
<!doctype html>
<html ng-app="MyModule">
	<head>
		<meta charset="utf-8">
	</head>
	<body>
		<hello>
			<div>这里是指令内部的内容。</div>
		</hello>
	</body>
	<script src="framework/angular-1.3.0.14/angular.js"></script>
	<script src="transclude.js"></script>
</html>
```


```javascript
var myModule = angular.module("MyModule", []);
myModule.directive("hello", function() {
    return {
    	restrict:"AE",
    	transclude:true,
    	template:"<div>Hello everyone!<div ng-transclude></div></div>"
    } 
});
```



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-6b01c1ebfddecea7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



使用**trabsclude**内部嵌套的内容会保存下来。这个有非常重要的作用，因为指令互相之间是可以嵌套的，如果最外层的指令把内部的内容全部替换，很显然内部的指令没有办法起作用。





###comile与link(操作元素，添加css样式，绑定事件)


指令在执行时候的机制


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-fd90df1125c52758.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



###指令与控制器之间的交互



```html
<!doctype html>
<html ng-app="MyModule">
	<head>
		<meta charset="utf-8">
	</head>
	<body>
		<div ng-controller="MyCtrl">
			<loader howToLoad="loadData()">滑动加载</loader>
		</div>
		<div ng-controller="MyCtrl2">
			<loader howToLoad="loadData2()">滑动加载</loader>
		</div>
	</body>
	<script src="framework/angular-1.3.0.14/angular.js"></script>
	<script src="Directive&Controller.js"></script>
</html>
```


```javascript
var myModule = angular.module("MyModule", []);
myModule.controller('MyCtrl', ['$scope', function($scope){
	$scope.loadData=function(){
		console.log("加载数据中...");
    }
}]);
myModule.controller('MyCtrl2', ['$scope', function($scope){
    $scope.loadData2=function(){
        console.log("加载数据中...22222");
    }
}]);
myModule.directive("loader", function() {
    return {
    	restrict:"AE",
    	link:function(scope,element,attrs){
    		element.bind('mouseenter', function(event) {
    			//scope.loadData();
    			// scope.$apply("loadData()");
    			// 注意这里的坑，howToLoad会被转换成小写的howtoload
    			scope.$apply(attrs.howtoload);
    		});
        }
    } 
});
```

指令通过属性值的不同来调用不同控制器上的方法


link:function(scope,element,attrs)

scope作用域


element元素

attrs属性




![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-8fba9732b4eb025f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



###指令间的交互

scope: {},   创建独立作用域scope

这里面的controller和MVC的controller不是一个东西，这个是指令内部的controller，他是用来给我们的指令暴露出一组方法给外部去调用的。


supermanCtrl在link中

controller给指令暴露了三个方法





指令间的交互是通过指令上面内部的**controller**暴露出来的方法来给外部进行调用的，


```html
<!doctype html>
<html ng-app="MyModule">

<head>
    <meta charset="utf-8">
    <link rel="stylesheet" href="css/bootstrap-3.0.0/css/bootstrap.css">
    <script src="framework/angular-1.3.0.14/angular.js"></script>
    <script src="Directive&Directive.js"></script>
</head>

<body>
	<div class="row">
		<div class="col-md-3">
			<superman strength>动感超人---力量</superman>
		</div>
	</div>
	<div class="row">
		<div class="col-md-3">
			<superman strength speed>动感超人2---力量+敏捷</superman>
		</div>
	</div>
	<div class="row">
		<div class="col-md-3">
			<superman strength speed light>动感超人3---力量+敏捷+发光</superman>
		</div>
	</div>
</body>

</html>

```


```javascript
var myModule = angular.module("MyModule", []);
myModule.directive("superman", function() {
    return {
        scope: {},
        restrict: 'AE',
        controller: function($scope) {
            $scope.abilities = [];

            this.addStrength = function() {
                $scope.abilities.push("strength");
            };
            this.addSpeed = function() {
                $scope.abilities.push("speed");
            };
            this.addLight = function() {
                $scope.abilities.push("light");
            };
        },
        link: function(scope, element, attrs) {
            element.addClass('btn btn-primary');
            element.bind("mouseenter", function() {
                console.log(scope.abilities);
            });
        }
    }
});
myModule.directive("strength", function() {
    return {
        require: '^superman',
        link: function(scope, element, attrs, supermanCtrl) {
            supermanCtrl.addStrength();
        }
    }
});
myModule.directive("speed", function() {
    return {
        require: '^superman',
        link: function(scope, element, attrs, supermanCtrl) {
            supermanCtrl.addSpeed();
        }
    }
});
myModule.directive("light", function() {
    return {
        require: '^superman',
        link: function(scope, element, attrs, supermanCtrl) {
            supermanCtrl.addLight();
        }
    }
});
```



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-29ee8200b7aed295.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




###scope的类型与独立scope


**什么是独立scope**

```html
<!doctype html>
<html ng-app="MyModule">
	<head>
		<meta charset="utf-8">
		<link rel="stylesheet" href="css/bootstrap-3.0.0/css/bootstrap.css">
	</head>
	<body>
		<hello></hello>
		<hello></hello>
		<hello></hello>
		<hello></hello>
	</body>
	<script src="framework/angular-1.3.0.14/angular.js"></script>
	<script src="IsolateScope.js"></script>
</html>
```


页面上有四个hello指令


```javascript
var myModule = angular.module("MyModule", []);
myModule.directive("hello", function() {
    return {
        restrict: 'AE',
        template: '<div><input type="text" ng-model="userName"/>{{userName}}</div>',
        replace: true
    }
});
```


给这个指令上面加了input
用来接收用户的输入，然后后面加了双向数据绑用来展示输入的内容，


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-9d058b72b91d4094.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


页面上有四个input，因为这里没有给他创建独立scope会造成当我们其中一个任意input输入发生变化时，会影响其他三个指令实例。


这是有问题的，他们应该互相不受影响，就没有办法独立去使用了，

创建独立scope很简单只要加一个配置项，`scope:{}`


```html
<!doctype html>
<html ng-app="MyModule">
	<head>
		<meta charset="utf-8">
		<link rel="stylesheet" href="css/bootstrap-3.0.0/css/bootstrap.css">
	</head>
	<body>
		<hello></hello>
		<hello></hello>
		<hello></hello>
		<hello></hello>
	</body>
	<script src="framework/angular-1.3.0.14/angular.js"></script>
	<script src="IsolateScope.js"></script>
</html>
```





```javascript
var myModule = angular.module("MyModule", []);
myModule.directive("hello", function() {
    return {
        restrict: 'AE',
        scope:{},
        template: '<div><input type="text" ng-model="userName"/>{{userName}}</div>',
        replace: true
    }
});
```


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-3eee206b8ca4020a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



这个时候每个指令都有他独立的scope空间，这样他们就不会互相影响了。



###scope的绑定策略


**什么是绑定策略**



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-058d726680968696.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


```html
<!doctype html>
<html ng-app="MyModule">
	<head>
		<meta charset="utf-8">
		<link rel="stylesheet" href="css/bootstrap-3.0.0/css/bootstrap.css">
	</head>
	<body>
		<div ng-controller="MyCtrl">
			<drink flavor="{{ctrlFlavor}}"></drink>
		</div>
	</body>
	<script src="framework/angular-1.3.0.14/angular.js"></script>
	<script src="ScopeAt.js"></script>
</html>
```


首先指令drink 有一个叫flavor一个自定义的属性，使用{{}}绑定了一个ctrlFlavor属性


```javascript
var myModule = angular.module("MyModule", []);
myModule.controller('MyCtrl', ['$scope', function($scope){
	$scope.ctrlFlavor="百威";
}])
myModule.directive("drink", function() {
    return {
    	restrict:'AE',
        template:"<div>{{flavor}}</div>",
        link:function(scope,element,attrs){
        	scope.flavor=attrs.flavor;
        }
    }
});
```


定义控制器上面赋值一个属性ctrlFlavor值是百威

定义指令drink

template模板用来显示{{flavor}}内容

link函数里面把指令属性flavor的值赋给scope里flavor属性

实际上template模板用来显示{{flavor}}内容是来自于scope上面的flavor属性


```javascript
<div ng-controller="MyCtrl">
			<drink flavor="{{ctrlFlavor}}"></drink>
		</div>
```

由于在指令用的这个地方把flavor绑定到了控制器里面的ctrlFlavor上面所以指令里面显示的应该是百威。



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-6e4b26e6840fb3ac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




这样的写法如果每次都这样写会比较累，实际上可以通过`scope:{flavor:'@'}`这样的方式去写，link函数部分就可以不需要了。


```html
<!doctype html>
<html ng-app="MyModule">
	<head>
		<meta charset="utf-8">
		<link rel="stylesheet" href="css/bootstrap-3.0.0/css/bootstrap.css">
	</head>
	<body>
		<div ng-controller="MyCtrl">
			<drink flavor="{{ctrlFlavor}}"></drink>
		</div>
	</body>
	<script src="framework/angular-1.3.0.14/angular.js"></script>
	<script src="ScopeAt.js"></script>
</html>
```


```javascript
var myModule = angular.module("MyModule", []);
myModule.controller('MyCtrl', ['$scope', function($scope){
	$scope.ctrlFlavor="百威";
}])
myModule.directive("drink", function() {
    return {
    	restrict:'AE',
        scope: {
            flavor: '@'
        },
        template:"<div>{{flavor}}</div>",
        /*link:function(scope,element,attrs){
        	scope.flavor=attrs.flavor;
        }*/
    }
});
```


这样写和之前也是等价的。

这个是`@`绑定，有一个地方需要注意他传递的是字符串传递的不是对象


**scope  = 号的绑定**


这种绑定是双向进行绑定


业务是这样的dang不仅仅要把$scope上的ctrlFlavor传递给指令，当指令来修改了$scope上的ctrlFlavor的内容希望也让控制器里面的内容也发生变化，


```html
<!doctype html>
<html ng-app="MyModule">
	<head>
		<meta charset="utf-8">
		<link rel="stylesheet" href="css/bootstrap-3.0.0/css/bootstrap.css">
	</head>
	<body>
		<div ng-controller="MyCtrl">
			Ctrl:
			<br>
			<input type="text" ng-model="ctrlFlavor">
			<br>
			Directive:
			<br>
			<drink flavor="ctrlFlavor"></drink>
		</div>
	</body>
	<script src="framework/angular-1.3.0.14/angular.js"></script>
	<script src="ScopeEqual.js"></script>
</html>
```


```javascript
var myModule = angular.module("MyModule", []);
myModule.controller('MyCtrl', ['$scope', function($scope){
	$scope.ctrlFlavor="百威";
}])
myModule.directive("drink", function() {
    return {
    	restrict:'AE',
        scope:{
        	flavor:'='
        },
        template:'<input type="text" ng-model="flavor"/>'
    }
});
```


通过 scope:{flavor:'='},就把flavor的内容绑定到$scope.ctrlFlavor="百威";上面。


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-002b87786e15b653.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**scope  & 号的绑定**


```html
<!doctype html>
<html ng-app="MyModule">
	<head>
		<meta charset="utf-8">
		<link rel="stylesheet" href="css/bootstrap-3.0.0/css/bootstrap.css">
	</head>
	<body>
		<div ng-controller="MyCtrl">
			<greeting greet="sayHello(name)"></greeting>
			<greeting greet="sayHello(name)"></greeting>
			<greeting greet="sayHello(name)"></greeting>
		</div>
	</body>
	<script src="framework/angular-1.3.0.14/angular.js"></script>
	<script src="ScopeAnd.js"></script>
</html>
```


有greeting 指令，这个指令里面要调用控制器上面的方法叫sayHello



```javascript
var myModule = angular.module("MyModule", []);
myModule.controller('MyCtrl', ['$scope', function($scope){
	$scope.sayHello=function(name){
		alert("Hello "+name);
	}
}])
myModule.directive("greeting", function() {
    return {
    	restrict:'AE',
        scope:{
        	greet:'&'
        },
        template:'<input type="text" ng-model="userName" /><br/>'+
        		 '<button class="btn btn-default" ng-click="greet({name:userName})">Greeting</button><br/>'
    }
});
```


js中有一个控制器，作用域上有sayHello方法，弹参数出来。

写了greeting指令，写了scope:{greet:'&'},

在模板里面用一个按钮调用greet函数

这个函数在页面上绑定控制器上面的sayHello方法




![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-85a3d700eebb36dd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


###AngularJS内置的指令



http://ngnice.com/  AngularJS-api



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-0ed1c903dd25c550.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-ce0e5302ffc8b580.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




```html
<html ng-app='TestFormModule'>
	<head>
		<meta http-equiv="content-type" content="text/html; charset=utf-8" />
		<script src="framework/angular-1.3.0.14/angular.js"></script>
		<script src="FormBasic.js"></script>
	</head>
	<body>
		<form name="myForm" ng-submit="save()" ng-controller="TestFormModule">
			  <input name="userName" type="text" ng-model="user.userName" required/>
			  <input name="password" type="password" ng-model="user.password" required/>
			  <input type="submit" ng-disabled="myForm.$invalid"/>
		</form>
	</body>
</html>

```


```javascript
var appModule = angular.module('TestFormModule', []);
appModule.controller("TestFormModule",function($scope){
	$scope.user={
		userName:'damoqiongqiu',
		password:''
	};
	$scope.save=function(){
		alert("保存数据!");
	}
});
```



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-973ff96722645d86.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



```html
<!doctype html>
<html ng-app>
	<head>
		<script src="framework/angular-1.3.0.14/angular.js"></script>
		<script src="FormAdv1.js"></script>
	</head>
	<body>
		<div ng-controller="Controller">
			<form name="form" class="css-form" novalidate>
				Name:
				<input type="text" ng-model="user.name" name="uName" required /><br/>
				E-mail:
				<input type="email" ng-model="user.email" name="uEmail" required /><br/>
				<div ng-show="form.uEmail.$dirty && form.uEmail.$invalid">
					Invalid:
					<span ng-show="form.uEmail.$error.required">Tell us your email.</span>
					<span ng-show="form.uEmail.$error.email">This is not a valid email.</span>
				</div>
				Gender:<br/>
				<input type="radio" ng-model="user.gender" value="male" />
				male
				<input type="radio" ng-model="user.gender" value="female" />
				female<br/>
				<input type="checkbox" ng-model="user.agree" name="userAgree" required />
				I agree:
				<input ng-show="user.agree" type="text" ng-model="user.agreeSign" required />
				<div ng-show="!user.agree || !user.agreeSign">
					Please agree and sign.
				</div>
				<br/>
				<button ng-click="reset()" ng-disabled="isUnchanged(user)">
					RESET
				</button>
				<button ng-click="update(user)" ng-disabled="form.$invalid || isUnchanged(user)">
					SAVE
				</button>
			</form>
		</div>
	</body>
</html>

```


```javascript
function Controller($scope) {
	$scope.master = {};

	$scope.update = function(user) {
		$scope.master = angular.copy(user);
	};

	$scope.reset = function() {
		$scope.user = angular.copy($scope.master);
	};

	$scope.isUnchanged = function(user) {
		return angular.equals(user, $scope.master);
	};

	$scope.reset();
}
```


###实例解析Expander



```html
<html ng-app='expanderModule'>
	<head>
		<meta http-equiv="content-type" content="text/html; charset=utf-8" />
		<link rel="stylesheet" type="text/css" href="ExpanderSimple.css"/>
		<script src="framework/angular-1.3.0.14/angular.js"></script>
		<script src="ExpanderSimple.js"></script>
	</head>
	<body>
		<div ng-controller='SomeController'>
			<expander class='expander' expander-title='title'>
				{{text}}
			</expander>
		</div>
	</body>
</html>

```


定义一个控制器 ，写入expander指令，后面的expander-title='title'是使用ag绑定策略来进行绑定，


```javascript
var expanderModule=angular.module('expanderModule', []);
expanderModule.directive('expander', function() {
	return {
		restrict : 'EA',
		replace : true,
		transclude : true,
		scope : {
			title : '=expanderTitle'
		},
		template : '<div>'
				 + '<div class="title" ng-click="toggle()">{{title}}</div>'
				 + '<div class="body" ng-show="showMe" ng-transclude></div>'
				 + '</div>',
		link : function(scope, element, attrs) {
			scope.showMe = false;
			scope.toggle = function() {
				scope.showMe = !scope.showMe;
			}
		}
	}
});
expanderModule.controller('SomeController',function($scope) {
    $scope.title = '点击展开';
	$scope.text = '这里是内部的内容。';
});

```


首先directive定义一个指令expander

restrict : 'EA',  可以用元素也可以用属性，

replace : true,   会替换

transclude : true,  内部内容是可以变换的

```javascript
scope : {
			title : '=expanderTitle'
		},
//=  等于号这样一个等值绑定  绑定expanderTitle这样一个东西，
```

这个东西就是页面上写的

```html
<expander class='expander' expander-title='title'>
				{{text}}
			</expander>
```

template
```javascript
template : '<div>'
                 + '<div class="title" ng-click="toggle()">{{title}}</div>'
                 + '<div class="body" ng-show="showMe" ng-transclude></div>'
                 + '</div>',
```

template里面有一个ng-click绑定到`toggle()`这样一个方法上面，这个方法是有指令`link`这个函数里面提供出来的，很显然是指令内部去使用，外部是调取不到这个方法的，

```javascript
link : function(scope, element, attrs) {
			scope.showMe = false;
			scope.toggle = function() {
				scope.showMe = !scope.showMe;
			}
		}
```

看看`ng-show`

`ng-show`是用来进行显示和隐藏的切换的，他会根据`showMe `的属性是不是为`true`来决定这个`div`到底显不显示。


以上是一个简单指令的例子



![图片.png](http://upload-images.jianshu.io/upload_images/3877962-9e39a4fc357db349.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)











###Accordion

css

```css
.expander {
	border: 1px solid black;
	width: 250px;
}

.expander>.title {
	background-color: black;
	color: white;
	padding: .1em .3em;
	cursor: pointer;
}

.expander>.body {
	padding: .1em .3em;
}
```

html

```html
<html ng-app="expanderModule">
	<head>
		<meta http-equiv="content-type" content="text/html; charset=utf-8" />
		<link rel="stylesheet" type="text/css" href="Accordion.css"/>
		<script src="framework/angular-1.3.0.14/angular.js"></script>
		<script src="Accordion.js"></script>
	</head>
	<body ng-controller='SomeController' >
		<accordion>
			<expander class='expander' ng-repeat='expander in expanders' expander-title='expander.title'>
				{{expander.text}}
			</expander>
		</accordion>
	</body>
</html>
```

这里面指令进了嵌套，外面是个`accordion`，内部是`expander `这样一个指令，`expander `使用`ng-repeat='expander in expanders'`来进行迭代，他会根据`expanders`数组里面内容来决定创建多少个`expander `，




javascript

```javascript
var expModule=angular.module('expanderModule',[])
expModule.directive('accordion', function() {
	return {
		restrict : 'EA',
		replace : true,
		transclude : true,
		template : '<div ng-transclude></div>',
		controller : function() {
			var expanders = [];
			this.gotOpened = function(selectedExpander) {
				angular.forEach(expanders, function(expander) {
					if (selectedExpander != expander) {
						expander.showMe = false;
					}
				});
			}
			this.addExpander = function(expander) {
				expanders.push(expander);
			}
		}
	}
});

expModule.directive('expander', function() {
	return {
		restrict : 'EA',
		replace : true,
		transclude : true,
		require : '^?accordion',
		scope : {
			title : '=expanderTitle'
		},
		template : '<div>'
				  + '<div class="title" ng-click="toggle()">{{title}}</div>'
				  + '<div class="body" ng-show="showMe" ng-transclude></div>'
				  + '</div>',
		link : function(scope, element, attrs, accordionController) {
			scope.showMe = false;
			accordionController.addExpander(scope);
			scope.toggle = function toggle() {
				scope.showMe = !scope.showMe;
				accordionController.gotOpened(scope);
			}
		}
	}
});

expModule.controller("SomeController",function($scope) {
	$scope.expanders = [{
		title : 'Click me to expand',
		text : 'Hi there folks, I am the content that was hidden but is now shown.'
	}, {
		title : 'Click this',
		text : 'I am even better text than you have seen previously'
	}, {
		title : 'Test',
		text : 'test'
	}];
});

```
`expander`指令中`require : '^?accordion',`依赖外部的`accordion`，

在`link`里面就能接收到第四个参数`accordionController`

通过参数`accordionController`就可以跟外层的指令来进行一些交互，

`accordion`里面通过`controller `暴露了一些方法出来，

```javascript
controller : function() {
            var expanders = [];
            this.gotOpened = function(selectedExpander) {
                angular.forEach(expanders, function(expander) {
                    if (selectedExpander != expander) {
                        expander.showMe = false;
                    }
                });
            }
            this.addExpander = function(expander) {
                expanders.push(expander);
            }
        }
```


这样`expander`就能通过`accordionController`来调用









效果



![图片.png](http://upload-images.jianshu.io/upload_images/3877962-638cd88811aec914.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



第三方指令库，借助指令库加强开发速度


![图片.png](http://upload-images.jianshu.io/upload_images/3877962-e8904a53139d1d0d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


###指令的运行原理：compile和link




###总结：ERP类型的系统必备的UI组件


![图片.png](http://upload-images.jianshu.io/upload_images/3877962-bac8e6bd7b9951d0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


http://miniui.com/






###总结：互联网/电商型系统必备的UI组件



![图片.png](http://upload-images.jianshu.io/upload_images/3877962-186853cf895f76bf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




###第三方指令库angular-ui



###Directive思想的起源和原理概述
