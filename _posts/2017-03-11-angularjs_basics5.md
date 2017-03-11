---
layout: post
title: 基本概念和用法-路由，模块，依赖注入 2-3
subtitle: 基本概念和用法-路由，模块，依赖注入 2-3
author: 继小鹏
date: 2017-03-11 23:50:13 +0800
category: AngularJS
tag: AngularJS
---


###AngularJS的模块化实现



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

上面代码有一个p标签里面有一个双花括号的取值表达式，

js代码是一个很简单的函数这个函数里面有一个$scope的参数，在他上面去赋值，AngularJS就可以取到对应的值。

其实上面的代码是不够模块化的，因为写了一个全局的函数，这不符合模块化，会污染全局空间，可以这样写。


```html
<!doctype html>
<html ng-app="HelloAngular">
    <head>
        <meta charset="utf-8">
    </head>
    <body>
        <div ng-controller="helloNgCtrl">
            <p>{{greeting.text}},Angular</p>
        </div>
    </body>
    <script src="js/angular-1.3.0.js"></script>
    <script src="NgModule1.js"></script>
</html>

```


```javascript
var helloModule=angular.module('HelloAngular', []);
helloModule.controller('helloNgCtrl', ['$scope', function($scope){
	$scope.greeting = {
        text: 'Hello'
    };
}]);

```


上面的js利用angular的module方法去定义一个模块，定义一个名为HelloAngular的模块，定义完之后在模块的实例上面去调用controller方法，来创建一个名为helloNgCtrl的控制器，后面是一个方括号，是一个数组，第一个参数是一个字符串'$scope'，第二个参数是一个函数，这个函数实际上和之前写的HelloAngular函数内容是一模一样的，只不过这个函数是一个匿名函数，以上代码就是AngularJS实现模块化的一个方法。





###一个完整的项目结构是什么样子的？---实例演示

使用模块化有什么好处呢？AngularJS里面的模块又是什么东西呢，更具官方的定义，AngularJS里面的模块实际上他是一个集合，就相当于一个框子，他是由模型，视图，控制器，过滤器，服务，等等。。。把他组合到一起实现某一个功能那么他就叫一个模块。


有了AngularJS模块化工具之后一个真实的项目他的目录结构是什么样子的。



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-61b011c81442379d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


假设有一个叫BookStore这样一个应用app，


先来看一级的结构


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-bf48adc3d68a87c5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


package.json其实是给mpm，node.js工具去用的，如果不想用是可以不要的，



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-75010b346a5c9c5b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


node_modules这个东西是我们用npm也就是说node的模块管理器，去装一些插件的时候他自动生成的，所有的插件都会装入这个目录下面，一般来说在开发一个项目的时候会把nodejs下面的工具都直接安装在项目的目录下面同时会把这东西直接提交到版本控制工具中，这样做是为了大家版本一直，避免版本冲突。


其他代码都房子app目录下面


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-544272cfa01c20c7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这个目录下面分成几个小目录，


**framework**目录    这个目录下面我们不止放AngularJS，可能还有Bootstrap获取还有其他的ui控件，都可以放在这个下面，便于管理和组织，


**tpls**目录用来放模板，就是是说一些html片段，把他抽成一个小片段，什么叫html片段呢？实际上就是一段不完整的html的结构，他是没有html根标签，也没有body和其他的东西，就是其中的一小块，


```html
<ul>
    <li ng-repeat="book in books">
        书名:{{book.title}}   作者:{{book.author}}
    </li>
</ul>

```


实际上就像晓得零件这个片段会被我们的控制器加载进来，然后控制器把数据传递给他，形成一些真实的html标签，就会插入到问道流中去。


一个应为会有一个主文件，叫做index，

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


这个index来加载一些js文件，

放一个根的视图叫做**ng-view**

```html
<body>
    <div ng-view>
    </div>
</body>
```


也就是说充当视图最根的一个容器，通过这个根容器可以实现跟多的功能，


所有的js代码是放在js目录下，



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-6379adcd581c1eaf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


一般来说这些js名称是预定熟成的，


app.js 作为启动点的js


controllers.js  说明这个文件方的是多个控制器，而不是一个，


directives.js 指令js


filters.js  过滤器 


services.js  服务


还可以有route路由获取其他































###使用ng-Route进行视图之间的路由


通过不同的url来访问不同的viel



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-1739525f4c619432.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-6f6f8e2b302980ea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-8e01f7564aaad999.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


通过路由$routeProvider来实现不同url访问不同的viel,一个应用里面有很多的视图，那么怎么判断哪个url对应哪个视图呢？AngularJS提供了叫$routeProvider，从字面意义上知道这是路由，他会根据访问的路径不同展示不同的viel，这是$routeProvider基本的功能，




![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-42a29fe2f9b66aad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当我们访问hello的时候他就会就是使用templateUrl模板url去找到模板片段tpls下面的hello.html片段，作为他的模板controller: 'HelloCtrl'意识是使用HelloCtrl控制器去处理整个视图的生成，

Angular路由的意思就是根据地址栏里面的url不同他去帮我们展现不同的视图，这个视图是由控制器去负责生成出来的，

$routeProvider有两个方法一个是when一个是otherwise，otherwise是不是when定义的情况下发生。


会发现AngularJS路由生成页面会有个#/list，有一个#号，这个#号是为了防止浏览器像后台提交请求，使用#号实际上就是内部的锚点，这样的话浏览器不会向服务端发请求。这样的写法其实是告诉浏览器在页面内进行跳转，AngularJS会拦截到url地址，他会去把#号后面的内容取出来，取出来后来跟$routeProvider里面所写的内容进行匹配，这样他就知道去展现哪个视图。


有了这个东西就可以把不同的视图交给不同的控制器去处理这样就能把视图之间的职能分的很清楚，可以用很多的控制器来处理不同的视图不同的内容。



###一切都是从模块开始的



再来看看**controllers.js**


```javascript
var bookStoreCtrls = angular.module('bookStoreCtrls', []);

bookStoreCtrls.controller('HelloCtrl', ['$scope',
    function($scope) {
        $scope.greeting = {
            text: 'Hello'
        };
    }
]);

bookStoreCtrls.controller('BookListCtrl', ['$scope',
    function($scope) {
        $scope.books =[
        	{title:"《Ext江湖》",author:"大漠穷秋"},
        	{title:"《ActionScript游戏设计基础（第二版）》",author:"大漠穷秋"},
        	{title:"《用AngularJS开发下一代WEB应用》",author:"大漠穷秋"}
        ]
    }
]);

/**
 * 这里接着往下写，如果控制器的数量非常多，需要分给多个开发者，可以借助于grunt来合并代码
 */
```

先来看hello

首先要定义一个模块

`var bookStoreCtrls = angular.module('bookStoreCtrls', []);`


bookStoreCtrls 模块有一个HelloCtrl名称的controller，也就是HelloCtrl名称的控制器，

打开根目录浏览器预览如果什么都不输会自动跳入app/index.html#/hello

这是因为


```javascript
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

因为otherwise意思就是说如果不满足上面所有的情况，就会跳到 redirectTo: '/hello'这样一个路径上去，跳到这个路径上就会归controller: 'HelloCtrl'来管理了，

controller: 'HelloCtrl'它什么都没做，只是往$scope.greeting上面赋了一个对象

templateUrl: 'tpls/hello.html',使用模板的路径加载一个hello.html片段

```html
<p>{{greeting.text}},Angular</p>
```


模板加载进来之后就会获得控制器上所赋的对象的值，获得值以后会拼接成一个完整的html然后把他插入到index里面，


```html
<body>
    <div ng-view>
    </div>
</body>
```

插入到index里面写的ng-view里面去，显示结果。



再来看BookListCtrl控制器


```javascript
bookStoreCtrls.controller('BookListCtrl', ['$scope',
    function($scope) {
        $scope.books =[
        	{title:"《Ext江湖》",author:"大漠穷秋"},
        	{title:"《ActionScript游戏设计基础（第二版）》",author:"大漠穷秋"},
        	{title:"《用AngularJS开发下一代WEB应用》",author:"大漠穷秋"}
        ]
    }
]);

```

这个控制器在$scope.books上赋值了一个数组这个数组包含这对象，


```javascript
.when('/list',{
        templateUrl:'tpls/bookList.html',
        controller:'BookListCtrl'
    }).
```

在路由中的`/list`使用`'tpls/bookList.html'`来作为模板

```html
<ul>
    <li ng-repeat="book in books">
        书名:{{book.title}}   作者:{{book.author}}
    </li>
</ul>
```

这个模板使用` ng-repeat`迭代数组的指令来迭代books数组，从而生成列表，

当访问`list`的时候也就说明`BookListCtrl`控制器去管理了，从而成了列表结构现在在视图



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-b4cbab482dfd5654.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


如果应用里面还有其他的控制器，可以继续在`controllers.js`里继续往下写，


###ng官方推荐的模块切分方式是什么？


在项目开发时，可能有很多人来共同完成这个项目，这个时候不可能大家一起来改一个文件，改一个文件很容易冲突，这个时候有两种方式，

一种方式是使用代码合并和混淆工具grunt工具让大家开发完成以后自动合并成一个文件，意思是都合并到`controllers.js`文件中来。


另一种方式是可以定义多个模块，可以定义多个模块，





多个模块如何实现加载呢？



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-d8582da9c1253cb8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


```javascript
var bookStoreApp = angular.module('bookStoreApp', [
    'ngRoute', 'ngAnimate', 'bookStoreCtrls', 'bookStoreFilters',
    'bookStoreServices', 'bookStoreDirectives'
]);
```

以上代码使用了官方推荐的模块切分的方式，也就是说把控制器，指令，过滤器，服务等。。。分别定义成一个或者多个js文件，这个时候提供一个入口点`app.js`，这个`app.js`定义一个模块module，这个模块module作为启动点，他实际上没有太多的功能，只是说告诉AngularJS我依赖哪一些模块，然后加一个路由的配置的功能


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

这种方式是AngularJS官方所推荐的方式。



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-12a3aa683a7570f9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


AngularJS官方所推荐的方式就是说我用一个总的`app`的模块作为总的入口点，这个模块他依赖于`controllers`,`directives`,`filters`,`services`,`routes`



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-5985ae08587319be.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上图可见`bookStoreApp`模块是作为项目的启动点的




![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-57e6d4510cf10dc0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


可以在index里面看到ng-app作为启动点

ng-app这个指令的时候就知道我从ng-app这个指令开始内部的标签内容就归我Angular js来管了，也就是说Angular js从ng-app这个地方启动的，可以想象既然是main函数main方法，一个应用里面显然只能有一个，所以在任意一个单页Angular js应用里面ng-app这个指令只能出现一次。


AngularJS在加载完以后会来找ng-app这个指令，一个单页Angular js应用里面ng-app这个指令只能出现一次。他是一个main方法。

AngularJS找到了这个指令以后呢就会尝试执行这个启动点的模块`bookStoreApp`。


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-1d6094a93e3b1639.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


然后他就会发现这个启动点的模块还依赖这么多东西，他就会等待这些东西加载完成。


如果使用到更多的模块要在起点模块中注册依赖。











###模块之间的互相依赖怎么做？---依赖注入


```javascript
var bookStoreApp = angular.module('bookStoreApp', [
    'ngRoute', 'ngAnimate', 'bookStoreCtrls', 'bookStoreFilters',
    'bookStoreServices', 'bookStoreDirectives'
]);
```

`bookStoreApp `模块之间这种依赖的方式写的是一个字符串，那么AngularJS是怎么知道你是这样一个依赖关系呢？


这个就要借助于AngularJS叫做依赖注入的一个机制，



```javascript
var bookStoreApp = angular.module('bookStoreApp', [
    'ngRoute', 'ngAnimate', 'bookStoreCtrls', 'bookStoreFilters',
    'bookStoreServices', 'bookStoreDirectives'
]);
```

虽然注入写的是字符串，但是AngularJS在启动的时候她回去检测，检测字符串和对于的模块到底有没有注册或者说有没有被加载进来，如果你写一个模块没有的话AngularJS是会报错的，AngularJS内部的依赖注入是这样去做的。


需要知道如果你需要依赖一些模块，你只要在后面的方括号数组里面





![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-69867d5d9f418e9a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


以ng开头的是AngularJS自身所提供的模块，'ngRoute'ru'路由, 'ngAnimate动画',


AngularJS自身都提供了哪些模块呢？




![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-767c23454b933eb5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


angular-animate.js动画


angular-mocks.js是用来做单元测试写一些假数据



angular-scenario.js场景测试集成测试会依赖他


angular-touch.js支持移动上面的开发手机版本的开发



这么多模块，根据项目需求去选择

但是核心的`angular.js`是必须加载进来的
