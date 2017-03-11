---
layout: post
title: 基本概念和用法-综合应用BookStore  2-8
subtitle: 基本概念和用法-综合应用BookStore  2-8
author: 继小鹏
date: 2017-03-11 23:58:13 +0800
category: AngularJS
tag: AngularJS
---

AngularJS实例开发



![图片.png](http://upload-images.jianshu.io/upload_images/3877962-744d6ab9cec78f53.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


###界面原型设计

拿到一个项目以后，首先根据功能点，根据需求分析，把界面先画出来，有了这个界面以后才能去搭建前台的一些界面，


###怎么切分功能模块建立项目目录结构

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-61b011c81442379d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


###根据划分的目录结构使用angularjs UI和Bootstrap编写UI

使用angularjs UI和Bootstrap编写UI先把界面先拉出来，可能没有功能，点击一些东西没有真正请求后台，但是我们可以先把界面先写出来，这样像流水线那样去写代码，可能有些人比较喜欢从前台一直写道后台，一直从css到js然后一直写道后台服务，写道数据库脚本，那么这样的开发方式。这种方式也不能说不好，但是从生产效率来说，最好还是使用流水线的方式， 就是说你做前台一些工作的时候，尽量把前台的一些东西写完，一批一批的去做，不用频繁的去切换上下文，因为频繁切换实际上带来是比较多效率的损失，



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-02eb8eed83454348.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


使用Bootstrap布局登入页面

这里登入页面没做校验直接可登入



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-2af9061535c42364.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


点击登入进入列表页

列表页左侧是根据不同的书籍类型右侧是`ng-guer`

点击左侧会加载不同类型数据


这些数据都是假的， 把假数据写在json文件里面，

```javascript
[{
    "bookId":"1",
    "index": "1",
    "name": "用AngularJS开发下一代WEB应用",
    "author": "大漠穷秋",
    "pubTime": "2014-01-01",
    "price":"35"
}, {
    "bookId":"2",
    "index": "2",
    "name": "Ext江湖",
    "author": "大漠穷秋",
    "pubTime": "2014-01-01",
    "price":"35"
}]

```

这种做法在开发中比较有用，如果总是依赖后台可能老是需要打扰你的同事，或者是你一直切换上下文，到后台去调试服务，这样其实工作的效率不会特别高，做前台的时候。可以做一些假的数据放在json里，把逻辑测试通过以后再去写后台，这个过程你顺手还把数据结构数据的结构给定义了，



首先把界面写出来，一开始要写的就是模板，根据设计图然后通过样式，把模板写出来，


比如登入的表单，`loginForm.html`

```javascript
<div class="panel panel-primary">
    <div class="panel-body">
        <div class="row">
            <div class="col-md-12">
                <h2 class="text-center">图书管理系统</h2>
            </div>
        </div>
        <div class="row">
            <div class="col-md-12">
                <form class="form-horizontal" role="form">
                    <div class="form-group">
                        <label class="col-md-2 control-label">
                            用户名：
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
                            <a ui-sref="booklist({bookType:0})" class="btn btn-success btn-lg">登录</a>
                            <button class="btn btn-default btn-lg" ng-click="setFormData()">注册</button>
                        </div>
                    </div>
                </form>
            </div>
        </div>
    </div>
</div>
```


`home.html`home是最开始作为容器的一个东西，

把模板都写出来之后再去写js的代码，






```html
<div class="container">
    <div ui-view="main"></div>
</div>

```


###路由的使用


AngularJS内置有路由的工具但是不太好用，在真正的项目开发中，还是要用`UIRouter`路由工具，



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-a65a7e724786e70a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
