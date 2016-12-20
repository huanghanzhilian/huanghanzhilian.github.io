---
layout: post
title: JavaScript 精粹 基础 进阶(9)OOP面向对象编程(下)
subtitle: JavaScript 精粹 基础 进阶(9)OOP面向对象编程(下)
author: 继小鹏
date: 2016-12-20 11:55:03 +0800
category: JavaScript
tag: __proto__
---
我们怎么去模拟重载，在javasceipr中我们可以通过参数的类型区别或者数量的区别，来去让同样一个函数名字，可以根据不同的参数列表的情况来去调用相应的函数。


javascript中函数类型是不确定的，并且参数的个数也是可以任意的，那么我们可以通过判断实际传入的参数的个数，来去做一个模拟的重载，


###OOP(模拟重载、链式调用、模块化) 


>模拟重载



	function person() {
		var args = arguments;
		if (typeof args[0] === 'object' && args[0]) {
			if (args[0].name) {
				this.name = args[0].name;
			}
			if (args[0].age) {
				this.age = args[0].age;
			}
		} else {
			if (args[0]) {
				this.name = args[0];
			}
			if (args[1]) {
				this.age = args[1];
			}
		}
	};
	person.prototype.toString = function() {
		return "姓名:" + this.name + "年龄:" + this.age
	}


	var peng = new person({
		name: "继小鹏",
		age: 23
	});
	console.log(peng.toString()); //姓名:继小鹏年龄:23

	var peng1 = new person("是你", 23);
	console.log(peng1.toString()); //姓名:是你年龄:23




>调用子类方法


**例子1**

	if (!Object.create) {
	    Object.create = function(proto) {
	        function F() {};
	        F.prototype = proto;
	        return new F();
	    };
	}

	function person(name) {//基类
		this.name=name;
	}
	person.prototype.init=function(){
		console.log("你好"+this.name)
	}

	function student(name,classname){   //学生类
		this.classname=classname;
		person.call(this,name);
	}

	student.prototype = Object.create(person.prototype);
	student.prototype.constructor = student;


	student.prototype.init=function(){
		console.log("你好s"+this.name)
	}




	var peng=new student("继小鹏","class2");
	console.log(peng);
	peng.init();




**例子2子类调用基类方法**


	function person(name) {//基类
		this.name=name;
	}

	function student(name,classname){   //学生类
		this.classname=classname;
		person.call(this,name);
	}

	person.prototype.init=function(){
		console.log(this.name)
	}

	student.prototype.init=function(){
		person.prototype.init.apply(this,arguments);
	}

	var peng=new student("继小鹏","class2");
	console.log(peng);
	peng.init();



>链式调用


	function classman() {}
	classman.prototype.addClass = function(str) {
		console.log('calss' + str + 'added');
		return this;
	}
	var mang = new classman();
	mang.addClass('classA').addClass('classB').addClass('classC')

	// calssclassAadded
	// calssclassBadded
	// calssclassCadded


使用jq的时候$("#id").addClass('df')
选择器做些操作后在继续addClass('df')还可以再做动作一层层链式去调用。


例子解释


	function classman() {}   //现定义一个构造器classman
	classman.prototype.addClass = function(str) {   //给classman构造器prototype添加addClass属性方法
		console.log('calss' + str + 'added');   //输出表示添加一个class
		return this;  //return this表示返回classman的实例因为返回了实例那么紧接着后面不需要加mang.addClass('classA')直接后面加.addClass('classB').addClass('classB')就可以，每次执行完都会返回实例
	}
	var mang = new classman();
	mang.addClass('classA').addClass('classB').addClass('classC')

	// calssclassAadded
	// calssclassBadded
	// calssclassCadded



>抽象类


	function Detectorlse() {
		throw new Error("Abstract class can not be invoked directly!");
	}
	Detectorlse.detect = function() {
		console.log('Detcetion starting...');
	}
	Detectorlse.stop = function() {
		console.log('Detector stopped');
	}
	Detectorlse.init = function() {
		throw new Error("Error");
	}

	function linkDetector() {};
	linkDetector.prototype = Object.create(Detectorlse.prototype)
	linkDetector.prototype.constructor = linkDetector;

	//...add methods to LinkDetector...


>defineProperty(ES5)



	function Person(name) {
		Object.defineProperty(this, 'name', {
			value: name,
			enumerable: true
		});
	};
	Object.defineProperty(Person, 'arms_num', {
		value: 2,
		enumerable: true
	});
	Object.seal(Person.prototype);
	Object.seal(Person);

	function student(name, classname) {
		this.classname = classname;
		Person.call(this, name);
	};
	student.prototype = Object.create(Person.prototype);
	student.prototype.constructor = student;

	var peng = new Person('继小鹏');
	console.log(peng);

	var han = new student("汗", "class2");
	console.log(han);




>模块化


定义简单模块化


	var moduleA;
	moduleA=function(){
		var prop=1;
		function func(){};
		return {
			func:func,
			prop:prop
		}
	}();


定义简单模块化2

	var moduleA;
	moduleA = new function() {
		var prop = 1;

		function func() {};
		this.func = func;
		this.prop = prop;
	}();




###实践（探测器）