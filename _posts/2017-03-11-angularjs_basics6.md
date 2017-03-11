---
layout: post
title: 基本概念和用法-双向数据绑定 2-4
subtitle: 基本概念和用法-双向数据绑定 2-4
author: 继小鹏
date: 2017-03-11 23:52:13 +0800
category: AngularJS
tag: AngularJS
---


双向数据绑定指的是两个方向，这两个方向指的是从视图到数据模型，AngularJS是MVC框架，程序主要是通过控制器进行操作的，控制器去修改数据模型，数据模型的变更会反应到视图上。视图上面如果发生了数据变化AngularJS框架会同步到数据模型中。


###简单例子


```html
<!doctype html>
<html ng-app>
    <head>
        <meta charset="utf-8">
    </head>
    <body>
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


html页面中p标签的双花口号是一个取值表达式，


这个页面有一个问题，如果刷新页面在网络不好的情况下可能会看到`{{greeting.text}}`











###取值表达式与ng-bind指令

AngularJS提供了一个叫ng-bind指令来避免上面出现取值表达式的问题，


```html
<!doctype html>
<html ng-app>
    <head>
        <meta charset="utf-8">
    </head>
    <body>
        <div ng-controller="HelloAngular">
            <p><span ng-bind="greeting.text"></span>,Angular</p>
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

使用ng-bind指令不会出现取值表达式的问题


在`{{}}`取值表达式和`ng-bind`间如何取舍，什么时候该使用哪个呢？


AngularJS本身的库在记载完以后整个页面就归AngularJS来管了这个时候使用`{{}}`绑定的方式呢就不会有很大的问题。


也就是说在应用主程序index中如果有数据绑定，使用`ng-bind`去绑定，一般在导入ng核心库的时候一般都是放在index上面，一般不会在内层去加载ng核心库，所以后续的页面通过模板加载进来的页面使用`{{}}`绑定就ok了。这样是不会堪当表达式的。



在什么情况下会需要视图会修改值，然后导致去改变数据模型呢？

典型的场景就是form表单，form是让用户可以交互和输入的。



###双向绑定的经典场景


```html
<!doctype html>
<html ng-app="UserInfoModule">

<head>
    <meta charset="utf-8">
    <link rel="stylesheet" href="css/bootstrap-3.0.0/css/bootstrap.css">
    <script src="js/angular-1.3.0.js"></script>
    <script src="Form.js"></script>
</head>

<body>
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
</body>

</html>

```


```javascript
var userInfoModule = angular.module('UserInfoModule', []);
userInfoModule.controller('UserInfoCtrl', ['$scope',
    function($scope) {
        $scope.userInfo = {
            email: "1319639755@qq.com",
            password: "253445528",
            autoLogin: true
        };
        $scope.getFormData = function() {
            console.log($scope.userInfo);
        };
        $scope.setFormData = function() {
            $scope.userInfo = {
                email: '2254513188@qq.com',
                password: 'damoqiongqiu',
                autoLogin: false
            }
        };
        $scope.resetForm = function() {
            $scope.userInfo = {
                email: "1319639755@qq.com",
                password: "253445528",
                autoLogin: true
            };
        }
    }
])

```


预览




以上代码创建模块，这个模块不以来任何东西，所以数组是空，

有了模块需要建立一个控制器由控制器来控制数据模型和视图，UserInfoCtrl，这个控制器放在了form上。


在$scope上赋一个对象userInfo，定义好登入需要的一些信息，

然后回到html绑定值，使用`ng-model`绑定值。


这样这些输入项就和数据模型绑定起来了，


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-82bd9a11cb4bf7a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


在$scope上建立getFormData方法，打印userInfo的数据


如何调用$scope上的方法呢？在页面按钮标签使用ng-click="getFormData()"指令，就可以去执行$scope上的方法。

点击获取数据按钮，就会打印userInfo的数据



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-abf7291aa79db6c7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



如果我修改了input上面的值会不会同步到数据模型中去，也就是$scope.userInfo对象上面去。



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-f52a5f1453cc0b03.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


答案是数据修改了，也就是说视图上的修改会自动同步到数据模型上面去，



```javascript
        $scope.setFormData = function() {
            $scope.userInfo = {
                email: '2254513188@qq.com',
                password: 'damoqiongqiu',
                autoLogin: false
            }
        };
```

也可以通过程序修改数据模型，视图同步改变。



```javascript
        $scope.resetForm = function() {
            $scope.userInfo = {
                email: "1319639755@qq.com",
                password: "253445528",
                autoLogin: true
            };
        }
```

重置表单






###动态切换标签样式


双向数据绑定还可以用在修改样式上面，


```css
.text-red {
    background-color: #ff0000;
}
.text-green {
    background-color: #00ff00;
}

```


```html
<!doctype html>
<html ng-app="MyCSSModule">

<head>
    <meta charset="utf-8">
    <link rel="stylesheet" href="CSS1.css">
</head>

<body>
    <div ng-controller="CSSCtrl">
        <p class="text-{{color}}">测试CSS样式</p>
        <button class="btn btn-default" ng-click="setGreen()">绿色</button>
    </div>
</body>
<script src="js/angular-1.3.0.js"></script>
<script src="CSS1.js"></script>

</html>

```


```javascript
var myCSSModule = angular.module('MyCSSModule', []);
myCSSModule.controller('CSSCtrl', ['$scope',
    function($scope) {
        $scope.color = "red";
        $scope.setGreen = function() {
            $scope.color = "green";
        }
    }
])

```



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-b87f5c300e937b90.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-7d121bcbb2d33299.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


###ng-class


这种方式有一个问题，真实项目中标签结构很复杂，如果{{}}里面的值取错了，或者说没有值，取成了null，就会发生一些比较诡异的情况。


为了避免这种状况AngularJS提供了`ng-class`指令，

`ng-class`和原生的class是有去别的，它支持是可以接收一些表达式的  。


```css
.error {
    background-color: red;
}
.warning {
    background-color: yellow;
}


```


```html
<!doctype html>
<html ng-app="MyCSSModule">

<head>
    <meta charset="utf-8">
    <link rel="stylesheet" href="NgClass.css">
</head>

<body>
    <div ng-controller='HeaderController'>
        <div ng-class='{error: isError, warning: isWarning}'>{{messageText}}</div>
        <button ng-click='showError()'>Simulate Error</button>
        <button ng-click='showWarning()'>Simulate Warning</button>
    </div>
</body>
<script src="js/angular-1.3.0.js"></script>
<script src="NgClass.js"></script>

</html>

```


```javascript
var myCSSModule = angular.module('MyCSSModule', []);
myCSSModule.controller('HeaderController', ['$scope',
    function($scope) {
        $scope.isError = false;
        $scope.isWarning = false;
        $scope.showError = function() {
            $scope.messageText = 'This is an error!';
            $scope.isError = true;
            $scope.isWarning = false;
        };
        $scope.showWarning = function() {
            $scope.messageText = 'Just a warning. Please carry on.';
            $scope.isWarning = true;
            $scope.isError = false;
        };
    }
])


```


`<div ng-class='{error: isError, warning: isWarning}'>{{messageText}}</div>`

这行代码使用了ng-class，他是一个表达式，如果isError的值是true他就会用error这个样式，如果isWarning的值是true他就会用warning这个样式，



```javascript
var myCSSModule = angular.module('MyCSSModule', []);
myCSSModule.controller('HeaderController', ['$scope',
    function($scope) {
        $scope.isError = false;
        $scope.isWarning = false;
        $scope.showError = function() {
            $scope.messageText = 'This is an error!';
            $scope.isError = true;
            $scope.isWarning = false;
        };
        $scope.showWarning = function() {
            $scope.messageText = 'Just a warning. Please carry on.';
            $scope.isWarning = true;
            $scope.isError = false;
        };
    }
])


```

这段js代码中一开始是两个值都是false，底下两个按钮分别执行两个不同的方法，



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-db03e31ad10d53a3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-42fe859ab7c03db4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




###ng-show和ng-hide


控制一个标签的显示和隐藏，

```html
<!doctype html>
<html ng-app="MyCSSModule">

<head>
    <meta charset="utf-8">
</head>

<body>
    <div ng-controller='DeathrayMenuController'>
        <button ng-click='toggleMenu()'>Toggle Menu</button>
        <ul ng-show='menuState.show'>
            <li ng-click='stun()'>Stun</li>
            <li ng-click='disintegrate()'>Disintegrate</li>
            <li ng-click='erase()'>Erase from history</li>
        </ul>
    <div/>
</body>

<script src="js/angular-1.3.0.js"></script>
<script src="NgShow.js"></script>

</html>


```


```javascript
var myCSSModule = angular.module('MyCSSModule', []);
myCSSModule.controller('DeathrayMenuController', ['$scope',
    function($scope) {
        $scope.menuState={show:false};
        $scope.toggleMenu = function() {
            $scope.menuState.show = !$scope.menuState.show;
        };
    }
])

```


这个例子是点击按钮切换列表项的显示状态，


`<ul ng-show='menuState.show'>` menuState.show这个模型的值


按钮点击运行toggleMenu方法


```javascript

        $scope.toggleMenu = function() {
            $scope.menuState.show = !$scope.menuState.show;
        };
```
每次运行都会区一个和之前相反的值，也就是区非。



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-2a4e3ac74b9035cc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-0659ed51cec68907.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这是一种开关效果。


**ng-hide是和ng-show是想反的动作**





###ngAnimate