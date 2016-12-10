---
layout: post
title: JavaScript获取DOM元素位置和尺寸大小
subtitle: JavaScript获取DOM元素位置和尺寸大小
author: 继小鹏
date: 2016-12-11 05:46:09 +0800
category: JavaScript
tag: DOM
---
在一些复杂的页面中经常会用JavaScript处理一些DOM元素的动态效果，这种时候我们经常会用到一些元素位置和尺寸的计算，浏览器兼容性问题也是不可忽略的一部分，要想写出预想效果的JavaScript代码，我们需要了解一些基本知识。

####基础概念

为了方便理解，我们需要了解几个基础概念，每个HTML元素都有下列属性

offset| client| scroll
----|------|----
offsetWidth| clientWidth| scrollWidth
offsetHeight| clientHeight| scrollHeight
offsetLeft| clientLeft| scrollLeft
offsetTop| clientTop| scrollTop

>clientHeight和clientWidth用于描述元素内尺寸，是指 元素内容+内边距 大小，不包括边框（IE下实际包括）、外边距、滚动条部分

>offsetHeight和offsetWidth用于描述元素外尺寸，是指 元素内容+内边距+边框，不包括外边距和滚动条部分

>clientTop和clientLeft返回内边距的边缘和边框的外边缘之间的水平和垂直距离，也就是左，上边框宽度

>offsetTop和offsetLeft表示该元素的左上角（边框外边缘）与已定位的父容器（offsetParent对象）左上角的距离
>offsetParent对象是指元素最近的定位（relative,absolute）祖先元素，递归上溯，如果没有祖先元素是定位的话，会返回null




写个小例子方便理解


	<div id="divParent" style="padding: 18px; background-color: #aaa; position: relative;">
	    <div id="divDisplay" style="background-color: #0f0; margin: 30px; padding: 10px;
	        height: 200px; width: 200px; border: solid 10px #f00;">
	    </div>
	</div>



	var div = document.getElementById('divDisplay');

	var clientHeight = div.clientHeight;
	var clientWidth = div.clientWidth;
	div.innerHTML += 'clientHeight: ' + clientHeight + '<br/>';
	div.innerHTML += 'clientWidth: ' + clientWidth + '<br/>';

	var clientLeft = div.clientLeft;
	var clientTop = div.clientTop;
	div.innerHTML += 'clientLeft: ' + clientLeft + '<br/>';
	div.innerHTML += 'clientTop: ' + clientTop + '<br/>';

	var offsetHeight = div.offsetHeight;
	var offsetWidth = div.offsetWidth;
	div.innerHTML += 'offsetHeight: ' + offsetHeight + '<br/>';
	div.innerHTML += 'offsetWidth: ' + offsetWidth + '<br/>';

	var offsetLeft = div.offsetLeft;
	var offsetTop = div.offsetTop;
	div.innerHTML += 'offsetLeft: ' + offsetLeft + '<br/>';
	div.innerHTML += 'offsetTop: ' + offsetTop + '<br/>';

	var offsetParent = div.offsetParent;
	div.innerHTML += 'offsetParent: ' + offsetParent.id + '<br/>';


效果如图



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-4c16b0f2a5f14f47.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-f29eb579a580df62.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


从图中我们可以看到，clientHeight就是div的高度+上下各10px的padding，clientWidth同理

而clientLeft和ClientTop即为div左、上边框宽度

offsetHeight是clientHeight+上下个3px的边框宽度之和，offsetWidth同理

offsetTop是div 30px的 maggin+offsetparent 8px的 padding，offsetLeft同理


>6.scrollWidth和scrollHeight是元素的内容区域加上内边距加上溢出尺寸，当内容正好和内容区域匹配没有溢出时，这些属性与clientWidth和clientHeight相等

>7.scrollLeft和scrollTop是指元素滚动条位置，它们是**可写**的



下面写个简单例子理解

	<div id="divParent" style="background-color: #aaa; padding:8px; border:solid 7px #000; height:200px; width:500px; overflow:auto;">
	    <div id="divDisplay" style="background-color: #0f0; margin: 30px; padding: 10px;
	        height: 400px; width: 200px; border: solid 3px #f00;">
	    </div>
	</div>



    var divP = document.getElementById('divParent');
    var divD = document.getElementById('divDisplay');

    var scrollHeight = divP.scrollHeight;
    var scrollWidth = divP.scrollWidth;
    divD.innerHTML += 'scrollHeight: ' + scrollHeight + '<br/>';
    divD.innerHTML += 'scrollWidth: ' + scrollWidth + '<br/>';



效果图


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-fc19f4bb6fe78aa6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



在FireFox和IE10（IE10以下版本盒模型和W3C标准不一致，不加讨论，兼容性问题下面会介绍到）下得到结果scrollHeight: **494**，而在Chrome和Safari下得到结果scrollHeight: **502**，差了8px，根据8可以简单推测是divParent的padding，来算一下是不是

我们可以看看它们的结果是怎么来的，首先scrollHeight肯定包含了divDisplay所需的高度 400px的高度+上下各10px的padding+上下各3px的border+上下各30px的margin，这样

400+10*2+3*2+30*2=486

这样486+8=494， 486+8*2=502果真是这样，**在FireFox和IE10下没有计算下padding**

有了这些基础知识后，我们就可以计算元素的位置和尺寸了。


####相对于文档与视口的坐标



当我们计算一个DOM元素位置也就是坐标的时候，会涉及到两种坐标系，**文档坐标**和**视口坐标**。

我们经常用到的**document就是整个页面部分，而不仅仅是窗口可见部分**，还包括因为窗口大小限制而出现滚动条的部分，它的左上角就是我们所谓相对于文档坐标的原点。

**视口是显示文档内容的浏览器的一部分，它不包括浏览器外壳**（菜单，工具栏，状态栏等），也就是当前窗口显示页面部分，不包括滚动条。

如果文档比视口小，说明没有出现滚动，文档左上角和视口左上角相同，一般来讲在两种坐标系之间进行切换，需要加上或减去滚动的偏移量（scroll offset）。

为了在坐标系之间进行转换，我们需要判定浏览器窗口的滚动条位置。window对象的pageXoffset和pageYoffset提供这些值，IE 8及更早版本除外。也可以通过scrollLeft和scrollTop属性获得滚动条位置，正常情况下通过查询文档根节点（document.documentElement）来获得这些属性值，但在怪异模式下必须通过文档的body上查询。



	function getScrollOffsets(w) {
		var w = w || window;
		if (w.pageXoffset != null) {
			return {
				x: w.pageXoffset,
				y: pageYoffset
			};
		}
		var d = w.document;
		if (document.compatMode == "CSS1Compat")
			return {
				x: d.documentElement.scrollLeft,
				y: d.documentElement.scrollTop
			};
		return {
			x: d.body.scrollLeft,
			y: d.body.scrollTop
		};
	}



有时候能够判断视口的尺寸也是非常有用的


	function getViewPortSize(w) {
		var w = w || window;
		if (w.innerWidth != null)
			return {
				w: w.innerWidth,
				h: w.innerHeight
			};
		var d = w.document;
		if (document.compatMode == "CSS1Compat")
			return {
				w: d.documentElement.clientWidth,
				h: d.documentElement.clientHeight
			};
		return {
			w: d.body.clientWidth,
			h: d.body.clientHeight
		};
	}



**文档坐标**

任何HTML元素都拥有offectLeft和offectTop属性返回元素的X和Y坐标，对于很多元素，这些值是文档坐标，但是对于以定位元素后代及一些其他元素（表格单元），返回相对于**祖先**的坐标。我们可以通过简单的递归上溯累加计算



	function getElementPosition(e) {
		var x = 0,
			y = 0;
		while (e != null) {
			x += e.offsetLeft;
			y += e.offsetTop;
			e = e.offsetParent;
		}
		return {
			x: x,
			y: y
		};
	}



尽管如此，这个函数也不总是计算正确的值，当文档中含有滚动条的时候这个方法就不能正常工作了，我们只能在没有滚动条的情况下使用这个方法，不过我们用这个原理算出一些元素相对于某个父元素的坐标。

**视口坐标**

计算**视口坐标**就相对简单了很多，可以通过调用**元素**的**getBoundingClientRect**方法。方法返回一个有left、right、top、bottom属性的对象，分别表示元素四个位置的相对于视口的坐标。**getBoundingClientRect所返回的坐标包含元素的内边距和边框**，不包含外边距。兼容性很好，非常好用

**元素尺寸**

由上面计算坐标方法，我们可以方便得出元素尺寸。在符合W3C标准的浏览器中getBoundingClientRect返回的对象还包括width和height,但在原始IE中未实现，但是通过返回对象的right-left和bottom-top可以方便计算出。