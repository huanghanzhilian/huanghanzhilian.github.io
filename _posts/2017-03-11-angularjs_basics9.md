---
layout: post
title: 基本概念和用法-Service与Peovider  2-7
subtitle: 基本概念和用法-Service与Peovider  2-7
author: 继小鹏
date: 2017-03-11 23:56:13 +0800
category: AngularJS
tag: AngularJS
---



###使用$http服务


```html
<html ng-app="MyModule">
	<head>
		<meta http-equiv="content-type" content="text/html; charset=utf-8" />
		<script src="framework/angular-1.3.0.14/angular.js"></script>
		<script src="HTTPBasic.js"></script>
	</head>
	<body>
		<div ng-controller="LoadDataCtrl">
			<ul>
				<li ng-repeat="user in users">
					{{user.name}}
				</li>
			</ul>
		</div>
	</body>
</html>
```


有ng-controller叫做LoadDataCtrl，

这个ng-controller有个列表`ng-repeat`将数组循环出来。


`ng-repeat="user in users"`这里的`users`数据不再是像前面写列子一样写死在代码里面，希望通过后台去加载进来，




```javascript
var myModule=angular.module("MyModule",[]);
myModule.controller('LoadDataCtrl', ['$scope','$http', function($scope,$http){
	$http({
        method: 'GET',
        url: 'data.json'
    }).success(function(data, status, headers, config) {
        console.log("success...");
        console.log(data);
        $scope.users=data;
    }).error(function(data, status, headers, config) {
        console.log("error...");
    });
}]);
```


调用AngularJS里面的`$http`这个服务，

`method: 'GET',`  数据交互方式`GET或POST`

`url: 'data.json'`要请求哪个地址。

`success`成功后执行函数

`error`请求错误执行函数


这里请求的是`json`文件


```json
[{
    "name": "《用AngularJS开发下一代WEB应用》"
},{
    "name": "《Ext江湖》"
},{
    "name": "《ActionScript3.0游戏设计基础（第二版）》"
}]

```


拿到这些数据以后把他`$scope.users`上面去


`$scope.users=data;`









###创建自己的Service

```html
<html ng-app="MyServiceApp">

<head>
    <meta http-equiv="content-type" content="text/html; charset=utf-8" />
    <link rel="stylesheet" href="framework/bootstrap-3.0.0/css/bootstrap.css">
    <script src="framework/angular-1.3.0.14/angular.js"></script>
    <script src="MyService1.js"></script>
</head>

<body>
    <div ng-controller="ServiceController">
        <label>用户名</label>
        <input type="text" ng-model="username" placeholder="请输入用户名" />
        <pre ng-show="username">{{users}}</pre>
    </div>
</body>

</html>

```

有一个`inout`他的`ng-model`等于`username`

要实现的效果是当输入项内容发生变化的时候就去向后台发起请求，


```javascript
var myServiceApp = angular.module("MyServiceApp", []);
myServiceApp.factory('userListService', ['$http',
    function($http) {
        var doRequest = function(username, path) {
            return $http({
                method: 'GET',
                url: 'users.json'
            });
        }
        return {
            userList: function(username) {
                return doRequest(username, 'userList');
            }
        };
    }
]);

myServiceApp.controller('ServiceController', ['$scope', '$timeout', 'userListService',
    function($scope, $timeout, userListService) {
        var timeout;
        $scope.$watch('username', function(newUserName) {
            if (newUserName) {
                if (timeout) {
                    $timeout.cancel(timeout);
                }
                timeout = $timeout(function() {
                    userListService.userList(newUserName)
                        .success(function(data, status) {
                            $scope.users = data;
                        });
                }, 350);
            }
        });
    }
]);

//...
```

AngularJS封装了叫`$watch`用来监控一个数据模型的变化，这里利用`$watch`来监听，

监听`username`他发生变化以后执行一个匿名函数，这个函数里面来检测是不是拿到新的`newUserName`如果拿到新的值就向后台去发送请求，

那么调用谁去发请求呢？就调用自己封装的`userListService`

这个地方有一个比较绕的东西叫做`'$timeout'`

这个`'$timeout'`就是说当我们在页面上进行输入的时候不是说我们每次按下减排他就去请求后台，如果这样的话会发现页面会抖动，假如说每按下一个键他就去向后台发起请求，很显然页面会不断地狂刷，

这个时候加一个防止抖动的处理，这是比较常见的动作，只有当你`350`毫秒不再按下一个按键的时候，就是说延迟350毫秒没有按下，这个时候他才会去向后台发起请求。

当你连续按键的时候，并不会向后台连续发送请求，



最后调用的函数是`userListService.userList`这个函数，

自己定义的`Service`和AngularJS内置的`Service`有两点不同点，

一种是我们自己定义的`Service`他的命名不要用`$`打头，

第二我们自己定义的`Service`也是可以向AngularJS内置的服务一样去进行注入的，但是注入的时候有一个不同的地方，就是说我们自己定义的`Service`是必须放在最后一个的，

有了这个`Service`以后，假设要做相同的操作就可以去一直去调用它，很多的`controller`控制器都可以去共用的，从而实现了这个功能的复用，



比如说我要把`userListService`抽出作为自己的服务，这个Servuce里面会去返回`userList`用户列表数据。

把它抽成一个服务之后，其他的控制器就可以调用它，


项目中控制器会有很多，如果有控制器之间有代码相同，那么就可以抽到Servuce服务里面，方便调用。



![图片.png](http://upload-images.jianshu.io/upload_images/3877962-fffa1f271901c615.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)







###Service的特性




![图片.png](http://upload-images.jianshu.io/upload_images/3877962-bdf22fc399e21f2c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



###Service，Factory，Porvider，本质上都是Porvider


![图片.png](http://upload-images.jianshu.io/upload_images/3877962-79e457e466cc4831.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



###使用$filter



![图片.png](http://upload-images.jianshu.io/upload_images/3877962-8ca8222843d1b55f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



###其他内置Service介绍


![图片.png](http://upload-images.jianshu.io/upload_images/3877962-af691fa30e840a78.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)