---
layout: post
title: JavaScript 精粹 基础 进阶(7)函数和作用域（闭包、作用域）
subtitle: JavaScript 精粹 基础 进阶(7)函数和作用域（闭包、作用域）
author: 继小鹏
date: 2016-12-20 11:54:48 +0800
category: JavaScript
tag: proto
---
闭包在JavaScript 中是一个非常重要的概念。

####闭包例子


	function outer() {
		var loc = 30;
		return loc;
	};
	console.log(outer()); //30


`outer`函数是一个函数声明，有一个局部变量`loc `赋值为30，返回`loc `。

当这个函数调用之后，局部变量就会被释放了，


	function outer() {
		var loc = 30;
		return function() {
			return loc;
		};
	};
	var func = outer();
	console.log(func()); //30


这个`outer`函数有一个`loc`的局部变量，返回值是一个匿名函数表达式，在这个函数表达式又返回`outer`函数的`loc`局部变量，这种情况`loc`是不会被释放掉的。

调用`var func = outer();`返回的是一个匿名函数，这个匿名函数里面仍然能够访问到`outer()`de 局部变量`loc `，当`outer()`函数被调用之后，`func()`这个函数再次调用的时候任然能访问到它外层`outer()`函数的局部变量`loc `。
这种情况就是我们通常所说的**闭包**。


**那么闭包的作用是什么呢？在前端编程里，闭包是非常常见的**

>闭包无处不在


	<body>
	<input type="button" value="按钮" id="but">
	<script type="text/javascript">
	var but = document.getElementById("but");
	! function() {
		var loc = 0;
		but.onclick = function() {
			console.log(loc++);
		};
	}();
	</script>
	</body>


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-d26c006fb9866d89.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


	<input type="button" value="按钮" id="but">
	<script type="text/javascript">
	var but = document.getElementById("but");
	! function() {
		var loc = "locc";
		but.addEventListener('click', function() {
			console.log(loc);
		}, false);
	}();
	</script>

比如说我们可能有一些自己的局部变量，或者说我们的函数有一些外函数的变量，我们在做点击事件的时候，这个点击事件可能会用到外层的一些局部变量，有了闭包我们可以在数据的传递上更为灵活。



	! function() {
		var loc = "loc";
		var url = "http://www.huanghanlian.com/";
		$.ajax({
			url: url,
			success: function() {
				//do sth
				console.log(loc);
			}
		});
	}();


有一个异步的请求，用jq的`$.ajax`方法也可以在`success`的回调函数中去用到外层具体的一些变量。

因为闭包的缘故，在最外层匿名函数调用结束后，`success`的回调函数仍然可以访问外层的局部变量。`loc `，`url`




>闭包-常见错误之循环闭包

	document.body.innerHTML = "<div id=a1>aa</div>" + "<div id=a2>bb</div>" + "<div id=a3>cc</div>" + "<div id=a4>dd</div>";
	for (var i = 1; i < 4; i++) {
		document.getElementById("a" + i).addEventListener('click', function() {
			console.log(i);//始终都是4
		}, false);
	};


这里面在网页上面添加三个元素，通过循环来给这三个元素绑定事件，想当点击第一个元素的时候，输出1点击第二个输出2第三个输出3。实现方式是先循环获取元素，给元素增加点击事件，点击事件里面会输出`i`的值。

但是上面的代码始终只会输出4，这其实是闭包的原因，`addEventListener('click', function() {这里是回调函数}`是回调函数，也就是说当点击时回调函数去做你想做的事情， 当我点击的湿乎乎回调函数回去动态的拿到  `i`的值，这个`i`的值在整个初始化过程中完成之后实际上`i`的值就已经是4了，所以始终输出4。


如果想想要实现效果就要用到闭包

	document.body.innerHTML = "<div id=a1>aa</div>" + "<div id=a2>bb</div>" + "<div id=a3>cc</div>";
	for (var i = 1; i < 4; i++) {
		! function(i) {
			document.getElementById("a" + i).addEventListener('click', function() {
				console.log(i);//1,2,3
			}, false);
		}(i);
	};


这个例子里面在每一层循环的时候，用一个匿名函数而且是立即执行的匿名函数给他包装起来，然后将每一次遍历的1.2.3分别的值去传到这个匿名函数里，然后匿名函数接到这个参数`i`再放到点击事件中去引用`i`当我们每次点击事件输出的值`i`就会取每一个闭包环境下的`i`。所以这样就能达到效果。


>闭包-封装

闭包还有个好处就是可以封装一些变量


	(function() {
		var _userId = 23492;
		var _typeId = 'item';
		var expor = {};

		function converter(userId) {
			return +userId;
		};

		expor.getUserId = function() {
			return converter(_userId);
		};

		expor.getTypeId = function() {
			return _typeId;
		};
		window.expor = expor;
	}());
	console.log(expor.getUserId()); //23492
	console.log(expor.getTypeId()); //item
	console.log(expor._userId); //undefined
	console.log(expor._typeId); //undefined


比如说以上代码，有个立即调用函数，他有自己的函数作用域，在里面定义的局部变量外部是不可以访问到的，最后可以通过`window.expor = expor;`这样的方式来去把最终想输出的对象输出出去，那么外部就可以通过`expor `对象上提供的方法，就可以访问到函数里面的对象或变量。


**闭包的概念**


   
>在计算机科学中，闭包(也称词法闭包或函数闭包)是指一个函数或函数的引用，与一个引用环境绑在一起。这个引用环境是一个存储该函数每个非局部变量(也叫自由变量)的表。


>闭包，不同于一般函数，它允许一个函数在立即词法作用域外调用时，仍可访问非本地变量
   





####作用域


>全局，函数，eval  [作用域]

JavaScript中的作用域，实际上是比较简单的。

**全局作用域**

	var a = 10;
	console.log(window.a); //10

在最外层声明变量a，这样就声明了全局作用域下的变量a。


	for (var i = 0; i < 4; i++) {
		console.log(i); //0.1.2.3
	}
	console.log(i); //4


在全局范围内用for循环里面有个`vae i=0`定义了一个变量`i`，可能会误解为这个`i`只能在这个for循环里面可见，对外不可见，实际上这是错误的理解，JavaScript中是没有块级作用域的，也就是说不管是for循环while 循环里面去定义的变量，实际上和在外面定义的变量，也就是for循环所在的外面去定义变量实际上是没有差别的，所以 `i`在外面也能访问到。


**函数作用域**


	(function() {
		var b = 20;
	})();
	console.log(b);//test_lt.html:13 Uncaught ReferenceError: b is not defined(…)

上面匿名立即执行函数表达式，在执行的时候声明局部变量b，在函数外面是拿不到的。函数有自己独立的作用域。




**eval  作用域**

	eval("var a=1;");




>作用域链


	var le3 = 3;

	function out() {
		var le = 1;

		function out2() {
			var le2 = 2;
			console.log(le, le2, le3); //1 2 3
		}
		out2()
	}
	out();


`out()`函数中有一个内部函数`out2()`它里面有一个局部变量`le2`，函数`out2()`能访问到自己的内部变量`le2`，也可以在向上访问`le`变量，也就是所谓的闭包，可以访问外层函数的局部变量，对于函数`out2()`来讲也叫作它的自由变量，还可以访问最外层的`le3 `变量，也就是全局对象，这个作用域手机从内向外都可以访问的。




	function out() {
		var le = 1;
		var func = new Function("var p=0;console.log(p);console.log(le)");
		func();
	}
	out();
	//输出
	//0
	//Uncaught ReferenceError: le is not defined(…)

如果使用`new Function`去构造的一个函数，构造器，去调用构造器定义位置所在的变量`le`的。



>利用函数作用域封装


	(function() {
		//do sth here
		var a, b;
	})();

	! function() {
		//do sth here
		var a, b;
	}();



利用函数作用域的特性，我们经常看到很多类库，或者代码最外层如果没有模块化的一些工具的话，会在最外层去写一个`function`这样一个匿名函数，这样的好处就是可以把函数内部的变量变成函数的局部变量，而不是全局变量，这样防止大量的全局变量和其他的一些类库或者其他代码引发冲突，`! function() {}()`这样的作用其实就是把函数变成函数表达式，而不是函数声明，如果省略掉这个`!`叹号的话，那么他会理解为函数声明，会被前置处理掉，那么最后留下一个括号或者你省略名字的话，都会报语法错误。