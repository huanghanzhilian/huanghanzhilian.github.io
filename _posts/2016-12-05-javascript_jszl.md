---
layout: post
title: JavaScript 精粹整理
subtitle: JavaScript 精粹整理
author: 继小鹏
date: 2016-12-06 12:54:39 +0800
category: JavaScript
tag: JavaScript
---

avaScript 是 弱类型 语言，但并不是没有类型


####数据类型

JavaScript 是 **弱类型** 语言，但并不是没有类型，JavaScript可以识别下面 7 种不同类型的值：

**基本数据类型**


1. Boolean
2. Number
3. String
4. null
5. undefined
6. Symbol


**对象类型Object**


1. Array
2. RegExp
3. Date
4. Math
5. ...


可以使用` typeof `判断数据类型，操作符返回一个字符串，但并非返回的所有结果都符合预期


	typeof false // "boolean"
	typeof .2 // "number"
	typeof NaN // "number"
	typeof '' // "string"
	typeof undefined // "undefined"
	typeof Symbol() // "symbol"

	typeof new Date() // "object"
	typeof [] // "object"

	typeof alert // "function"

	typeof null // "object"
	typeof not_defined_var // "undefined"


####变量

在应用程序中，使用变量来来为值命名。变量的名称称为` identifiers`

**声明**

1. 使用关键字 `var` ：函数作用域
2. 使用关键字 `let` ：块作用域 (block scope local variable)
3. 直接使用：全局作用域               

.



	var global_var = 1;

	function fn() {
		var fn_var = 2;

		if (fn_var > 10) {
			let block_var = 3;
			global_var2 = 4;
		}
	}



只声明不赋值，变量的默认值是 `undefined`

`const` 关键字可以声明不可变变量，同样为块作用域。对不可变的理解在对象上的理解需要注意


	const num = 1;
	const obj = {
		prop: 'value'
	};

	num = 2; // Uncaught TypeError: Assignment to constant variable.
	obj['prop'] = 'value2';

	obj = []; // Uncaught TypeError: Assignment to constant variable.



**变量提升**

JavaScript中可以引用稍后声明的变量，而不会引发异，这一概念称为变量声明提升(**hoisting**)

	console.log(a); // undefined
	var a = 2;

等同于

    var a;
    console.log(a);
    a = 2;


####函数

一个函数就是一个可以被外部代码调用(或者函数本身递归调用)的 `子程序`
定义函数

1. 函数声明
2. 函数表达式
3. Function 构造函数
4. 箭头函数

.


	function fn(){}

	var fn = function(){}

	var fn = new Function(arg1, arg2, ... argN, funcBody)

	var fn = (param) => {}

**arguments**

1. arguments:  一个包含了传递给当前执行函数参数的类似于数组的对象

2. arguments.length: 传给函数的参数的数目

3. ~~arguments.caller: 调用当前执行函数的函数~~

4. ~~arguments.callee: 当前正在执行的函数~~

.

	function foo() {
		return arguments;
	}
	foo(1, 2, 3); // Arguments[3]
	// { "0": 1, "1": 2, "2": 3 }


**rest**


	function foo(...args) {
		return args;
	}
	foo(1, 2, 3); // Array[3]
	// [1, 2, 3]

	function fn(a, b, ...args) {
		return args;
	}

	fn(1, 2, 3, 4, 5); // Array[3]
	// [3, 4, 5]


**default**

函数的参数可以在定义的时候约定默认值

	function fn(a = 2, b = 3) {
		return a + b;
	}


	fn(2, 3); // 5

	fn(2); // 5

	fn(); // 5


####对象

JavaScript 中对象是可变`键控集合`(**keyed collections**)

**定义对象**

1. 字面量
2. 构造函数


.

	var obj = {
		prop: 'value',
		fn: function() {}
	};

	var date = new Date();

**构造函数**

构造函数和普通函数并没有区别，使用`new`关键字调用就是构造函数，使用构造函数可以 **实例化** 一个对象

函数的返回值有两种可能

1. 显式调用`return`返回`return`后表达式的求值

2. 没有调用`return`返回`undefined`

.



	function People(name, age) {
		this.name = name;
		this.age = age;
	}

	var people = new People('Byron', 26);
	console.log(people)



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-55949d1a871cc863.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


构造函数返回值

1. 没有返回值
2. 简单数据类型
3. 对象类型


前两种情况构造函数返回构造对象的实例，**实例化**对象正是利用的这个特性

第三种构造函数和普通函数表现一致，返回`return`后表达式的结果


**prototype**


1. 每个函数都有一个 prototype 的对象属性，对象内有一个 constructor 属性，默认指向函数本身
2. 每个对象都有一个 __proto__ 的属性，属相指向其父类型的 prototype            

.



	function Person(name) {
		this.name = name;
	}

	Person.prototype.print = function() {
		console.log(this.name);
	};

	var p1 = new Person('Byron');
	var p2 = new Person('Casper');

	p1.print();
	p2.print();
	console.log(p1)



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-f37571c1c251b9e9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


####this 和作用域

作用域可以通俗的理解

1. 我是谁
2. 我有哪些马仔

其中我是谁的回答就是 this

马仔就是我的局部变量

**this 场景**

**普通函数**


1. 严格模式：`undefined`
2. 非严格模式: 全局对象
3. Node: `global`
4. 浏览器: `window`

**构造函数**：对象的实例

**对象方法**：对象本身

**call & apply**

1. fn.call(context, arg1, arg2, …, argn)
2. fn.apply(context, args)


.



	function isNumber(obj) {
		return Object.prototype.toString.call(obj) === '[object Number]';
	}


**Function.prototype.bind**

`bind` 返回一个新函数，函数的作用域为 `bind` 参数


	function fn() {
		this.i = 0;

		setInterval(function() {
			console.log(this.i++);
		}.bind(this), 500)
	}

	fn();


**() => {}**

箭头函数是 **ES6** 提供的新特性，是简写的 **函数表达式**，拥有词法作用域和`this`值

	function fn() {
		this.i = 0;

		setInterval(() => {
			console.log(this.i++);
		}, 500)
	}

	fn();


####继承

在 JavaScript 的场景，继承有两个目标，子类需要得到父类的：

1. 对象的属性
2. 对象的方法

.



	function inherits(child, parent) {
		var _proptotype = Object.create(parent.prototype);
		_proptotype.constructor = child.prototype.constructor;
		child.prototype = _proptotype;
	}

	function People(name, age) {
		this.name = name;
		this.age = age;
	}

	People.prototype.getName = function() {
		return this.name;
	}

	function English(name, age, language) {
		People.call(this, name, age);
		this.language = language;
	}

	inherits(English, People);

	English.prototype.introduce = function() {
		console.log('Hi, I am ' + this.getName());
		console.log('I speak ' + this.language);
	}

	function Chinese(name, age, language) {
		People.call(this, name, age);
		this.language = language;
	}

	inherits(Chinese, People);

	Chinese.prototype.introduce = function() {
		console.log('你好，我是' + this.getName());
		console.log('我说' + this.language);
	}

	var en = new English('Byron', 26, 'English');
	var cn = new Chinese('色拉油', 27, '汉语');

	en.introduce();
	cn.introduce();



**ES6 class 与继承**


	"use strict";

	class People {
		constructor(name, age) {
			this.name = name;
			this.age = age;
		}

		getName() {
			return this.name;
		}
	}

	class English extends People {
		constructor(name, age, language) {
			super(name, age);
			this.language = language;
		}

		introduce() {
			console.log('Hi, I am ' + this.getName());
			console.log('I speak ' + this.language);
		}
	}

	let en = new English('Byron', 26, 'English');

	en.introduce();


####语法

**label statement**



	loop:
		for (var i = 0; i < 10; i++) {
			for (var j = 0; j < 5; j++) {
				console.log(j);
				if (j === 1) {
					break loop;
				}
			}
		}

	console.log(i);


**语句与表达式**

	var x = {
		a: 1
	};

	{
		a: 1
	}

	{
		a: 1,
		b: 2
	}


**立即执行函数**

	( function() {}() );
	( function() {} )();
	[ function() {}() ];

	~ function() {}();
	! function() {}();
	+ function() {}();
	- function() {}();

	delete function() {}();
	typeof function() {}();
	void function() {}();
	new function() {}();
	new function() {};

	var f = function() {}();

	1, function() {}();
	1 ^ function() {}();
	1 > function() {}();



####高阶函数


高阶函数是把函数当做参数或者返回值是函数的函数


**回调函数**

	[1, 2, 3, 4].forEach(function(item) {
		console.log(item);
	});


**闭包**

闭包由两部分组成

1. 函数
2. 环境：函数创建时作用域内的局部变量

.


	function makeCounter(init) {
		var init = init || 0;

		return function() {
			return ++init;
		}
	}

	var counter = makeCounter(10);

	console.log(counter());
	console.log(counter());


**典型错误**


	for (var i = 0; i < doms.length; i++) {
		doms.eq(i).on('click', function(ev) {
			console.log(i);
		});
	}

                 

	for (var i = 0; i < doms.length; i++) {
		(function(i) {
			doms.eq(i).on('click', function(ev) {
				console.log(i);
			});
		})(i);
	}





**惰性函数**

	function eventBinderGenerator() {
		if (window.addEventListener) {
			return function(element, type, handler) {
				element.addEventListener(type, hanlder, false);
			}
		} else {
			return function(element, type, handler) {
				element.attachEvent('on' + type, handler.bind(element, window.event));
			}
		}
	}

	var addEvent = eventBinderGenerator();


**柯里化**



一种允许使用部分参数生成函数的方式


	function isType(type) {
		return function(obj) {
			return Object.prototype.toString.call(obj) === '[object ' + type + ']';
		}
	}

	var isNumber = isType('Number');

	console.log(isNumber(1));
	console.log(isNumber('s'));

	var isArray = isType('Array');

	console.log(isArray(1));
	console.log(isArray([1, 2, 3]));

           

	function f(n) {
		return n * n;
	}

	function g(n) {
		return n * 2;
	}

	console.log(f(g(5)));

	function pipe(f, g) {
		return function() {
			return f.call(null, g.apply(null, arguments));
		}
	}

	var fn = pipe(f, g);

	console.log(fn(5));


**尾递归**

1. 尾调用是指某个函数的最后一步是调用另一个函数
2. 函数调用自身，称为递归
3. 如果尾调用自身，就称为尾递归

递归很容易发生"栈溢出"错误（stack overflow）


	function factorial(n) {
		if (n === 1) return 1;
		return n * factorial(n - 1);
	}

	factorial(5) // 120


但对于尾递归来说，由于只存在一个调用记录，所以永远不会发生"栈溢出"错误


	function factorial(n, total) {
		if (n === 1) return total;
		return factorial(n - 1, n * total);
	}

	factorial(5, 1) // 120


柯里化减少参数



	function currying(fn, n) {
		return function(m) {
			return fn.call(this, m, n);
		};
	}

	function tailFactorial(n, total) {
		if (n === 1) return total;
		return tailFactorial(n - 1, n * total);
	}

	const factorial = currying(tailFactorial, 1);

	factorial(5) // 120



**反柯里化**


	Function.prototype.uncurry = function() {
		return this.call.bind(this);
	};


push 通用化


	var push = Array.prototype.push.uncurry();

	var arr = [];

	push(arr, 1);
	push(arr, 2);
	push(arr, 3);

	console.log(arr);