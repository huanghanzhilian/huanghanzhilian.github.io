---
layout: post
title: 我的
category: php
tag: jekyll update
---

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