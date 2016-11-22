---
layout: post
title: xsssssss
category: mysql
tag: jekyll
---
<h3>ssssssssssss</h3>





1. 简单流程概括：

    我们在使用requireJS时，都会把所有的js交给requireJS来管理，也就是我们的页面上只引入一个require.js，把data-main指向我们的main.js。
    通过我们在main.js里面定义的require方法或者define方法，requireJS会把这些依赖和回调方法都用一个数据结构保存起来。
    当页面加载时，requireJS会根据这些依赖预先把需要的js通过document.createElement的方法引入到dom中，这样，被引入dom中的script便会运行。
    由于我们依赖的js也是要按照requireJS的规范来写的，所以他们也会有define或者require方法，同样类似第二步这样循环向上查找依赖，同样会把他们村起来。
    当我们的js里需要用到依赖所返回的结果时(通常是一个key value类型的object),requireJS便会把之前那个保存回调方法的数据结构里面的方法拿出来并且运行，然后把结果给需要依赖的方法。
    以上就是一个简单的流程。

 
2. 测试代码

下面我把一个requireJS小Demo写出来，是下面研究源码的基础：
main.js

{% highlight ruby %}
require.config({
	paths: {
		"a": "a",
		"b": "b"
	}
});
require(['a'], function (a){
    console.log('main');
    //console.log(a);
});
{% endhighlight %}


{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

{% highlight ruby %}
def show
  @widget = Widget(params[:id])
  respond_to do |format|
    format.html # show.html.erb
    format.json { render json: @widget }
  end
end
{% endhighlight %}


{% highlight ruby %}
req = requirejs = function (deps, callback, errback, optional) {

    //Find the right context, use default
    var context, config,
        contextName = defContextName;

    // Determine if have config object in the call.
    if (!isArray(deps) && typeof deps !== 'string') {
        // deps is a config object
        config = deps;
        if (isArray(callback)) {
            // Adjust args if there are dependencies
            deps = callback;
            callback = errback;
            errback = optional;
        } else {
            deps = [];
        }
    }

    if (config && config.context) {
        contextName = config.context;
    }

    context = getOwn(contexts, contextName);
    if (!context) {
        context = contexts[contextName] = req.s.newContext(contextName);
    }

    if (config) {
        context.configure(config);
    }
    var fg = context.require(deps, callback, errback);
    return fg;
};
{% endhighlight %}

转载请注明本文出自xiaanming的博客（http://blog.csdn.net/xiaanming/article/details/26810303），请尊重他人的辛勤劳动成果，谢谢！

大家好！差不多两个来月没有写文章了，前段时间也是在忙换工作的事，准备笔试面试什么的事情，现在新工作找好了，新工作自己也比较满意，唯一遗憾的就是自己要去一个新的城市，新的环境新的开始，希望自己能尽快的适应新环境，现在在准备交接的事情，自己也有一些时间了，所以就继续给大家分享Android方面的东西。

相信大家平时做Android应用的时候，多少会接触到异步加载图片，或者加载大量图片的问题，而加载图片我们常常会遇到许多的问题，比如说图片的错乱，OOM等问题，对于新手来说，这些问题解决起来会比较吃力，所以就有很多的开源图片加载框架应运而生，比较著名的就是Universal-Image-Loader，相信很多朋友都听过或者使用过这个强大的图片加载框架，今天这篇文章就是对这个框架的基本介绍以及使用，主要是帮助那些没有使用过这个框架的朋友们。该项目存在于Github上面https://github.com/nostra13/Android-Universal-Image-Loader，我们可以先看看这个开源库存在哪些特征

多线程下载图片，图片可以来源于网络，文件系统，项目文件夹assets中以及drawable中等
支持随意的配置ImageLoader，例如线程池，图片下载器，内存缓存策略，硬盘缓存策略，图片显示选项以及其他的一些配置
支持图片的内存缓存，文件系统缓存或者SD卡缓存
支持图片下载过程的监听
根据控件(ImageView)的大小对Bitmap进行裁剪，减少Bitmap占用过多的内存
较好的控制图片的加载过程，例如暂停图片加载，重新开始加载图片，一般使用在ListView,GridView中，滑动过程中暂停加载图片，停止滑动的时候去加载图片
提供在较慢的网络下对图片进行加载

当然上面列举的特性可能不全，要想了解一些其他的特性只能通过我们的使用慢慢去发现了，接下来我们就看看这个开源库的简单使用吧

新建一个Android项目，下载JAR包添加到工程libs目录下

新建一个MyApplication继承Application，并在onCreate()中创建ImageLoader的配置参数，并初始化到ImageLoader中代码如下
<p>个人简历</p>
<p>{{ page.date | date_to_string }}</p>