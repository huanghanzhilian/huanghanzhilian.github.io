---
layout: post
title: JavaScript创建对象
subtitle: JavaScript创建对象
author: 继小鹏
date: 2016-12-05 17:54:14 +0800
category: JavaScript
tag: JavaScript
---
JavaScript 有Date、Array、String等这样的内置对象，功能强大使用简单，人见人爱，但在处理一些复杂的逻辑的时候，内置对象就很无力了，往往需要开发者自定义对象。

####对象是什么

从JavaScript定义上讲对象是无序属性的集合，其属性可以包含基本值、对象或函数。也就是说对象是一组没有特定顺序的属性，每个属性会映射到一个值上，是一组键值对，值可以是数据或对象。


####最简单的对象

JavaScript的一对花括号{}就可以定义一个对象，这样的写法实际上和调用Object的构造函数一样

    var obj={};
    var obj2=new Object();


这样构建出来的对象仅仅包含一个指向Object的prototype的指针，可以使用一些valueOf、hasQwnProperty等方法，没有多大实际作用，自定义对象嘛总要有一些自定义的属性、方法神马的。

	var bool = new Boolean(0);
	document.write(bool.valueOf());
	//false

JavaScript valueOf() 方法


	var obj = {};             //定义对象obj
	obj.a = 0;                //动态赋值字符串属性a值为0
	obj.fn = function() {     //动态赋值字符串属性fn值为匿名函数    属性值为函数表达式
		console.log(this);
	}

	var obj2 = {
		a: 0,                 //使用字面量赋值方法
		fn: function() {
			alert(this);
		}
	}


可以在定义完对象后通过”.”为其添加属性和方法，也可以使用字面量赋值方法在定义对象的时候为其添加属性和方法，这样创建的对象，其方法和属性可以直接使用对象引用，类似于类的静态变量和静态函数，这样创建对象有一个明显缺陷——在定义大量对象的时候很费力，要一遍遍的写几乎是重复的代码。


####抽象一下

既然是重复代码就可以抽象出来，用函数来做这些重复工作，在创建对象的时候调用一个专门创建对象的方法，对于不同的属性值只需要传入不同参数即可。

	function createObj(a, fn) {              //函数声明  带有参数
		var obj = {};                        //定义对象obj  局部变量
		obj.a = a;                           //动态赋值字符串属性a值为穿参a
		obj.fn = fn;                         //动态赋值字符串属性fn值为穿参fn
		return obj;                          //函数返回obj对象
	}

	var obj = createObj(2, function() {      //声明obj变量值为createObj函数再传参  obj等于createObj返回的obj对象
		alert(this.a);
	});


这样在创建大量对象的时候，就可以通过调用此方法来做一些重复工作了，这种方式也不完美，因为在很多时候需要判断对象的类型，上面代码创建出来的对象都是最原始的Object对象实例，只是拓展了一些属性和方法。


####有型一些

又是function登场的时候，JavaScript中function就是个对象，在创建对象的时候打可以抛开上面createObj方法，直接使用function作为对象，怎么实现复用呢，这就在于function作为对象的特殊性了。

1.  function可以接受参数，可以根据参数来创建相同类型不同值的对象

2.  function作为构造函数（通过new操作符调用）的时候会返回一个对象    
构造函数的返回值分为两种情况，当function没有return语句或者return回一个基本类型（bool,int,string,undefined,null）的时候，返回new创建的一个匿名对象，该对象即为函数实例；如果function体内return一个引用类型对象（Array,Function,Object等）时，该对象会覆盖new创建的匿名对象作为返回值。

3.  那么使用function怎么解决类型识别问题呢，每个function实例对象都会有一个constructor属性（也不是“有”，而是可以对应），这个属性就可以指示其构造是谁，也可以使用instanceof 操作符来做判断对象是否为XXX的实例。



	function Person(name) {                                  //函数声明  带有参数
		this.name = name;                                    //this指向函数本身this.name等于传参
		this.fn = function() {                               //this.fn等于函数表达式
			alert(this.name);                                //弹窗this.name属性值
		}
	}

	var person1 = new Person('Byron');                       //通过new操作符调用  new创建的一个匿名对象

	console.log(person1)
	console.log(person1.constructor == Person); //true       //每个function实例对象都会有一个constructor属性  这个属性就可以指示其构造是谁
	console.log(person1 instanceof Person); //true           //通过instanceof判断对象类型   是否为Person的实例。



这样就完美了吧，也不是！虽然构造函数可以使对象有型，但对象的每个实例中的方法都要重复一遍！


	function Person(name) {
		this.name = name;
		this.fn = function() {
			alert(this.name);
		}
	}

	var person1 = new Person('Byron');
	var person2 = new Person('Frank');

	console.log(person1.fn == person2.fn); //false

看看看，虽然两个实例的fn一模一样，但是却不是一回事儿，这如果一个function对象有一千个方法，那么它的每个实例都要包含这些方法的copy，很让内存无语啊。

####来个实例

究竟有没有一种近乎完美的构造对象的方式，及不用做重复工作，又有型，对象通用方法又不必重复？其实可以发现使用function已经距离要求和接近了，只差那么一点儿——需要一个所有function对象的实例共享的容器，在这个容器内存发实例需要共享的属性和方法，正好这个容器是现成的——prototype原型


	function Person(name) {
		this.name = name;
	}

	Person.prototype.share = [];

	Person.prototype.printName = function() {
		alert(this.name);
	}

	var person1 = new Person('Byron');
	var person2 = new Person('Frank');

	console.log(person1.printName == person2.printName); //true



这样每个Person的实例都有自己的属性name，又有所有实例共享的属性share和方法printName，基本问题都解决了，对于一般的对象处理就可以始终这个有型又有爱的创建对象模式了。