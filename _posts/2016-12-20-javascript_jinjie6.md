---
layout: post
title: JavaScript 精粹 基础 进阶(6)函数和作用域（函数、this）
subtitle: JavaScript 精粹 基础 进阶(6)函数和作用域（函数、this）
author: 继小鹏
date: 2016-12-20 11:54:41 +0800
category: JavaScript
tag: __proto__
---
函数是一块JavaScript代码，被定义一次，但可执行调用多次，js中的函数也是对象，所以js函数可以像其他对象那样操作和传递所以我们也常叫js中的函数为函数对象。

####函数概述



函数的构成主要有几个部分`函数名`,`参数列表`,`函数体`


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-1c7cba2619ce442d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


	function foo(x, y) {
		if (typeof x === 'number' && typeof y === 'number') {
			return x + y;
		} else {
			return 0;
		}
	}
	var cdr = foo(1,2);
	console.log(cdr);//3


需要注意的是函数的返回值是依赖与`return `语句的，如果没有`return `语句，默认会在所有代码执行以后返回一个`undefined`。那么这是一般的函数调用。

	function foo(x, y) {
		if (typeof x === 'number' && typeof y === 'number') {} else {

		}
	}
	var cdr = foo(1, 2);
	console.log(cdr);//undefined



如果是作为构造器，外部使用`new`去调用的话，如果没有`return `语句，或者`return `是基本类型的话，那么会将`this`作为返回


	function foo(x, y) {
		if (typeof x === 'number' && typeof y === 'number') {} else {
			return x+y;
		}
		this.x=x+y;
	}
	var cdr = new foo(1, 2);
	console.log(cdr.x);//3




>重点

**this**

函数在不同的调用方式下他的`this`指向是不一样的，并且不同的调用方式下也会有一些细微的差别。





**arguments**

函数里面有一个特殊的对象，叫`arguments`他和参数是有一定的连带关系的。

	function foo(a,b) {
		return arguments;
	}
	var cdr=foo(1,2)
	console.log(cdr);


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-ffcd8c9d1f9b1758.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



**不同的调用方式**

直接调用|对象方法|构造器|call/apply/bind
---|---|---|---
foo()|o.method()|new Foo()|func.call(o);



####函数声明与表达式

创建函数有不同的方式，常见的两种就是**函数声明**和**函数表达式**

>函数声明

	function foo(a, b) {
	    a=+a;
	    b=+b;
	    if (isNaN(a)||isNaN(b)) {   //isNaN() 函数用于检查其参数是否是非数字值。是数字返回true，非数字返回false
	    	return;
	    }
	    return a+b;
	}
	var cdr = foo(1, 2);
	console.log(cdr);//3


>函数表达式

把一个匿名函数赋值给一个变量，这种就是函数表达式

	var add = function(a, b) {
		//do sth
	}


需要注意的是函数声明的函数可以在函数前调用

	add();
	function add() {
		console.log(1);//1
	}

但是函数表达式会报错`Uncaught TypeError: add is not a function`

	add();
	var add = function() {
		alert(1)
	}


>函数表达式-立即执行函数表达式


把一个匿名函数用一个括号()括起来，然后再去直接调用，这种函数定义的方式呢，也叫作函数表达式，并且是立即执行函数表达式。

	(function() {
		console.log(1); //1
	})();




>return 函数

我们也可以将函数对象，作为一个返回值，因为函数也是对象。这种形式也是函数表达式。

	return function(){
		//do sth;
	};



>命名式函数表达式

这种并不常见，同样赋值给给一个函数，但是这个函数不是匿名函数，而是有一个名字的函数。这就是命名式函数表达式

	var add = function add() {
		//do sth
	};


除了函数声明和函数表达式创建函数以外，还有一种不常见的一种创建函数对象的方式。就是函数构造器`Function构造器`

>Function构造器

函数构造器就是大写的`Function`，

	var add = new Function ('a','b','console.log(a+b);');
	add(1,2);//3

	var add = Function ('a','b','console.log(a+b);');
	add(1,2);//3


可以通过`new`或者直接调用的方式 运行，他们俩基本没什么区别，

前面两个参数是函数对象里面的行参，最后一个参数表示函数体里面的代码。

用参数是字符串会带来安全上的一些隐患。


Function构造器去创建的函数里面，创建的变量仍然是局部变量，也可以使用立即调用。

	Function ('var lop="local";console.log(lop);')();//Function构造器去创建的函数并且立即调用
	console.log(lop);//lop是局部变量外部无法访问会报错




在立即调用函数中有lo变量，他的内部有一个df函数声明函数这个函数内可以拿到lo变量

	(function() {
		var lo = "lo";

		function df() {
			console.log(lo)
		}
		df();
	})();
    //输出lo


但是在Function构造器中lo却访问不到。
在立即调用函数里，lo不可访问，全局变量l可以访问。所以说Function构造器很少使用

	var l="l";
	(function(){
		var lo="lo";
		Function ('console.log(l);console.log(lo);')();
	})()

	//l
	//Uncaught ReferenceError: lo is not defined(…)




**比一比三种创建函数的方式**


|         | 函数声明 | 函数表达式 | 函数构造器  |
| :-------- | :--------:| :--: |:----:|
| 前置| √ |    |   |
| 允许匿名|    |  √  |√|
| 立即调用|    |  √  |√|
| 在定义该函数的作用域通过函数名访问| √ |    |   |
| 没有函数名|    |      |√|



- 前置：只有函数声明会被前置，就好像被拉倒了最前面，说以说可以在函数声明的位置之前，去调用这样一个函数。
但是函数表达式和函数构造器是代码执行阶段，才会去创建对应的函数对象，所以不会被前置。

- 允许匿名：函数表达式和函数构造器都是允许匿名的，事实上函数构造器只能是匿名的是不可以有名字的，但是函数声明他的名字是不可以省略的，省略在比较新的浏览器会报错。

- 立即调用：函数声明是不能被立即调用的，如果你尝试把函数声明后面写一个括号，那么会报一个异常，因为函数声明被提前解析，并且前置放到最前了，所以会报错。函数表达式和函数构造器因为不会前置，所以表达式计算结果是一个函数对象的时候我们可以加一对括号把他立即去调用。

-  在定义该函数的作用域通过函数名访问：比如说`fuction fd(){}`在function同级作用域下，可以通过`fd()`去调用这个函数，但是函数表达式和函数构造器就不可以这样。

- 没有函数名：事实上函数构造器只能是匿名的是不可以有名字的



####[JavaScript]this


在不同的环境下，同一个函数在不同的调用方式下，这个 `this`都有可能是不同的。


>全局作用域下的this

全局作用域的 `this`一般指向全局对象，那么在浏览器里面这个全局对象就是`window`


	console.log(this.document === document); //true
	console.log(this === window); //true


可以看到`this.document `等价于`window.document`
`this`等价于`window`



	this.a = 37;
	console.log(window.a); //37

给全局对象添加一个属性a，这样就相当于创建了一个全局变量a，并且赋值为37。


>一般函数的this(浏览器)

	function f1() {
		return this;
	}
	console.log(f1() === window); //true

	var f1 = function() {
		return this;
	}
	console.log(f1() === window); //true


用函数声明或者函数表达式，返回this，这里的this指向全局对象。




	var f2 = function() {
		"use strict";
		return this;
	}
	console.log(f2()); //undefined

需要注意的是再严格模式下那么默认一般情况下this会指向


>作为对象方法的函数的this

	var o = {
		prop: 37,
		f: function() {
			return this.prop;
		}
	};
	console.log(o.f()); //37



创建了一个对象字面量`o`，`o`里面有一个属性`f`值呢是一个函数对象，对于一个把函数作为对象属性值的方式，叫做对象的方法，作为对象的方法去调用`o.f()`这种情况下`this`会指向对象`o`所以`o.f()`被调用返回37。


	var o = {
		prop: 37
	};

	function indep() {
		return this.prop;
	}
	o.f = indep;
	console.log(o.f());//37


定义对象`o`只有一个属性`prop`赋值为37
这里买你有一个独立的`indep`函数，函数体`return`一个`this.prop;`如果直接去调用`indep()`那么这个this会指向`window`。

如果用赋值方式`o.f`给`o`对象创建属性`f`值为`indep`函数对象，那么这样去调用我们任然能够拿到37，`console.log(o.f());//37`。



>对象原型链上的this



`Object.create({x:1});`是系统内置的函数，这个函数会接收一个参数，一般是一个对象。他会返回一个新创建的对象，并且让这个对象的原型指向参数，参数一般是个对象。

	var o = {
		f: function() {
			return this.a + this.b;
		}
	};
	var p = Object.create(o);
	p.a = 1;
	p.b = 4;
	console.log(p);



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-d44fa743806b5fa9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

原型链上有f方法



	var o = {
		f: function() {
			return this.a + this.b;
		}
	};
	var p = Object.create(o);
	p.a = 1;
	p.b = 4;
	console.log(p.f());//5


创建o对象属性f值是一个函数对象， 

通过`var p = Object.create(o);`创建了一个对象`p`,这个对象`p`是一个空的对象，并且他的原型会指向`o`,使用`p.a`给对象p添加属性值为1`p.b`值为4，这样就创建在对象上的属性，那么我去调用p原型上的方法的时候，`this.a + this.b;`任然能取到对象上的a和b。



>get/set方法与this


	function modulus() {
		return Math.sqrt(this.re * this.re + this.im * this.im);
	};

	var o = {
		re: 1,
		im: -1,
		get phase() {
			return Math.atan2(this.im, this.re);
		}
	};

	Object.defineProperty(o, 'modulus', {
		get: modulus,
		enumerable: true,
		configurable: true
	});

	console.log(o.phase, o.modulus);//-0.7853981633974483 1.4142135623730951


>构造器中的this

	function MyClass() {
		this.a = 23;
	};
	var o = new MyClass();
	console.log(o.a); //23


如果正常去调用一个函数的话`MyClass()`那么这个函数的`this`会指向`window`但是如果我用`new`实例化构造函数去调用，那么这里面的`this`会指向这样一个空的对象，并且这个对象的原型会指向`MyClass.prototype`
最后这个this.a会作为返回值，因为这里没有`return`所以默认`this`会作为返回值，所以这个对象o就会就会有a属性值23。





	function c2() {
		this.a = 23;
		return {
			a: 24
		};
	}
	var f = new c2();
	console.log(f.a); //24

c2函数this等于23，但是这一次`return`返回了一个对象，那么这个a就不再是23了，而是返回的这个对象，o.a属性就变成了24。


如果是作为构造器，外部使用new去调用的话，如果没有return语句，或者return是基本类型的话，那么会将this作为返回



>call/apply方法与this

	function add(c, d) {
		return this.a + this.b + c + d;
	};

	var o = {
		a: 1,
		b: 3
	};

	var h=add.call(o, 5, 7);//1+3+5+7
	console.log(h)

	add.apply(o, [10, 20]);//1+3+10+20

	function bar() {
		console.log(Object.prototype.toString.call(this));
	};

	bar.call(7);//[object Number]


函数add，这里面返回`this.a + this.b + c + d;`把这四个数相加起来，定义o对象有两个属性a和b，通过对象`call`方法，`.call(o, 5, 7)`第一个参数是你想作为`this`的这样一个对象，5和7是想要添加的参数，5赋值给`function add(c, d)`的c，7赋值给d；最终的结果就是`1+3+5+7`，
`add.call(o, 5, 7)`和`add.apply(o, [10, 20])`基本没什么差别，只是`apply`是数组作为传参的。


>bind方法与this


	function f() {
		return this.a;
	};

	var g = f.bind({
		a: "test"
	});
	console.log(g()); //test

	var o = {
		a: 23,
		f: f,
		g: g
	};
	console.log(o.f(), o.g()); //23 "test"


在EcmaScript5中扩展了叫bind的方法（IE6,7,8不支持）

这里有个函数对象`f`,通过`bind`方法，发现`f.bind({	a: "test"});`有一个参数，这个参数是一个对象，这个对象就是你想要将某一个对象作为`this`的时候那就把这样一个对象穿进去，那么我们拿到一个新的`g`对象，新的`g`对象在调用的时候`this`已经指向了`bind`的参数，这对于我们需要去绑定一次重复调用仍然实现绑定，这样会比`call`,`apply`会更加高效点。

还可以看到，这里有一个`o`然后把a赋值为23，f属性赋值为f函数，g赋值绑定之后的方法，输出f的时候能拿到23，一般的函数根据调用方式来判断，他是通过对象属性去调用的，那么这里买你的this就会指向`o`那么就会拿到a：23。这里面比较特殊的我使用`bind`的方法去绑定了之后，即使我们把 新绑定之后的方法作为对象的属性去调用，仍然会按照之前的绑定去走。所以返回"test"



####[JavaScript]函数属性arguments





>函数属性 & arguments


接触函数属性和方法



	function foo(a, b, d) {
		console.log(arguments);
		console.log(arguments.length); //2
		console.log(arguments[0]); //1
		arguments[1] = 10;
		console.log(b); //change to 10;

		arguments[2] = 100;
		console.log(d); //即使手动赋值任然是undefined. 没有赋值arguments[2]=100;d等于undefined
		console.log(arguments.callee === foo);
	}
	foo(1, 2);
	console.log(foo.length);
	console.log(foo.name);
	console.dir(foo);


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-327ae46a1e186720.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


定义函数`foo`
通过`foo.name`获取函数名
通过`foo.length`获取函数行参个数
通过`arguments.length`获取函数实际传参的个数

有一个问题。`foo(1, 2);`传参只有两个参数，实际上`d`是没有传进来的， 这种情况尝试去`arguments[2] = 100;`去赋值的时候对应的`d`仍然是`undefined`


**严格模式下**

	function foo(a, b, d) {
		'use strict'
		console.log(arguments.length); //2
		console.log(arguments[0]); //1
		arguments[1] = 10;
		console.log(b); //严格模式下仍然是1

		arguments[2] = 100;
		console.log(d); //即使手动赋值任然是undefined. 没有赋值arguments[2]=100;d等于undefined
		console.log(arguments.callee === foo);  //严格模式下arguments.callee是禁止使用
	}
	foo(1, 2);
	console.log(foo.length);
	console.log(foo.name);



>apply/call方法


	function foo(x, y) {
		console.log(x, y, this);
	};
	foo.call(100, 1, 2);					//1 2 Number {[[PrimitiveValue]]: 100}
	foo.apply(true, [3, 4]);				//3 4 Boolean {[[PrimitiveValue]]: true}
	foo.apply(null);						//undefined undefined Window {...}
	foo.apply(undefined);					//undefined undefined Window {...}


定义`foo`函数，有x,y两个属性，输出参数和对应的`this`

对于`call()`来讲第一个参数就是想作为`this`的对象如果不是对象他会转成对象，所以这里的`foo.call(100, 1, 2);	`**100**会转成对应的包装类`Number `值为100。类似的`foo.apply(true, [3, 4]);`**true**会转换成布尔值。
如果第一个参数是`null`,`undefined`的话，那么`this`会指向全局对象，对于浏览器就是`window对象`


**需要注意在严格模式下**


	function foo(x, y) {
		'use strict';
		console.log(x, y, this);
	};
	foo.call(100, 1, 2);					//1 2 100
	foo.apply(true, [3, 4]);				//3 4 true
	foo.apply(null);						//undefined undefined null
	foo.apply(undefined);					//undefined undefined undefined

在严格模式下`this`输出直接量


>bind方法

在EcmaScript5中扩展了叫bind的方法（IE6,7,8不支持）



	this.x = 9; //创建了一个全局变量x并赋值为9
	var mod = {
		x: 81,
		getX: function() {
			return this.x;
		}
	};

	console.log(mod.getX()); //81

	var gitX = mod.getX;
	console.log(gitX());; //9

	var bound = gitX.bind(mod);
	console.log(bound());//81


创建了一个全局变量`x`并赋值为9，然后有一个变量`mod `赋值为一个对象字面量，里面有x属性 值为81，有一个getX属性，值是一个匿名函数，他会返回`this.x`。


如果`对象(点)属性名  mod.getX()`这样的调用方式。会返回这个对象为`this`返回81。

`var gitX = mod.getX;`如果定义一个全局变量gitX赋值为`mod`对象的`getX`属性，这个时候`this`就会指向全局对象，也就是`this.x = 9;`所以返回9，`console.log(gitX());; //9`



通过`bind`方法改变函数运行时的`this`指向，`var bound = gitX.bind(mod);`也就是说`gitX函数在运行时this`为`mod对象`。所以再次输出81。



>bind与currying  科里化功能

函数科里化就是把一个函数拆成多个单元。

	function add(a,b,c){
		return a+b+c;
	};

	var func=add.bind(undefined,100);
	func(1,2);//103

	var func2=func.bind(undefined,200);
	func2(10);//310



这里有一个函数`add`他的作用是把参数abc相加然后作为返回值，

可能有的时候不需要一次把函数都调用完，而是我调用一次把前面两个参数传完了后，得到这样的函数我再去调用，传入第三个值，比如通过add.bind()方法这次不需要改变this，那写个undefined，提高额外参数100，这个我们拿到bind函数以后，相当于这个100就会固定赋值给a第一个参数，在调用`func(1,2);`传入1和2的时候，1就会给b，2就会给c，所以最后的答案就是103。

类似的再去`func.bind()`也就是说绑定了两个参数ab，b传入200，再去调用一次`func2(10)`传入10，那么就是100+200+10，最终结果就是310。

实际例子


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-796ebbd5039095bc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



>bind与new


	function foo() {
		this.b = 100;
		return this.a;
	};

	var func = foo.bind({
		a: 1
	});
	console.log(func()); //1

	var func2 = new func();
	console.log(func2); //{b: 100}

foo函数声明，函数体`this.b=100`和`return this.a;`那如果我直接调用的话，`this.b=100`中的`this`是指向全局对象，所以说相当于创建了一个全部变量`b`并赋值为100，返回全局对象上的`a`属性。

使用`bind()`方法，`var func = foo.bind({	a: 1	});`来传入一个参数，一个字面量对象只有一个属性`a`，直接调用的话`func()`，那么`foo`函数中的`this`就会指向`bind({a:1})`的参数。所以调用`func()`返回1。


如果使用`new`的话就特殊些，使用`new`除非`func()`函数`return  除非是对象`如果不是对象将会把`this`作为返回值，并且`this`会被初始化成一个默认的空对象，这个空对象的原型是`foo.prototype`，所以使用`new`去调用的时候`var func2 = new func();`，即使用了`var func = foo.bind({	a: 1	});`bind方法但是这个this任然会指向没有bind的时候的一样，所以返回值会忽略`return `，返回`this`对象。