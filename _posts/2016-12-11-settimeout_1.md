---
layout: post
title: setTimeout()和setInterval() 何时被调用执行
subtitle: setTimeout()和setInterval() 何时被调用执行
author: 继小鹏
date: 2016-12-11 05:53:25 +0800
category: JavaScript
tag: BOM
---
setTimeout()和setInterval()经常被用来处理延时和定时任务。**setTimeout**() 方法用于在指定的毫秒数后调用函数或计算表达式,而**setInterval**()则可以在每隔指定的毫秒数循环调用函数或表达式，直到clearInterval把它清除。

####定义



从定义上我们可以看到两个函数十分类似，只不过前者执行一次，而后者可以执行多次，两个函数的参数也相同，第一个参数是要执行的code或句柄，第二个是延迟的毫秒数。

很简单的定义，使用起来也很简单，但有时候我们的代码并不是按照我们的想象精确时间被调用的，很让人困惑

####简单示例

看个简单的例子,简单页面在加载完两秒后，写下Delayed alert!

    setTimeout('document.write("Delayed alert!");', 2000);

看起来很合理，我们再看个setInterVal()方法的例子


	var num = 0;
	var i = setInterval(function() {
		num++;
		var date = new Date();
		document.write(date.getMinutes() + ':' + date.getSeconds() + ':' + date.getMilliseconds() + '<br>');
		if (num > 10)
			clearInterval(i);
	}, 1000);


页面每隔1秒记录一次当前时间（分钟：秒：毫秒），记录十次后清除，不再记录。考虑到代码执行时间可能记录的不是执行时间，但时间间隔应该是一样的，看看结果



	43: 38: 116
	43: 39: 130
	43: 40: 144
	43: 41: 158
	43: 42: 172
	43: 43: 186
	43: 44: 200
	43: 45: 214
	43: 46: 228
	43: 47: 242
	43: 48: 256

####为什么


时间间隔几乎是1000毫秒，但不精确，这是为什么呢？原因在于我们对JavaScript定时器存在一个误解，**JavaScript其实是运行在单线程的环境中的**，这就意味着定时器仅仅是**计划**代码在未来的某个时间执行，而具体执行时机是不能保证的，因为页面的生命周期中，不同时间可能有其他代码在控制JavaScript进程。在页面下载完成后代码的运行、事件处理程序、Ajax回调函数都是使用同样的线程，实际上浏览器负责进行排序，指派某段程序在某个时间点运行的**优先级**。


我们把效果放大一下看看，添加一个耗时的任务


	function test() {
		for (var i = 0; i < 500000; i++) {
			var div = document.createElement('div');
			div.setAttribute('id', 'testDiv');
			document.body.appendChild(div);
			document.body.removeChild(div);
		}
	}
	setInterval(test, 10);
	var num = 0;
	var i = setInterval(function() {
		num++;
		var date = new Date();
		document.write(date.getMinutes() + ':' + date.getSeconds() + ':' + date.getMilliseconds() + '<br>');
		if (num > 10)
			clearInterval(i);
	}, 1000);


我们又加入了一个定时任务，看看结果

	47: 9: 222
	47: 12: 482
	47: 16: 8
	47: 19: 143
	47: 22: 631
	47: 25: 888
	47: 28: 712
	47: 32: 381
	47: 34: 146
	47: 35: 565
	47: 37: 406

这下效果明显了，差距甚至都超过了3秒，而且差距很不一致。

我们可以可以把JavaScript想象成在时间线上运行。当页面载入的时候首先执行的是页面生命周期后面要用的方法和变量声明和数据处理，在这之后JavaScript进程将等待更多代码执行。当进程空闲的时候，下一段代码会被触发

除了主JavaScript进程外，还需要一个在进程下一次空闲时执行的代码队列。随着页面生命周期推移，代码会按照执行顺序添加入队列，例如当按钮被按下的时候他的事件处理程序会被添加到队列中，并在下一个可能时间内执行。在接到某个Ajax响应时，回调函数的代码会被添加到队列。**JavaScript中没有任何代码是立即执行的，但一旦进程空闲则尽快执行。**定时器对队列的工作方式是当特定时间过去后将代码插入，这并不意味着它会马上执行，只能表示它尽快执行。

知道了这些后，我们就能明白，如果想要精确的时间控制，是不能依赖于JavaScript的setTimeout函数的。



####重复的定时器

使用 setInterval() 创建的定时器可以使代码循环执行，到有指定效果的时候，清除interval就可以，如下例


	var my_interval = setInterval(function() {
		if (condition) {
			//..........
		} else {
			clearInterval(my_interval);
		}
	}, 100);


但这个方式的问题在于定时器的代码可能在代码再次被添加到队列之前还没有执行完成，结果导致循环内的判断条件不准确，代码多执行几次，之间没有停顿。不过JavaScript已经解决这个问题，当使用setInterval（）时，仅当没有该定时器的其他代码实例时才将定时器代码插入队列。这样确保了定时器代码加入到队列的最小时间间隔为指定间隔。

这样的规则带来两个问题

1. 某些间隔会被跳过
2. 多个定时器的代码执行之间的间隔可能比预期要小

为了避免这两个缺点，我们可以使用setTimeout（）来实现重复的定时器


	setTimeout(function() {
		//code
		setTimeout(arguments.callee, interval);
	}, interval)



这样每次函数执行的时候都会创建一个新的定时器，第二个setTimeout（）调用使用了agrument.callee 来获取当前实行函数的引用，并设置另外一个新定时器。这样做可以保证在代码执行完成前不会有新的定时器插入，并且下一次定时器代码执行之前至少要间隔指定时间，避免连续运行。


	setTimeout(function() {
		var div = document.getElementById('moveDiv');
		var left = parseInt(div.style.left) + 5;
		div.style.left = left + 'px';
		if (left < 200) {
			setTimeout(arguments.callee, 50);
		}
	}, 50);


这段定时器代码每次执行的时候，把一个div向右移动5px，当坐标大于200的时候停止。