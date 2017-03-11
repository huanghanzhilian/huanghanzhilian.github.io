---
layout: post
title: JavaScript 精粹 基础 进阶(8)OOP面向对象编程(上)
subtitle: JavaScript 精粹 基础 进阶(8)OOP面向对象编程(上)
author: 继小鹏
date: 2016-12-20 11:54:56 +0800
category: JavaScript
tag: proto
---
面向对象编程，oop并不是针对与javascript，很多语言都实现了oop这样一个编程发法论，比如说java，c++，都是实现了oop的语言。


###概念与继承


>概念

面向对象程序设计(Object-oriented programming OOP)是一种程序设计范型，同时也是一种程序开发的方法。对象指的是类的实例，它将对象作为程序的基本单元，将程序和数据封装其中，以提高软件的重用性，灵活性和扩展性。       来自于 ----维基百科



OOP重点的一些特性：

- 继承

- 封装 

- 多态  

- 抽象


>基于原型的继承



	function Foo() {
		this.y = 2;
	};
	Foo.prototype.x = 1;
	console.log(Foo.prototype); //object
	var obj1 = new Foo();
	console.log(obj1.y); //2
	console.log(obj1.x); //1


函数声明创建`Foo()`函数，这个函数就会有一个内置的`Foo.prototype`，并且这个属性是对象，并且是预设的。


	function Foo() {
		this.y = 2;
	};

	console.log(Foo.prototype); //object



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-81f6d095f48ed6dd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


	function Foo() {
		this.y = 2;
	};
	Foo.prototype.x = 1;
	console.log(Foo.prototype); //object

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-91e78a44dcce6ce6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后我们把`Foo.prototype`对象增加一个属性`x`并且赋值为1。

	function Foo() {
		this.y = 2;
	};
	Foo.prototype.x = 1;
	console.log(Foo.prototype); //object
	var obj1 = new Foo();
	console.log(obj1.y); //2
	console.log(obj1.x); //1 

然后使用`new`操作符`new Foo();`来创建一个`Foo();`的实例，叫obj1，

当时用`new`去调用函数的时候，那么构造器也就是说这样一个函数就会作为一个构造器来使用，并且this会指向一个对象，而对象的原型会指向构造器的`Foo.prototype`属性。obj1实际上会成为Foo构造器中的this，最后会作为返回值，并且在构造器里面调用的时候会把y赋值为2，并且obj1的原型，也就是他的proto会指向Foo.prototype内置属性，最后可以看到obj.y会返回2，obj.x会返回1，y是这个对象上的直接量，而x是原型链上的，也就是Foo.prototype的


	function Foo() {
		this.y = 2;
	};
	Foo.prototype.x = 1;
	console.log(Foo.prototype);
	var obj1 = new Foo();

	console.log(obj1);


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-568a72e297d75aeb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




>prototype属性与原型


	function Foo() {};
	console.log(Foo.prototype);
	Foo.prototype.x=1;
	var obj=new Foo();



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-f788c71f7d0e532f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



使用函数声明去中创建一个函数的时候，这个函数就会有一个`prototype`属性，并且他默认会有两个属性。

- 一个是`constructor:Foo`constructor属性会指向它本身Foo。

- 另外一个属性是`__proto__`，`__proto__`是`Foo.prototype`的原型，那么他的原型会指向`Object.prototype`也就是说一般的对象比如用花括号括起来的对象字面量，他也会有`__proto__`他会指向`Object.prototype`因此`Object.prototype`上面的一些方法比如说`toString`，`valueOf`才会被每一个一般的对象所使用，

`x:1`这个是我通过赋值语句增加的。

**这句是Foo.prototype的结构  **


也就是说，这里面有一个`Foo`函数，这个`Foo`函数呢会有一个`prototype`的对象属性，他的作用呢就是在当使用`new Foo()`去构造Foo的实例的时候，那么构造器的`prototype`的属性，会用作`new`出来的这些对象的原型。


所以要搞清楚`prototype`和原型是两回事。


`prototype`是函数对象上的预设的对象属性，而原型呢是我们对象上的一个原型，原型通常都是他的构造器的`prototype`属性。



**实现class继承另外一个class**




	function Person(name, age) {
		this.name = name;
		this.age = age;
	};

	Person.prototype.hi = function() {
		console.log('Hi my name is' + this.name + ",I'm" + this.age + "years old new");
	};

	Person.prototype.legs_num = 2;
	Person.prototype.arms_num = 2;
	Person.prototype.walk = function() {
		console.log(this.name + "is walking...");
	};

	function Student(name, age, className) {
		Person.call(this, name, age);
		this.className = className;
	};
	Student.prototype = Object.create(Person.prototype);
	Student.prototype.constructor = Student;


	Student.prototype.hi = function() {
		console.log('Hi my name is' + this.name + ",I'm" + this.age + "years old new,and form" + this.className + ".");
	};

	Student.prototype.learn = function(subject) {
		console.log(this.name + 'is learing' + subject + 'at' + this.className + '.');
	};

	console.log(Student.prototype);

	var t = new Student("黄继鹏", 23, "class2");
	t.hi();
	console.log(t.legs_num);
	t.walk();
	t.learn('math')



这里有一个函数Person，人的意思意思是说只要人类的class。  在这个构造函数里面，通过`this.name = name;`和`this.age = age;`去做一个赋值，如何Person作为函数直接去调用的话，那么这里的this会指向全局对象，在浏览器里就会指向window，使用new去调用Person函数的时候，this会指向一个原型为Person.prototype的一个空对象，然后通过this.name去给这个空对象赋值，最后这里没有写返回值，使用new会this会作为返回值。


通过`Person.prototype.hi`来创建所有Person实例共享的方法。


再创建Student函数，学生这样一个class，那么学生是也是人，他是可以继承人的，每一个学生也是人，并且学生会有他的班级名称或者一些其他的功能方法，


	function Student(name, age, className) {
		Person.call(this, name, age);
		this.className = className;
	};

创建Student函数，这里多传了一个className参数，在Student函数也算是子类里先调用下父类，Person.call(this, name, age);然后把this作为Person里面的this再把name和age传进去，注意这里的this在new被实例的时候会是这个实例的返回值也就是直接量，

`this.className = className;`并且把Student的实例做好赋值，




**把Student.prototype能继承Person.prototype的一些方法**

	Student.prototype = Object.create(Person.prototype);


使用这样一个方法去拿到`Person.prototype`对象作为原型的值，这样`Student.prototype`原型就会有`Person.prototype`的值了。

如果去掉`Object.create()`的话。人有一些方法，但是学生也有自己的一些方法，`Student.prototype = Person.prototype;`，`Person.prototype;`赋值给`Student.prototype`的时候，当我想增加学生自己的方法时，比如说



	Student.prototype.learn = function(subject) {
		console.log(this.name + 'is learing' + subject + 'at' + this.className + '.');
	};


这样的话由于他们指向的是同一个对象，给`Student.prototype`增加对象的时候同时也给`Person.prototype;`增加了同样的属性，这不是我们想要的。


所以说通过`Student.prototype = Object.create(Person.prototype);`创建了一个空的对象，而这个空对象的原型指向了`Person.prototype`

这样的话我们既可以在访问`Student.prototype`的时候，可以向上查找`Person.prototype`同时可以在不影响`Person.prototype`的前提下创建一些自己的`Student.prototype`上的方法。


	Student.prototype.constructor = Student;

每一个`prototype`属性对象都会有一个`constructor `属性，他的值是指向函数本身，实际上这里面没有太大的用处，因为我们可以任意的去修改，但是为了保证一致性，我们把这个改成`Student.prototype.constructor = Student;`



**面向对象例子测试**


	function Person(name, age) {
		this.name = name;
		this.age = age;
	};

	Person.prototype.hi = function() {
		console.log('Hi my name is' + this.name + ",I'm" + this.age + "years old new");
	};

	Person.prototype.legs_num = 2;
	Person.prototype.arms_num = 2;
	Person.prototype.walk = function() {
		console.log(this.name + "is walking...");
	};

	function Student(name, age, className) {
		Person.call(this, name, age);
		this.className = className;
	};
	Student.prototype = Object.create(Person.prototype);
	Student.prototype.constructor = Student;


	Student.prototype.hi = function() {
		console.log('Hi my name is' + this.name + ",I'm" + this.age + "years old new,and form" + this.className + ".");
	};

	Student.prototype.learn = function(subject) {
		console.log(this.name + 'is learing' + subject + 'at' + this.className + '.');
	};


	var t = new Student("黄继鹏", 23, "class2");
	var poi=new Person("李汉",22);
	console.log(poi);
	console.log(t);



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-43c16c3c2dc86632.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)





###再谈原型链


>再谈原型链



	function Person(name, age) {
		this.name = name;
		this.age = age;
	};

	Person.prototype.hi = function() {
		console.log('Hi my name is' + this.name + ",I'm" + this.age + "years old new");
	};

	Person.prototype.legs_num = 2;
	Person.prototype.arms_num = 2;
	Person.prototype.walk = function() {
		console.log(this.name + "is walking...");
	};

	function Student(name, age, className) {
		Person.call(this, name, age);
		this.className = className;
	};
	Student.prototype = Object.create(Person.prototype);
	Student.prototype.constructor = Student;


	Student.prototype.hi = function() {
		console.log('Hi my name is' + this.name + ",I'm" + this.age + "years old new,and form" + this.className + ".");
	};

	Student.prototype.learn = function(subject) {
		console.log(this.name + 'is learing' + subject + 'at' + this.className + '.');
	};

	console.log(Student.prototype);

	var peng = new Student("黄继鹏", 23, "class2");
	peng.hi();
	console.log(peng.legs_num);
	peng.walk();
	peng.learn('math')




![dfg.png](http://upload-images.jianshu.io/upload_images/3877962-37d17481b52e9489.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这个图里面说明了上面代码例子的示意图

通过`var peng = new Student("黄继鹏", 23, "class2");`来创建了一个实例`peng`。

`peng`的实例他的原型我们用`__proto__`表示，就会指向构造器`Student.prototype`的属性。

`Student.prototype`上面有`hi`和`learn `方法，`Student.prototype`是通过`Student.prototype = Object.create(Person.prototype);`构造的，所以说`Student.prototype`是一个对象，并且的原型指向`Person.prototype`。


`Person.prototype`。也给她设置了很多属性，`hi`...等。



	Person.prototype.hi = function() {
		console.log('Hi my name is' + this.name + ",I'm" + this.age + "years old new");
	};


`Person.prototype.hi`其实是内置的普通对象，内置对象他本身也会有他的原型，他的原型就是`Object.prototype`。

也就是因为这样所以说随便一个对象才会有`hasOwnProperty`，`valueOf`，`toString`等一些公共的函数，这些函数都是从`Object.prototype`而来的。


当我们去调用


`peng.hi();`方法的时候，首先看这个对象上本身有没有`hi`方法，在本身没有所以会像上查找，差遭到`peng`原型也就是`Student.prototype`有这样一个函数方法，所以最终调用的是`Student.prototype`上面的`hi`方法。

如果`Student.prototype`不去写`hi`方法的时候`peng.hi();`会去调用`Person.prototype.hi`这样一个方法，

当我们调用`peng.walk();`的时候，先找`peng`上发现没有，然后`Student.prototype`上面，也没有，`Person.prototype`有`walk`所以最终调用结果是`Person.prototype`上面的`walk`方法。

那么我想去调用`peng.toString`的时候也是一层一层向上查找。找到`Object.prototype`那么最后到`null`也就是最根源没有了。



**关于一切的一般对象都会指向`Object.prototype`做一个实际的实验**


	var obj={x:1,y:2}

比如用`obj`等于一个花括号空的字面量，给他属性。

那么我们知道`obj`就是一个普通的对象，`obj.x就为1`，

可以通过`obj.__proto__`这样的机制可以让你去访问对象的原型


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-b5d9a4c69be98364.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


除了`obj.__proto__`以外，在es5里面提供了一个方法能够返回一个对象的原型，就是`Object.getPrototypeOf(obj)`这样一个方法，可以返回对象原型，



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-d719db594dfb0459.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


通过三个等号来判断`Object.getPrototypeOf(obj)`是不是等于`Object.prototype`



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-702aed1426ec6170.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

返回`true`





也就是说我们随便一个对象仔面了也好或者是函数函数内置的`prototype`属性然后去判断 他的原型可以看到也是`Object.prototype`



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-58ffd9189b45da83.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



也正因为如此所以说我们才可以调用`obj.toString()`，`obj.valueOf()`
实际上这些方法都是取自`Object.prototype`上的，


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-4f6ae91520f49f88.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



**并不是所有对象最终原型链上最终都有`Object.prototype`**

比如说我们创建obj2对象，然后用`var obj2=Object.create(null)`，`obj2.create(null)`的作用是创建空对象，并且这个对象的原型指向这样一个参数，但是这里参数是null，obj2这个时候他的原型就是`undefined`，`obj2.toString`就是`undefined`那么通过`Object.create(null)`创建出来的对象，就没有`Object.prototype`的一些方法。所以说并不是所有的对象都继承`Object.prototype`

只是一般我们对象字面量或者是函数的`prototype`预制的一般的对象上都有`Object.prototype`



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-dea28a1efdb65a25.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




**并不是所有的函数对象都有prototype这样一个预制属性的**



	function abc() {};
	console.log(abc.prototype);
	var hh = abc.bind(null);
	console.log(hh.prototype);



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-622fd9144fe05236.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


使用es5友谊和`bind`函数，`bind`函数是用来修改函数在运行时的`this`的，`bind`函数返回的也是一个函数，但是`bind`函数就没有`prototype`预设属性。



###prototype属性


javascript中的`prototype`原型，不像java的class，是一旦写好了以后不太容易去动态改变的，但是javascript中原型实际上也是普通的对象，那么意味着在程序运行的阶段我们也可以动态的给`prototype`添加或者删除一些属性，



	function Person(name, age) {
	    this.name = name;
	    this.age = age;
	};

	Person.prototype.hi = function() {
	    console.log('Hi my name is' + this.name + ",I'm" + this.age + "years old new");
	};

	Person.prototype.legs_num = 2;
	Person.prototype.arms_num = 2;
	Person.prototype.walk = function() {
	    console.log(this.name + "is walking...");
	};

	function Student(name, age, className) {
	    Person.call(this, name, age);
	    this.className = className;
	};
	Student.prototype = Object.create(Person.prototype);
	Student.prototype.constructor = Student;


	Student.prototype.hi = function() {
	    console.log('Hi my name is' + this.name + ",I'm" + this.age + "years old new,and form" + this.className + ".");
	};

	Student.prototype.learn = function(subject) {
	    console.log(this.name + 'is learing' + subject + 'at' + this.className + '.');
	};

	console.log(Student.prototype);

	var peng = new Student("黄继鹏", 23, "class2");
	peng.hi();
	console.log(peng.legs_num);
	peng.walk();
	peng.learn('math')

	Student.prototype.x=101;
	console.log(peng.x);//101

	Student.prototype={y:2};
	console.log(peng.y);//undefined
	console.log(peng.x);//101

	var nunnly=new Student("nunnly", 23, "class3");

	console.log(nunnly.y);//2
	console.log(nunnly.x);//undefined



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-d78de6b3ef231ccb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



	Student.prototype.x=101;
	console.log(peng.x);//101

	Student.prototype={y:2};
	console.log(peng.y);//undefined
	console.log(peng.x);//101

	var nunnly=new Student("nunnly", 23, "class3");

	console.log(nunnly.y);//2
	console.log(nunnly.x);//undefined


比如说这里的`Student.prototype`同过`Student.prototype.x=101;`把`huang`的原型动态的添加一个属性`x`那么我们发现所有的实例都会受到影响，现在去调用`console.log(peng.x);`发现他赋值为101了，

直接修改Student.prototype={y:2};构造器的属性，把他赋值为一个新的对象，`y:2`。

有趣的现象

	console.log(peng.y);//undefined
	console.log(peng.x);//101

当我们去修改`Student.prototype`的时候并不能修改已经实例化的对象，也就是说已经实例化的`peng`他的原型已经指向当时的`Student.prototype`如果你修改了`Student.prototype`的话，并不会影响已经创建的实例，之所以修改的x没有问题，是因为我们修改的是`peng`原型的那个对象，

但是再去用new重新实例化对象，那么会发现x不见了，并且y是新的y值。



>内置构造器的prototype


	Object.prototype.x = 1;
	var obj = {
		y: 3
	};
	console.log(obj.x); //1
	for (var key in obj) {
		console.log(key + "=" + obj[key]); //y=3 x=1
	}


比如说我们想让所有的对象他的原型链上都会有x属性会发现所有对象都会有x属性，这样的设置会在for...in的时候会被枚举出来，那么怎么解决这个问题呢




	Object.defineProperty(Object.prototype, 'x', {writable: true,value: 1});
	var obj = {
		y: 3
	};
	console.log(obj.x); //1
	for (var key in obj) {
		console.log(key + "=" + obj[key]); //y=3
	}



- value:属性的值给属性赋值
- writable:如果为false，属性的值就不能被重写。
- get: 一旦目标属性被访问就会调回此方法，并将此方法的运算结果返回用户。
- set:一旦目标属性被赋值，就会调回此方法。
- configurable:如果为false，则任何尝试删除目标属性或修改属性以下特性（writable, configurable, enumerable）的行为将被无效化。
- enumerable:是否能在for...in循环中遍历出来或在Object.keys中列举出来。





>创建对象-new/原型链



	function foo(){}    //定义函数对象 foo
	foo.prototype.z = 3;      //函数对象默认带foo.prototype对象属性  这个对象会作为new实例的对象原型  对象添加z属性=3

	var obj =new foo();    //用构造器方式构造新的对象
	obj.y = 2;    //通过赋值添加2个属性给obj
	obj.x = 1;   //通过new去构造这样一个对象他的主要特点是，他的原型会指向构造器的foo.prototype属性

	//一般foo.prototype对象他的原型又会指向Object.prototype
	//Object.prototype他也会有他的原型最后指向null整个原型链的末端

	obj.x; // 1  //访问obj.x发现对象上有x返回1
	obj.y; // 2  //访问obj.y发现对象上有x返回2
	obj.z; // 3  //obj上没有z并不会停止查找，会去查找他的原型foo.prototype.z返回3
	typeof obj.toString; // ‘function'  这是一个函数，toString是Object.prototype上面的每个对象都有
	'z' in obj; // true     obj.z是从foo.prototype继承而来的，所以说obj里面有z
	obj.hasOwnProperty('z'); // false   表示z并不是obj直接对象上的，而是对象原型链上的。




![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-a2fcce85ebf5bb1c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)





###instanceof


>instanceof


`instanceof`数据类型判断方法


	console.log([1, 2] instanceof Array); //true
	console.log(new Object() instanceof Array); //false


左边要求是一个对象`instanceof`右边要求是一个函数或者说构造器
他会判断右边的构造器的 `prototype`的属性是否出现在左边这个对象的原型链上。


	console.log([1, 2] instanceof Array); //true


`[1,2]`这里是数组字面量，数组的字面量他也有他的原型，他的原型就是`Array.prototype`所以返回`true`。


	console.log(new Object() instanceof Array); //false

`new Object()`new一个空对象空对象的原型会指向`Object.prototype`。`new Object()`的原型链不是`Array.prototype`所以返回`false`


需要注意的是

    console.log([1, 2] instanceof Object); //true

因为数组他的原型是Array.prototype，而Array.prototype的原型就是Object.prototype，所以返回true



所以说**instanceof**我们可以判断某一个对象他的原型链上是否有右边这个函数构造器的prototype对象属性。



	function per() {};

	function sor() {};
	sor.prototype = new per();
	sor.prototype.constructor = sor;

	var peng = new sor();
	var han = new per();
	console.log(peng instanceof sor); //true
	console.log(peng instanceof per); //true
	console.log(han instanceof sor); //false
	console.log(han instanceof per); //true



###实现继承的方式


>实现继承的方式


	if (!Object.create) {
		Object.create = function(proto) {
			function F() {};
			F.prototype = proto;
			return new F();
		};
	}


	function Person(name, age) {
		this.name = name;
		this.age = age;
	};

	Person.prototype.hi = function() {
		console.log('Hi my name is' + this.name + ",I'm" + this.age + "years old new");
	};

	Person.prototype.legs_num = 2;
	Person.prototype.arms_num = 2;
	Person.prototype.walk = function() {
		console.log(this.name + "is walking...");
	};

	function Student(name, age, className) {
		Person.call(this, name, age);
		this.className = className;
	};
	Student.prototype = Object.create(Person.prototype);
	Student.prototype.constructor = Student;


	Student.prototype.hi = function() {
		console.log('Hi my name is' + this.name + ",I'm" + this.age + "years old new,and form" + this.className + ".");
	};

	Student.prototype.learn = function(subject) {
		console.log(this.name + 'is learing' + subject + 'at' + this.className + '.');
	};

	console.log(Student.prototype);

	var t = new Student("黄继鹏", 23, "class2");
	t.hi();
	console.log(t.legs_num);
	t.walk();
	t.learn('math');



`Object.create()`也有他的问题，他是es5之后才支持的，但是没有关系在es5之前我们可以写一个模拟的方法。


	if (!Object.create) {
		Object.create = function(proto) {
			function F() {};
			F.prototype = proto;
			return new F();
		};
	}


这里面我们可以判断下有没有`Object.create`如果没有的话，我们可以把他赋值为一个函数，这里会传进来一个参数，写一个临时的空函数，把空函数的prototype属性赋值给想要作为原型的对象，然后返回new F()，会创建一个对象，这个对象的原型指向构造器的prototype，利用这样的规则返回空对象，并且对象原型指向参数也就是要继承的原型。