---
layout: post
title: DOM事件探秘(2)事件类型
subtitle: DOM事件探秘(2)事件类型
author: 继小鹏
date: 2016-12-11 05:50:04 +0800
category: JavaScript
tag: DOM
---
DOM有不同的事件类型，按大类分有鼠标事件，键盘事件。根据《DOM事件探秘》文章，我们来做几个综合实例。


>鼠标事件

面板拖拽状态切换功能。演示地址：[http://www.huanghanlian.com/DOM_Event_demo/Drag/](http://www.huanghanlian.com/DOM_Event_demo/Drag/)          


>知识点


有的时候我们想通过`class`去取元素，这时候我们可以写个简单封装。


	<div class="box">
		<div class="bon_com">

		</div>
		<div class="bon_com">

		</div>
		<div class="bon_com1">

		</div>
	</div>
	<script type="text/javascript">
	function getByClass(clsName, parent) {
		var oParent = parent ? document.getElementById(parent) : document,
			eles = [],
			elements = oParent.getElementsByTagName('*');

		for (var i = 0, l = elements.length; i < l; i++) {
			if (elements[i].className == clsName) {
				eles.push(elements[i]);
			}
		}
		return eles;
	}
	var p=getByClass("bon_com")[0];
	p.innerHTML="d";

上面的代码，`getByClass`函数要传两个参数，第一个是`class`类名，第二个是它父元素的`id`这个id可以不传，不传会默认选择`document`传进id或者`document`，然后`elements `变量循环id下的所有元素，循环值等于传进来的`class`时，把它`push`到`eles `数组里，然后再返回`return eles;`这样我们就成功找到了`class`元素了。


####1.面板拖拽功能实现

>效果图



![tuozuai.gif](http://upload-images.jianshu.io/upload_images/3877962-7cc52e0392f21b9a.gif?imageMogr2/auto-orient/strip)


>实现步骤

1. 告诉浏览器你在哪个地方按下的时候要开始拖动。
2. 要在页面中移动
3. 释放鼠标时停止移动




**告诉浏览器你在哪个地方按下的时候要开始拖动**

页面加载完运行拖拽函数，先获取要按下的区域             
当鼠标按下`onmousedown`事件，执行`fnDown`函数。

**要在页面中移动**

当鼠标按下指定按下区域时，在整个页面移动`onmousemove`意思是鼠标每次移动都会触发`onmousemove`事件，然后改变`box`的`clientX`和`clientY`坐标位置。

**释放鼠标时停止移动**

当鼠标松开`onmouseup`让`onmousemove`卸载掉。

    document.onmousemove=null;





####1.状态切换效果


![tuozuai1.gif](http://upload-images.jianshu.io/upload_images/3877962-729d355208b930bb.gif?imageMogr2/auto-orient/strip)



实现过程是，当点击切换区域打开切换列表，当鼠标经过和离开`li`时候改变其背景颜色，当点击的时候关闭切换列表，状态文字和图标换成点击的那个元素属性。这里有几个事件冒泡需要处理。




>键盘事件(抽奖)


演示地址：[http://www.huanghanlian.com/DOM_Event_demo/keyEvent/](http://www.huanghanlian.com/DOM_Event_demo/keyEvent/)          


1. `keyDown`当用户按下键盘上任意键时触发，而且如果按住不放会重复的触发此事件。
2. `keyPress`键盘上的字符键的时候触发，如果按住不放会重复的触发此事件。
3. `keyUp`当用户释放键盘上的键时候触发。


先把奖品放入数组，两个按钮，一个开始一个结束，点击开始时，运行`setInterval()`定时器，让定时器每隔20毫秒运行一个匿名函数，匿名函数里面的方法每隔50毫秒`Math.random()`生成随机数，`Math.random()`生成的是等于或者大于0，小于1的浮点数，如果我们要去查找数组的索引，比如数组有8个商品，那么就要有0-7的索引，那么`8*0等于0`我们获取最小索引，`8*0.99会小于8`我们把得来的随机数乘以数组长度`length`再把它`Math.floor()`向下舍入取整这样就能获取0-7的索引，然后再把要显示奖品元素点击停止时替换随机来的索引奖品即可。

再一个就是键盘事件，
    
    var flag = 0;
    // 键盘事件
    document.onkeyup = function(event) {
      event = event || window.event;
      if (event.keyCode == 13) {
        if (flag == 0) {
          playFun();
          flag = 1;
        } else {
          stopFun();
          flag = 0;
        }
      }
    }

先给`flag`为0也就是false

监听键盘事件 `onkeyup `键盘释放，判断释放是回车键，如果是在判断是否`flag`为0也就是false，就执行抽奖函数，并把`flag`变为1也就是true，再次点击又监听`onkeyup `键盘释放事件，这次`flag`为1就执行停止抽奖事件。以此来来回回第一次点击回车抽奖，再点以此停止......