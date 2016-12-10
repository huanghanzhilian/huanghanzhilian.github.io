---
layout: post
title: JavaScript 获取鼠标点击位置坐标 
subtitle: JavaScript 获取鼠标点击位置坐标 
author: 继小鹏
date: 2016-12-11 05:42:46 +0800
category: JavaScript
tag: DOM
---
在一些DOM操作中我们经常会跟元素的位置打交道，鼠标交互式一个经常用到的方面，令人失望的是不同的浏览器下会有不同的结果甚至是有的浏览器下没结果，这篇文章就上鼠标点击位置坐标获取做一些简单的总结，没特殊声明代码在IE8，FireFox，Chrome下进行测试兼容


####鼠标点击位置坐标


**相对于屏幕**


如果是涉及到鼠标点击确定位置相对比较简单，获取到鼠标点击事件后，事件screenX，screenY获取的是点击位置相对于屏幕的左边距与上边距，不考虑iframe因素，不同浏览器下表现的还算一致。



    document.onclick = function(event) {
    	var e = event || window.event;
    	console.log('x=' + e.screenX + ',' + 'y=' + e.screenY)
    }



**相对浏览器窗口**

简单代码即可实现，然而这是还不够，因为绝大多数情况下我们希望获取鼠标点击位置相对于浏览器窗口的坐标，event的clientX，clientY属性分别表示鼠标点击位置相对于文档的左边距，上边距。于是类似的我们写出了这样的代码



	document.onclick = function(event) {
		var e = event || window.event;
		console.log('x=' + e.clientX + ',' + 'y=' + e.clientX)
	}



**相对文档**


简单测试也没什么问题，但是clientX与clientY获取的是相对于当前屏幕的坐标，忽略页面滚动因素，这在很多条件下很有用，但当我们需要考虑页面滚动，也就是相对于文档（body元素）的坐标时怎么办呢？加上滚动的位移就可以了，下边我们试试怎么计算页面滚动的位移。

其实在Firefox下问题会简单很多，因为Firefox支持属性pageX,与pageY属性，这两个属性已经把页面滚动计算在内了。

在Chrome可以通过document.body.scrollLeft，document.body.scrollTop计算出页面滚动位移，而在IE下可以通过document.documentElement.scrollLeft
 ，document.documentElement.scrollTop



	document.body.style.height = "1500px"
	document.onclick = function getMousePos(event) {
		var e = event || window.event;
		var scrollX = document.documentElement.scrollLeft || document.body.scrollLeft;
		var scrollY = document.documentElement.scrollTop || document.body.scrollTop;
		var x = e.pageX || e.clientX + scrollX;
		var y = e.pageY || e.clientY + scrollY;
		//alert('x: ' + x + '\ny: ' + y);

		console.log('x=' + x + ',' + 'y=' + y)
	}



















