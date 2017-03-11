---
layout: post
title: 基本概念和用法-路由 2-5
subtitle: 基本概念和用法-路由 2-5
author: 继小鹏
date: 2017-03-11 23:54:13 +0800
category: AngularJS
tag: AngularJS
---


###AJAX请求不会留下History历史记录


在2005年左右，兴起了叫ajax的技术，有了ajax像后台提交数据的时候就不在需要用form表单去提交了，用form表达去提交会导致页面之间的切换，也就是说没法实现单页应用，ajax也有些缺陷，他第一个问题就是不会在浏览器留下历史记录，在一些应用中不留下历史纪录是没有问题的，在一些网络型或许门户型应用没有历史记录用户没有办法保存书签，下次再来访问通过url来访问，发送给别人可能不是想要的状态，


ajax对SEO是个灾难，



```html
<!doctype html>
<html ng-app="bookStoreApp">

<head>
    <meta charset="UTF-8">
    <title>BookStore</title>
    <script src="framework/1.3.0.14/angular.js"></script>
    <script src="framework/1.3.0.14/angular-route.js"></script>
    <script src="framework/1.3.0.14/angular-animate.js"></script>
    <script src="js/app.js"></script>
    <script src="js/controllers.js"></script>
    <script src="js/filters.js"></script>
    <script src="js/services.js"></script>
    <script src="js/directives.js"></script>
</head>

<body>
    <div ng-view>
    </div>
</body>

</html>

```



```javascript
var bookStoreApp = angular.module('bookStoreApp', [
    'ngRoute', 'ngAnimate', 'bookStoreCtrls', 'bookStoreFilters',
    'bookStoreServices', 'bookStoreDirectives'
]);

bookStoreApp.config(function($routeProvider) {
    $routeProvider.when('/hello', {
        templateUrl: 'tpls/hello.html',
        controller: 'HelloCtrl'
    }).when('/list',{
    	templateUrl:'tpls/bookList.html',
    	controller:'BookListCtrl'
    }).otherwise({
        redirectTo: '/hello'
    })
});
```


定义了一个模块，叫做bookStoreApp这个app调用config方法，在方法匿名函数的参数中有一个`$routeProvider`这个东西是AngularJS自身所提供的路由机制，通过 `$routeProvider`这个东西可以来进行路由的配置，当AngularJS发现浏览器地址栏是hello，就会使用`tpls/hello.html`这样一个模板，由  `HelloCtrl`控制器来处理模板和 数据之间的绑定。当他url是list的时候就会调用另一个模板和控制器。其他别的所有清况都会直接跳到hello，


`$routeProvider`是AngularJS自身提供的机制，需要注意的事AngularJS1.2以后，他把这些机制之间做了一个模块化的处理，也就是说`route`没有包含在`angular.js`里面，也就是说不在他的核心文件里面，是把他独立出来，成了一个模块，


所以需要注意的是如果需要使用路由的时候一定要在页面导入这个js


`<script src="framework/1.3.0.14/angular-route.js"></script>`



这个路由机制也有毛病，他不能进行深层次嵌套路由的。


看个例子，这是常见页面的布局方式


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-36f8444ac02d4ad4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


顶部放导航条，这里面是一级模块的菜单，下面是具体的内容，点击首页的时候是一个比较大的展现，当点击用户管理的时候，



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-af6cb82449d86bf6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-0680ef1d67262e64.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


他会切分成左右两块区域，


当点击左边的链接的时候右边的内容会相应发生变化，



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-0d9183928b953bbc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-d2f896e9bda93d79.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这是一种非常常见的布局方式，用AngularJS本身提供的路由来实现这样的效果是比较麻烦的，有第三方的人开发了一个`UIRoute`这样一个东西，



在github上去收搜https://angular-ui.github.io/


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-745c6ffb453263d6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


`UIRoute`提供了一套很好的机制，实现深层次的嵌套，




```html
<!doctype html>
<html ng-app="routerApp">

<head>
    <meta charset="utf-8">
    <link rel="stylesheet" href="css/bootstrap-3.0.0/css/bootstrap.css">
    <link rel="stylesheet" href="css/index.css">
    <script src="js/angular-1.3.0.js"></script>
    <script src="js/angular-animate.js"></script>
    <script src="js/angular-ui-router.js"></script>
    <script src="UIRoute3.js"></script>
</head>

<body>
    <div ui-view></div>
</body>

</html>

```


首先需要把` <script src="js/angular-ui-router.js"></script>`下载下来，导入到页面里面去，导入了`ui-router`以后就不需要AngularJS原生的`router`。


在写法上也会发生些变化


在页面上


```html
<!doctype html>


<body>
    <div ui-view></div>
</body>



```


使用`ui-view`这个指令，这个表示是一个视图，



来看js代码


```javascript
var routerApp = angular.module('routerApp', ['ui.router']);
routerApp.config(function($stateProvider, $urlRouterProvider) {
    $urlRouterProvider.otherwise('/index');
    $stateProvider
        .state('index', {
            url: '/index',
            views: {
                '': {
                    templateUrl: 'tpls3/index.html'
                },
                'topbar@index': {
                    templateUrl: 'tpls3/topbar.html'
                },
                'main@index': {
                    templateUrl: 'tpls3/home.html'
                }
            }
        })
        .state('index.usermng', {
            url: '/usermng',
            views: {
                'main@index': {
                    templateUrl: 'tpls3/usermng.html',
                    controller: function($scope, $state) {
                        $scope.addUserType = function() {
                            $state.go("index.usermng.addusertype");
                        }
                    }
                }
            }
        })
        .state('index.usermng.highendusers', {
            url: '/highendusers',
            templateUrl: 'tpls3/highendusers.html'
        })
        .state('index.usermng.normalusers', {
            url: '/normalusers',
            templateUrl: 'tpls3/normalusers.html'
        })
        .state('index.usermng.lowusers', {
            url: '/lowusers',
            templateUrl: 'tpls3/lowusers.html'
        })
        .state('index.usermng.addusertype', {
            url: '/addusertype',
            templateUrl: 'tpls3/addusertypeform.html',
            controller: function($scope, $state) {
                $scope.backToPrevious = function() {
                    window.history.back();
                }
            }
        })
        .state('index.permission', {
            url: '/permission',
            views: {
                'main@index': {
                    template: '这里是权限管理'
                }
            }
        })
        .state('index.report', {
            url: '/report',
            views: {
                'main@index': {
                    template: '这里是报表管理'
                }
            }
        })
        .state('index.settings', {
            url: '/settings',
            views: {
                'main@index': {
                    template: '这里是系统设置'
                }
            }
        })
});

```



和之前AngularJS的route是大同小异的，


只是说不再去使用`$routeProvider`了


换成`$stateProvider, $urlRouterProvider`



和之前AngularJS的route`$routeProvider`写法非常相似，但是他定义的方法名叫`state`


首先调用`$stateProvider`上面的`state`方法，来配置当地址栏里面发生什么样的变化的时候，就去使用哪一个模板，这里有很多比较快捷的语法，比如说`index`页面实现逻辑


```javascript
.state('index', {
            url: '/index',
            views: {
                '': {
                    templateUrl: 'tpls3/index.html'
                },
                'topbar@index': {
                    templateUrl: 'tpls3/topbar.html'
                },
                'main@index': {
                    templateUrl: 'tpls3/home.html'
                }
            }
        })
```


上面代码，state状态是index时候就去执行，url参数是/index，找index代码片段，

定义了几个`views`其实有三个view，我们把页面分成了两个部分顶部是一个导航条，然后下面的内容是会跟着去切换的，


```javascript
                '': {
                    templateUrl: 'tpls3/index.html'
                },
```

顶部写一个空的字符串，然后用tpls3下面的index.html来作为我们主页的html模板，


index.html文件


```html
<div class="container">
    <div ui-view="topbar"></div>
    <div ui-view="main"></div>
</div>

```

在这个html文件里面，又把路由分成了两块，一个topbar一个main，这是主区域，


```javascript
.state('index', {
            url: '/index',
            views: {
                '': {
                    templateUrl: 'tpls3/index.html'
                },
                'topbar@index': {
                    templateUrl: 'tpls3/topbar.html'
                },
                'main@index': {
                    templateUrl: 'tpls3/home.html'
                }
            }
        })
```

` 'topbar@index': `,`'main@index': `这两个的意思就是index模块中分的两个ui-view，还给他命名了叫做topbar，main

通过@这种语法就会自动去知道每一个小的小块去加载对应的模板，


其他的切分方式，和上面都大同小异，当然他还有一些比较方便的语法，比如说用点号`.`来分隔这种子模块，子区域，有了ui-router就可以做深层次路由的嵌套，包括一个页面上面分成多个区域，多个区域都可以定义匿名的ui-router，这样可以只切换一小块区域，而不是整个都去切换，



###前端路由的基本原理


###哈希#


以前通过a标签有一个叫锚点的机制，锚点是点一个链接他不会页面与页面之间进行跳转，他会在页内导航，后来就有人发现我们可以通过这样一个机制让浏览器不刷新页面而实现url地址的变化，url地址变了以后浏览器不会去刷洗页面，通过锚点这种方式进行了封装，这是比较老的浏览器都可以支持的。



###html5中新的history API


可以通过js代码去修改浏览器地址栏地址，这样浏览器会留下历史纪录，但是页面不会跳转，这是新的html5提供的。一般来说通过这种小的工具库都会封装这种方式从而实现向下兼容，


###路由的核心是给应用定义状态




###使用路由机制会影响整个应用编码的方式。


这种概念和传统开发的方式会有一些思路上的不同，也就是说你用路由的时候实际上你一定要为你的应用先把这些状态都定义好，通过哪一个地址进入哪一种状态都预先定义好，会影响整个应用编码的方式。


###考虑兼容性问题与“优雅降级”

一般比较完善的路由库，都会提供兼容性的处理，会帮你去检测浏览器的版本如果发现使用比较低版本浏览器ie这些，会自动帮你使用哈希这种方式，如果发下是新的浏览器就会使用html5  history API去操作。
