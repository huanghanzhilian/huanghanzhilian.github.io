---
layout: post
title: DOM事件探秘
subtitle: DOM事件探秘
author: 继小鹏
date: 2016-12-08 23:20:07 +0800
category: JavaScript
tag: DOM
---
JavaScript和HTML的交互是通过事件实现的。JavaScript采用异步事件驱动编程模型，当文档、浏览器、元素或与之相关对象发生特定事情时，浏览器会产生事件。如果JavaScript关注特定类型事件，那么它可以注册当这类事件发生时要调用的句柄。


####理解事件流

>事件流

比如有一个`div`元素，`div`里面有一个`按钮`。点击按钮的同时，也触发了div，甚至触发了div所在的整个页面。     
这个过程就是一个**事件流**。

**事件流描述的是从页面中接收事件的顺序，比如有两个嵌套的div，点击了内层的div，这时候是内层的div先出发click事件还是外层先触发？目前主要有三种模型**




1. IE的事件冒泡：事件开始时由最具体的元素接收，然后逐级向上传播到较为不具体的元素

2. Netscape的事件捕获：不太具体的节点更早接收事件，而最具体的元素最后接收事件，和事件冒泡相反

3. DOM事件流：DOM2级事件规定事件流包括三个阶段，事件捕获阶段，处于目标阶段，事件冒泡阶段，首先发生的是事件捕获，为截取事件提供机会，然后是实际目标接收事件，最后是冒泡句阶段。


Opera、Firefox、Chrome、Safari都支持DOM事件流，IE不支持事件流，只支持事件冒泡

如有以下html，点击div区域


	<!DOCTYPE html>
	<html lang="en">
	<head>
	    <meta charset="UTF-8">
	    <title>Document</title>
		<style type="text/css">

		</style>
	</head>
	<body>
		<div>
	        Click Here
	    </div>
	</body>
	</html>







事件冒泡模型| 事件捕获模型| DOM事件流
----|------|----
![Untitled](http://upload-images.jianshu.io/upload_images/3877962-36b1f0073bbddd0e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)| ![Untitled](http://upload-images.jianshu.io/upload_images/3877962-f9b12ae05b6c0603.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)| ![Untitled](http://upload-images.jianshu.io/upload_images/3877962-7654bffc69119aa9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


####事件处理程序（handler）


我们也称之为事件侦听器（listener），事件就是用户或浏览器自身执行的某种动作。比如click、load、moseover等，都是事件类型（俗称事件名称），而响应某个事件的方法就叫做事件处理程序或者事件监听器或者事件句柄，事件处理程序名字是：on+事件类型。

**HTML事件处理程序**

所谓的HTML事件处理程序，其实就是说，你的这个事件是直接加在`HTML`结构里的。

比如说我现在给`input`按钮写一个鼠标`点击`事件，点击按钮弹出`hello`。

    <input type="button" value="按钮" onclick="alert('hello')">

我把`onclick`事件，直接加到`input`标签上，实在`HTML`结构里，那么这样的事件就叫做**HTML事件**

在HTML事件处理程序中可以包含要执行的具体动作，也可以调用在页面其它地方定义的脚本,刚才的例子可以写成这样


	<div id="box">
		<input type="button" value="按钮" id="btn" onclick="showMessage()">
	</div>
    <script type="text/javascript">
    	function showMessage(){
    		alert('hello world!')
    	}
    </script>


我们把这种直接把事件加在`HTML`结构里的元素上，这种添加事件的方法叫做`HTML事件`。     
在HTML中指定事件处理程序书写很方便，但是有两个缺点。     


首先，存在加载顺序问题，如果事件处理程序在html代码之后加载，用户可能在事件处理程序还未加载完成时就点击按钮之类的触发事件，存在时间差问题。

其次，这样书写html代码和JavaScript代码紧密耦合，维护不方便。



>DOM0级事件处理程序

比较传统的方式。把一个函数赋值给一个事件的处理程序属性。用的比较多的方法，原因是简单 跨浏览器的优势。


通过JavaScript指定事件处理程序就是把一个方法赋值给一个元素的事件处理程序属性。每个元素都有自己的事件处理程序属性，这些属性名称通常为小写，如onclick等，将这些属性的值设置为一个方法，就可以指定事件处理程序，如下


	<div id="box">
		<input type="button" value="按钮" id="btn" onclick="showMessage()">
		<input type="button" value="按钮2" id="btn2">
	</div>
    <script type="text/javascript">
    	function showMessage(){
    		alert('hello world!')
    	}
    	//DOM0级事件处理程序
    	//先把要执行的按钮取到
    	var btn2=document.getElementById("btn2");    //取得btn2按钮对象
    	//给btn2添加onclick属性，这个属性触发事件处理程序，这个事件处理程序可以写成匿名函数，也可以调用已经存在的函数
    	btn2.onclick=function(){
    		alert(this.id)
    	}
    </script>

这样处理，事件处理程序被认为是元素的方法，事件处理程序在元素的作用域下运行，this就是当前元素，所以点击button结果是：btn2

这样还有一个好处，我们可以删除事件处理程序，只需把元素的onclick属性赋为null即可


或者

	<div id="box">
		<input type="button" value="按钮" id="btn" onclick="showMessage()">
		<input type="button" value="按钮2" id="btn2">
	</div>
    <script type="text/javascript">
    	function showMessage(){
    		alert('hello world!')
    	}
    	//DOM0级事件处理程序
    	//先把要执行的按钮取到
    	var btn2=document.getElementById("btn2");    //取得btn2按钮对象
    	//给btn2添加onclick属性，这个属性触发事件处理程序，这个事件处理程序可以写成匿名函数，也可以调用已经存在的函数
    	btn2.onclick=function(){
    		showMessage();
    	}
    </script>


也可以删掉这个事件


	<div id="box">
		<input type="button" value="按钮" id="btn" onclick="showMessage()">
		<input type="button" value="按钮2" id="btn2">
	</div>
    <script type="text/javascript">
    	function showMessage(){
    		alert('hello world!')
    	}
    	//DOM0级事件处理程序
    	//先把要执行的按钮取到
    	var btn2=document.getElementById("btn2");    //取得btn2按钮对象
    	//给btn2添加onclick属性，这个属性触发事件处理程序，这个事件处理程序可以写成匿名函数，也可以调用已经存在的函数
    	btn2.onclick=function(){
    		showMessage();
    	}

    	btn2.onclick=null;



>DOM2事件处理程序

DOM2级事件定义了两个方法用于处理指定和删除事件处理程序的操作：`addEventListener`和`removeEventListener`。所有的DOM节点都包含这两个方法。    

并且它们都接受三个参数：

1. 事件类型、

2. 事件处理方法、

3. 一个布尔值。最后布尔参数如果是**true**表示在**捕获阶段**调用事件处理程序，如果是**false**，则是在事件**冒泡阶段**处理。

刚才的例子我们可以这样写


	<div id="box">
		<input type="button" value="按钮2" id="btn">
	</div>
    <script type="text/javascript">
    	var btn = document.getElementById("btn"); //取得btn按钮对象
    	btn.addEventListener('click', function() {
    		alert(this.id);
    	}, false);
    </script>


也可以这样

	<div id="box">
		<input type="button" value="按钮2" id="btn">
	</div>
    <script type="text/javascript">
    	function showelen() {
    		alert(this.id)
    	}
    	var btn = document.getElementById("btn"); //取得btn按钮对象
    	btn.addEventListener('click', showelen, false);
    </script>


上面代码为button添加了click事件的处理程序，在冒泡阶段触发，与上一种方法一样，这个程序也是在元素的作用域下运行，不过有一个好处，我们可以为click事件添加多个处理程序


	<div id="box">
		<input type="button" value="按钮2" id="btn">
	</div>
    <script type="text/javascript">
    	var btn = document.getElementById("btn"); //取得btn按钮对象
    	btn.addEventListener('click', function() {
    		alert(this.id);
    	}, false);
    	btn.addEventListener('click', function() {
    		alert('Hello!');
    	}, false);
    </script>


这样两个事件处理程序会在用户点击button后按照添加顺序依次执行。


通过`addEventListener`添加的事件处理程序只能通过`removeEventListener`移除，移除时参数与添加的时候相同，这就意味着刚才我们添加的匿名函数无法移除，因为匿名函数虽然方法体一样，但是句柄却不相同，所以当我们有移除事件处理程序的时候可以这样写


	<div id="box">
		<input type="button" value="按钮2" id="btn">
	</div>
    <script type="text/javascript">
    	function showelen() {
    		alert(this.id)
    	}
    	var btn = document.getElementById("btn"); //取得btn按钮对象
    	btn.addEventListener('click', showelen, false);
    	btn.removeEventListener('click', showelen, false);
    </script>


这样写的效果挺好，但是要是低于IE8的浏览器，点击是完全没有效果的。下面我们来介绍ie兼容性问题。


>IE事件处理程序

IE并不支持`addEventListener`和`removeEventListener`方法，而是实现了两个类似的方法`attachEvent`和`detachEvent`，这两个方法都接收两个相同的参数，事件处理程序名称和事件处理程序方法，由于IE指支持事件冒泡，所以添加的程序会被添加到冒泡阶段。


使用attachEvent添加事件处理程序可以如下

	<div id="box">
		<input type="button" value="按钮" id="btn">
	</div>
    <script type="text/javascript">
    	var showelen=function() {
    		alert(this.id)
    	}
    	var btn = document.getElementById("btn"); //取得btn按钮对象
    	btn.attachEvent('onclick',showelen);
    </script>


结果是undefined，很奇怪，一会儿我们会介绍到

使用attachEvent添加的事件处理程序可以通过detachEvent移除，条件也是相同的参数，匿名函数不能被移除。



	<div id="box">
		<input type="button" value="按钮" id="btn">
	</div>
    <script type="text/javascript">
    	var showelen = function() {
    		alert('hello')
    	}
    	var btn = document.getElementById("btn"); //取得btn按钮对象
    	btn.attachEvent('onclick', showelen);
    	btn.detachEvent('onclick', showelen);
    </script>



那么现在回头想想，如果用`DOM2事件处理程序`,IE就不能有效。如果用`IE事件处理程序`,非IE浏览器就不能有效。那么这就是问题啊！

紧接着我们解决这个问题，`跨浏览器的事件处理程序`。



**跨浏览器的事件处理程序**


如何进行跨浏览器的事件处理呢？

恰当的使用能力检测，什么叫能力检测呢？          
就是说你有这个能力，你就用这个，你没有这个能力你有别的能力那你就去用你别的能力。

也就是说你要是支持`addEventListener`那你就用它。你要是支持`attachEvent`那你就用它呗。


这个跨浏览器，建议最好是封装在一个对象内。



	var showelen = function() {
		alert('hello')
	}
	var btn = document.getElementById("btn"); //取得btn按钮对象


	//跨浏览器事件处理程序
	var eventutil={
		//添加句柄
		addhandler:function(element,type,handler){
			if(element.addEventListener){                                   //判断是否支持dom2级
				element.addEventListener(type,handler,false);               //支持就运行dom2级事件处理程序
			}else if(element.attachEvent){                                  //判断是否支持ie事件处理
				element.attachEvent('on'+type,handler);                     //支持就运行ie事件处理程序   事件类型需要加'on'
			}else{                                                          //如果都不是就运行dom0级处理程序
				element['on'+type]=handler;
				//连接属性的时候,所有用到'.'的地方都可以用[]中括号
				//element.onclick===element['onclick']
			}
		},
		//删除句柄
		removehandler:function(element,type,handler){
			if(element.removeEventListener){
				element.removeEventListener(type,handler,false);
			}else if(element.detachEvent){
				element.detachEvent('on'+type,handler);
			}else{
				element['on'+type]=null;
			}
		}
	}

	eventutil.addhandler(btn,'click',showelen);
	//eventutil.removehandler(btn,'click',showelen)


连接属性的时候,所有用到'`.`'的地方都可以用`[]中括号 `                
`element.onclick===element['onclick']`



还可以这样写


	<div id="box">
		<input type="button" value="按钮" id="btn">
	</div>
    <script type="text/javascript">
	var showelen = function() {
		alert('hello')
	}
	var btn = document.getElementById("btn"); //取得btn按钮对象


	//跨浏览器事件处理程序
	function addEvent(node, type, handler) {
		if (!node) return false;
		if (node.addEventListener) {
			node.addEventListener(type, handler, false);
			return true;
		} else if (node.attachEvent) {
			node.attachEvent('on' + type, handler);
			return true;
		}
		return false;
	}

	addEvent(btn,'click',showelen);
    </script>



这样，首先我们解决了第一个问题参数个数不同，现在三个参数，采用事件冒泡阶段触发，第二个问题也得以解决，如果是IE，我们给type添加上on，第四个问题目前还没有解决方案，需要用户自己注意，一般情况下，大家也不会添加很多事件处理程序，试试这个方法感觉很不错，但是我们没有解决第三个问题，由于处理程序作用域不同，如果handler内有this之类操作，那么就会出错在IE下，实际上大多数函数都会有this操作。


	<div id="box">
		<input type="button" value="按钮" id="btn">
	</div>
    <script type="text/javascript">
	var showelen = function() {
		alert(this.id)
	}
	var btn = document.getElementById("btn"); //取得btn按钮对象


	//跨浏览器事件处理程序
	function addEvent(node, type, handler) {
		if (!node) return false;
		if (node.addEventListener) {
			node.addEventListener(type, handler, false);
			return true;
		} else if (node.attachEvent) {
			node.attachEvent('on' + type, function() { handler.apply(node); });
                        //handler.apply(node),将原本属于handler属性方法交给对象node来使用了。
			return true;
		}
		return false;
	}

	addEvent(btn,'click',showelen);
    </script>



这样处理就可以解决this的问题了，但是新的问题又来了，我们这样等于添加了一个匿名的事件处理程序，无法用detachEvent取消事件处理程序，有很多解决方案，我们可以借鉴大师的处理方式，jQuery创始人**John Resig**是这样做的


	<div id="box">
		<input type="button" value="按钮" id="btn">
	</div>
    <script type="text/javascript">
	var showelen = function() {
		alert(this.id)
	}
	var btn = document.getElementById("btn"); //取得btn按钮对象


	//跨浏览器事件处理程序
	function addEvent(node, type, handler) {
		if (!node) return false;
		if (node.addEventListener) {
			node.addEventListener(type, handler, false);
			return true;
		} else if (node.attachEvent) {
			node['e' + type + handler] = handler;
			node[type + handler] = function() {
				node['e' + type + handler](window.event);
			};
			node.attachEvent('on' + type, node[type + handler]);
			return true;
		}
		return false;
	}

	addEvent(btn,'click',showelen);
    </script>



在取消事件处理程序的时候


	<div id="box">
		<input type="button" value="按钮" id="btn">
	</div>
    <script type="text/javascript">
	var showelen = function() {
		alert(this.id)
	}
	var btn = document.getElementById("btn"); //取得btn按钮对象


	//跨浏览器事件处理程序
	function addEvent(node, type, handler) {
		if (!node) return false;
		if (node.addEventListener) {
			node.addEventListener(type, handler, false);
			return true;
		} else if (node.attachEvent) {
			node['e' + type + handler] = handler;
			node[type + handler] = function() {
				node['e' + type + handler](window.event);
			};
			node.attachEvent('on' + type, node[type + handler]);
			return true;
		}
		return false;
	}


	function removeEvent(node, type, handler) {
		if (!node) return false;
		if (node.removeEventListener) {
			node.removeEventListener(type, handler, false);
			return true;
		} else if (node.detachEvent) {
			node.detachEvent('on' + type, node[type + handler]);
			node[type + handler] = null;
		}
		return false;
	}

	addEvent(btn,'click',showelen);
	removeEvent(btn,'click',showelen);
    </script>



####事件对象

什么是事件对象？在触发DOM上的事件时都会产生一个对象。

**事件对象event**

>DOM中的事件对象


	<div id="box">
		<input type="button" value="按钮" id="btn">
	</div>
    <script type="text/javascript">
	var btn = document.getElementById("btn"); //取得btn按钮对象
	btn.onclick = function(e) {
		alert(e.type);//click
	}
    </script>


或者

	<div id="box">
		<input type="button" value="按钮" id="btn">
	</div>
    <script type="text/javascript">
	var showelen = function(e) {
	    alert(e.type);    //click
	}
	var btn = document.getElementById("btn"); //取得btn按钮对象


	//跨浏览器事件处理程序
	var eventutil={
	    //添加句柄
	    addhandler:function(element,type,handler){
	        if(element.addEventListener){                                   //判断是否支持dom2级
	            element.addEventListener(type,handler,false);               //支持就运行dom2级事件处理程序
	        }else if(element.attachEvent){                                  //判断是否支持ie事件处理
	            element.attachEvent('on'+type,handler);                     //支持就运行ie事件处理程序   事件类型需要加'on'
	        }else{                                                          //如果都不是就运行dom0级处理程序
	            element['on'+type]=handler;
	            //连接属性的时候,所有用到'.'的地方都可以用[]中括号
	            //element.onclick===element['onclick']
	        }
	    },
	    //删除句柄
	    removehandler:function(element,type,handler){
	        if(element.removeEventListener){
	            element.removeEventListener(type,handler,false);
	        }else if(element.detachEvent){
	            element.detachEvent('on'+type,handler);
	        }else{
	            element['on'+type]=null;
	        }
	    }
	}

	eventutil.addhandler(btn,'click',showelen);
	//eventutil.removehandler(btn,'click',showelen)
    </script>





点击button的时候我们可以看到弹出内容是click的弹窗

event对象包含与创建它的特定事件有关的属性和方法，触发事件的类型不同，可用的属性和方法也不同，但是所有事件都会包含


**属性/方法**|**类型**|**读/写**|**说明**
----|------|----
bubbles|Boolean|只读|事件是否冒泡
cancelable|Boolean|只读|是否可以取消事件的默认行为
currentTarget|Element|只读|事件处理程序当前处理元素
detail|Integer|只读|与事件相关细节信息
eventPhase|Integer|只读|事件处理程序阶段：1 捕获阶段，2 处于目标阶段，3 冒泡阶段
preventDefault()|Function|只读|取消事件默认行为
stopPropagation()|Function|只读|取消事件进一步捕获或冒泡
target|Element|只读|事件的目标元素
type|String|只读|被触发的事件类型
view|AbstractView|只读|与事件关联的抽象视图，等同于发生事件的window对象


在事件处理程序内部，this始终等同于currentTarget，而target是事件的实际目标。

要阻止事件的默认行为，可以使用preventDefault（）方法，前提是cancelable值为true，比如我们可以阻止链接导航这一默认行为


	<a id="a" href="http://www.huanghanlian.com/">连接</a>
	<script type="text/javascript">
		document.getElementById('a').onclick = function(e) {
			e.preventDefault();    //取消事件默认行为
		}
	</script>



stopPaopagation()方法可以停止事件在DOM层次的传播，即取消进一步的事件捕获或冒泡。我们可以在button的事件处理程序中调用stopPropagation（）从而避免注册在body上的事件发生


看看区别

	<div id="box">
		<a id="a" href="http://www.huanghanlian.com/">连接</a>
	</div>
	<script type="text/javascript">
		document.getElementById('box').onclick = function(e) {
			console.log('我是盒子box')
		}
		document.getElementById('a').onclick = function(e) {
			console.log(e.type);         //弹出事件类型
			e.preventDefault();          //取消事件默认行为
		}
	</script>



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-baf4778a77388f4f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击连接弹出事件类型后，事件冒泡还会向外扩散认为你也点了box

如果不想这样可以取消事件进一步捕获或冒泡

	<div id="box">
		<a id="a" href="http://www.huanghanlian.com/">连接</a>
	</div>
	<script type="text/javascript">
		document.getElementById('box').onclick = function(e) {
			console.log('我是盒子box')
		}
		document.getElementById('a').onclick = function(e) {
			console.log(e.type);         //弹出事件类型
			e.preventDefault();          //取消事件默认行为
			e.stopPropagation();         //取消事件进一步捕获或冒泡
		}
	</script>




![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-ac923784889549ba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




若是注释掉e.stopPropagation(); 在点击a的时候，由于事件冒泡，box的click事件也会触发，但是调用这句后，事件会停止传播。




>IE中的事件对象






访问IE中的event对象有几种不同的方式，取决于指定事件处理程序的方法。直接为DOM元素添加事件处理程序时，event对象作为window对象的一个属性存在


	<div id="box">
		按钮
	</div>
	<script type="text/javascript">
		function handler() {
			var e = window.event;
			alert(e.type);
		}
		var box = document.getElementById('box');
		box.onclick = handler;
	</script>



我们通过window.event取得了event对象，并检测到了其类型，可是如果事件处理程序是通过attachEvent添加的，那么就会有一个event对象被传入事件处理程序中


	var handler = function(e) {
		alert(e.type);
	}
	var box = document.getElementById('box');
	attachEvent(box, handler);



当然这时候也可以通过window对象访问event，方便起见，我们一般会传入event对象，IE中所有的事件都包含以下属性方法


**属性/方法**|**类型**|**读/写**|**说明**
----|------|----
cancelBulle|Boolean|读/写|默认为false，设置为true后可以取消事件冒泡
returnValue|Boolean|读/写|默认为true，设为false可以取消事件默认行为
srcElement|Element|只读|事件的目标元素
type|String|只读|被触发的事件类型



>跨浏览器的事件对象


虽然DOM和IE的event对象不同，但基于它们的相似性，我们还是可以写出跨浏览器的事件对象方案


	<div id="box">
		<a id="box_a" href="http://www.huanghanlian.com/">按钮</a>
	</div>
	<script type="text/javascript">

		var box = document.getElementById('box');
		var box_a = document.getElementById('box_a');

		box.onclick=function(){
			alert('我是整个父盒子');
		}

		box_a.onclick=function(e){
			var e = getEvent(e);    //为了兼容所有浏览器调用getEvent(e)返回event或者window.event
			//e=e||window.event;
			alert(getTarget(e));    //弹出事件的目标元素
			preventDefault(e);      //取消事件默认行为
			stopPropagation(e)      //可以取消事件冒泡
		};
		//返回event或者window.event
		function getEvent(e) {
			return e || window.event;
		}
		//事件的目标元素
		function getTarget(e) {
			return e.target || e.scrElement;
		}
		//取消事件默认行为
		function preventDefault(e) {
			if (e.preventDefault)
				e.preventDefault();
			else
				e.returnValue = false;
		}
		//可以取消事件冒泡
		function stopPropagation(e) {
			if (e.stopPropagation)
				e.stopPropagation();
			else
				e.cancelBubble = true;
		}


	</script>



>针对兼容，我们可以对这些功能进行封装，以便后期更好的调用。


    //跨浏览器事件处理程序
    var eventutil={
        //添加句柄
        addhandler:function(element,type,handler){
            if(element.addEventListener){                                   //判断是否支持dom2级
                element.addEventListener(type,handler,false);               //支持就运行dom2级事件处理程序
            }else if(element.attachEvent){                                  //判断是否支持ie事件处理
                element.attachEvent('on'+type,handler);                     //支持就运行ie事件处理程序   事件类型需要加'on'
            }else{                                                          //如果都不是就运行dom0级处理程序
                element['on'+type]=handler;
                //连接属性的时候,所有用到'.'的地方都可以用[]中括号
                //element.onclick===element['onclick']
            }
        },
        //删除句柄
        removehandler: function(element, type, handler) {
            if (element.removeEventListener) {
                element.removeEventListener(type, handler, false);
            } else if (element.detachEvent) {
                element.detachEvent('on' + type, handler);
            } else {
                element['on' + type] = null;
            }
        },
        //获取事件
        getEvent: function(event) {
            return event ? event : window.event;
        },
        //取消事件默认行为
        preventDefault: function(event) {
            if (event.preventDefault) {
                event.preventDefault();
            } else {
                event.returnValue = false;
            }
        },
        //阻止事件冒泡
        stopPropagation: function(event) {
            if (event.stopPropagation) {
                event.stopPropagation();
            } else {
                event.cancelBubble = true;
            }
        },
        //獲取事件屬性
        gitType: function(event) {
            return event.type;
        },
        //獲取事件的目标元素
        getElement: function(event) {
            return event.target || event.scrElement;
        }
    }




>针对这篇文章，做了个小[demo](https://github.com/huanghanzhilian/DOM_Event_mystery)。欢迎Star。