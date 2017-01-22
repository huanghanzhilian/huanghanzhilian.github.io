---
layout: post
title: JavaScript 精粹 基础 进阶(4)对象
subtitle: JavaScript 精粹 基础 进阶(4)对象
author: 继小鹏
date: 2016-12-20 11:54:24 +0800
category: JavaScript
tag: __proto__
---
对象中包含一系列属性，这些属性是无序的。每个属性都有一个字符串key和对应的value。


###JavaScript 对象概述


>概述

**对象中包含一系列属性，这些属性是无序的。每个属性都有一个字符串key和对应的value。**


    var obj = {x : 1, y : 2};    //定义obj对象， 有两个属性x和y
    obj.x; // 1   //访问对应obj.x属性取到对应的值
    obj.y; // 2   //访问对应obj.y属性取到对应的值



这里有两个重点，一个是属性是无序的，再一个每一个`key`是字符串。


>探索对象的key


    var obj = {};    //定义对象obj
    obj[1] = 1;      //动态赋值数字1属性值为1
    obj['1'] = 2;    //动态赋值字符串1属性值为2
    console.log(obj); // Object {1: 2}  //实际上结果指向的是同一个属性

    //每个属性都有一个字符串key和对应的value。

    obj[{}] = true;    //[{}] 空对象作为key
    obj[{x : 1}] = true;     //对象带有属性的对象作为key  //js都会把它转换成字符串然后再去处理，最终指向的是同一个属性
    console.log(obj)// Object {1: 2, [object Object]: true}

>回顾-数据类型


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-cf48b65c7a5c38b8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


函数，数组，日期，正则等都是对象

>对象结构

    var obj = {};    //字面量空对象
    obj.y = 2;       //赋值创建属性x
    obj.x = 1;       //赋值创建属性y
    

对象有个特点，他的属性可以动态的添加或删除的



**函数对象**


    function foo(){};
    console.log(foo.prototype);



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-0927720a486ccd98.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




创建一个函数声明      
每一个函数都会有一个`foo.prototype`对象属性


    function foo(){};
    foo.prototype.z=1;
    console.log(foo.prototype);


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-801d690e9bac45d7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


给`foo.prototype`对象属性添加属性`z`赋值为1，`foo.prototype`对象属性下有个`constructor`属性方法，该属性其实是指向自己本身。





    function foo(){};
    foo.prototype.z=1;
    console.log(foo.prototype);

    var obj =new foo();
    console.log(obj);



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-3c12980454ffbf3d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


通过`new`去实例化`foo()`函数`var obj =new foo();`那么这个`obj`原型就会指向构造器的`prototype`属性也就是`foo.prototype`，可以通过上图看出打印输出是一样的。




###JavaScript 创建对象，原型链

>对象创建-字面量

    var obj1 = {x : 1, y : 2};

对象字面量 ，用{}设置属性

    var obj2 = {
        x : 1,
        y : 2,
        o : {
            z : 3,
            n : 4
        }
    };

对象字面量也可以做一些嵌套，比如某些属性值可以又是对象


>创建对象-new/原型链

还有一种就是使用`new`构造器的方法创建对象，先来了解javascript中的原型链。

什么是原型链

    function foo(){}    //定义函数对象  
    foo.prototype.z = 3;      //函数对象默认带foo.prototype对象属性  对象添加z属性=3

    var obj =new foo();    //用构造器方式构造新的对象
    obj.y = 2;    //通过赋值添加2个属性给obj
    obj.x = 1;   //通过new去构造这样一个对象他的主要特点是，他的原型会指向构造器的foo.prototype属性

    obj.x; // 1  //访问obj.x发现对象上有x返回1
    obj.y; // 2  //访问obj.y发现对象上有x返回2
    obj.z; // 3  //obj上没有z并不会停止查找，会去查找他的原型foo.prototype.z返回3
    typeof obj.toString; // ‘function'  这是一个函数，toString是Object.prototype上面的每个对象都有
    'z' in obj; // true     obj.z是从foo.prototype继承而来的，所以说obj里面有z
    obj.hasOwnProperty('z'); // false   表示z并不是obj直接对象上的，而是对象原型链上的。


但是如果是赋值的话结果就一样了。


比如我们赋值`obj.z=5`

如果给`obj.z`尝试去赋值，就不会像原型链上去查找了。先看`obj.z`有没有，有的话修改它的值，没有的话，在对象上添加`obj.z`。


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-5279a12733aad48b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


    obj.z = 5;
    obj.hasOwnProperty('z'); // true
    foo.prototype.z; // still 3
    obj.z; // 5

    obj.z = undefined;
    obj.z; // undefined


那么怎样能拿到原型链上的z呢

    delete obj.z; // true  删除对象上的z属性
    obj.z; // 3  //这样就能获取原型链上的z




>对象创建-Object.create

从字面量理解是对象创建



`Object.create({x:1});`是系统内置的函数，这个函数会接收一个参数，一般是一个对象。他会返回一个新创建的对象，并且让这个对象的原型指向参数，参数一般是个对象。

	var obj = Object.create({
		x: 1
	});
	console.log(obj.x) // 1
	console.log(typeof obj.toString) // "function"
	console.log(obj.hasOwnProperty('x')); // false
	console.dir(obj)


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-dff2f50d6c39b7c5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)







###JavaScript 属性操作


>读写对象属性


	var obj = {
		x: 1,
		y: 2
	};
	console.log(obj.x); // 1
	console.log(obj["y"]); // 2

	obj["x"] = 3;
	obj.y = 4;

创建对象字面量`obj`，有`x`和`y`两个属性，可以用`obj.y`(点)操作符去访问他的属性，也可以用`obj["y"]`[中括号这里需要是字符串]


	var obj = {
		x1: 1,
		x2: 2
	};
	var i = 1,
		n = 2;

	for (; i <= n; i++) {
		console.log(obj['x' + i]);
	}
	// 输出: 1, 2


obj['x']使用方法，obj['x']同等于obj.x。obj['x']可以在[]内拼接字符串，obj.x+""不是那么方便





	var obj = {
		x1: 1,
		x2: 2
	};
	var p;
	for (p in obj) {
		console.log(obj[p]);
	}


用for...in遍历所有属性           
需要注意的是`用for...in`去遍历的话，有可能把原型链上的属性也遍历出来，并且他的顺序是不确定的。



>属性读写-异常


	var obj = {
	    x: 1
	};//创建对象obj
	console.log(obj.y); //如果访问一个不存在的属性会进行原型链查找如果找到末端还是找不到就会返回 undefined
	var yz = obj.y.z; //不能获取undefined的属性z   TypeError: Cannot read property 'z' of undefined
	obj.y.z = 2; //不能给undefined的属性z去赋值 TypeError: Cannot set property 'z' of undefined



	var yz;
	if (obj.y) {
	    yz = obj.y.z;
	}//判断当obj.y存在把他取出来


	//巧用运算符
	var yz = obj && obj.y && obj.y.z;




>属性删除



	var person = {
		age: 28,
		title: 'fe'
	};
	delete person.age; // true  表示删除成功
	delete person['title']; // true  表示删除成功
	person.age; // undefined
	delete person.age; // true

	delete Object.prototype; // false,  不允许删除

	//通过getOwnPropertyDescriptor方法去获取一个属性中所有的标签，第一个参数是你要查看的对象，第二个是你要去检测的属性这样就能拿到属性的描述器
	var descriptor = Object.getOwnPropertyDescriptor(Object, 'prototype');
	console.log(descriptor);




>属性检测


	var cat = new Object;  //使用new 构造对象
	cat.legs = 4;    //赋值属性legs值为4
	cat.name = "Kitty";  //赋值属性name值为Kitty

	'legs' in cat; // true   表示cat中有legs属性 in操作符是会向原型链上查找的
	'abc' in cat; // false   表示cat中没有abc属性
	"toString" in cat; // true, 继承 property有toString属性

	cat.hasOwnProperty('legs'); // true   表示cat直接量属性有legs
	cat.hasOwnProperty('toString'); // false   表示cat直接量属性没有toString   toString是在原型有

	cat.propertyIsEnumerable('legs'); // true  legs可枚举  查看是否可枚举propertyIsEnumerable   for...in会被循环
	cat.propertyIsEnumerable('toString'); // false   toString不可枚举  查看是否可枚举propertyIsEnumerable   for...in不会被循环


**自定义对象属性，让他的枚举标签是false**



	var cat = new Object;  //使用new 构造对象
	cat.legs = 4;    //赋值属性legs值为4
	cat.name = "Kitty";  //赋值属性name值为Kitty

	'legs' in cat; // true   表示cat中有legs属性 in操作符是会向原型链上查找的
	'abc' in cat; // false   表示cat中没有abc属性
	"toString" in cat; // true, 继承 property有toString属性

	cat.hasOwnProperty('legs'); // true   表示cat直接量属性有legs
	cat.hasOwnProperty('toString'); // false   表示cat直接量属性没有toString   toString是在原型有

	cat.propertyIsEnumerable('legs'); // true  legs可枚举  查看是否可枚举propertyIsEnumerable   for...in会被循环
	cat.propertyIsEnumerable('toString'); // false   toString不可枚举  查看是否可枚举propertyIsEnumerable   for...in不会被循环

	//通过Object.defineProperty给cat目标对象去添加一个属性price
	//enumerable:是否能在for...in循环中遍历出来或在Object.keys中列举出来。
	//value:属性的值给属性赋值
	Object.defineProperty(cat, 'price', {enumerable: false,	value: 1000});
	cat.propertyIsEnumerable('price'); // false  price不可枚举  查看是否可枚举propertyIsEnumerable   for...in不会被循环
	cat.hasOwnProperty('price'); // true   表示cat直接量属性有price
	console.log(cat)
	for(var k in cat){
		console.log(cat[k]);//循环出来看看能不能被枚举
	}


**判断对象属性是否存在进行操作**


	if (cat && cat.legs) {
	    cat.legs *= 2;
	}//8

判断`cat `是否存在且`cat.legs`不是`undefined`的时候，让`cat.legs`值乘等于2，表示`cat.legs`乘以2再赋值给`cat.legs`


	if (cat.legs !== undefined) {
		// only if cat.legs is not undefined
	}

判断`cat.legs`不等于`undefined`的时候去做动作


	if (cat.legs !== undefined) {
		// only if cat.legs is not undefined
	}


判断`cat.legs`严格不等于`undefined`的时候去做动作


>属性枚举


	var o = {
		x: 1,
		y: 2,
		z: 3
	};
	'toString' in o; // true
	o.propertyIsEnumerable('toString'); // false
	var key;
	for (key in o) {
		console.log(key); // x, y, z
	}

这样写出来的对象原型链属性默认是不可枚举的，所以`for....in`的时候原型链不会出来。






	var o = {
		x: 1,
		y: 2,
		z: 3
	};
	var obj = Object.create(o);  //创建obj对象以o对象作为原型
	obj.a = 4;

	console.log(obj)
	var key;
	for (key in obj) {
		console.log(key); // a, x, y, z
	}


创建`obj`对象以`o`对象作为原型
所有对象上的属性，和原型上的属性都会遍历中显示出来


**那有时候我只想处理对象上的属性，不想遍历我对象原型链上的属性。**

我们需要在加一个`obj.hasOwnProperty(key)`的判断，来过滤掉原型链上的属性就可以了。


	var o = {
		x: 1,
		y: 2,
		z: 3
	};
	var obj = Object.create(o);  //创建obj对象以o对象作为原型
	obj.a = 4;

	var key;
	for (key in obj) {
		if (obj.hasOwnProperty(key)){
			console.log(key); // a
		}
	}




>JavaScript get/set方法


	var man = {
		name: 'huang',
		weibo: '@.com',
		get age() {
			return new Date().getFullYear() - 1993;
		},
		set age(val) {
			console.log('年龄不能设置' + val);
		}
	}
	console.log(man.age); //23
	man.age = 100; //年龄不能设置100
	console.log(man.age); //23


定义了一个对象`man`有属性`name`值是`huang`，这里定义了一对`get`和`set`方法，来去访问属性`age`

语法

用`get`或`set`关键字开头，空格，然后是属性的名字，然后是括号再方括号，里面是一个函数体，

`get`方法会返回当前日期的年份减去我的出生日，会拿到我的年龄。

`set`方法会拿到括号赋值的值，唯一的参数创进来。

当访问`console.log(man.age); //23`会返回`get`方法，

当使用`man.age = 100;`也就是说给`set`去赋值的时候，就会去调用`set`方法，就会输出`年龄不能设置100`

再去输出`console.log(man.age); //23`会发现仍然是23，因为`set`没做任何的事情。




	var man = {
		name: 'huang',
		$age: null,
		get age() {
			if (this.$age == undefined) {
				return new Date().getFullYear() - 1993;
			} else {
				return this.$age;
			}
		},
		set age(val) {
			val = +val;
			if (!isNaN(val) && val > 0 && val < 150) {
				this.$age = +val;
			} else {
				throw new Error('Incorrect=' + val);
			}
		}
	}
	console.log(man.age); //23
	man.age = 100;
	console.log(man.age); //100
	man.age = 'abn';
	console.log(man.age);//Uncaught Error: Incorrect=NaN(…)







>get/set与原型链



	function foo() {};

	Object.defineProperty(foo.prototype, 'z', {
		get: function() {
			return 1;
		}
	});

	var obj = new foo();

	console.log(obj.z); //1
	obj.z = 10;
	console.log(obj.z); //还是输出1

	Object.defineProperty(obj, 'z', {
		value: 100,
		configurable: true
	});

	console.log(obj.z);//100
	delete obj.z;
	console.log(obj.z);//1

解释

	function foo() {};

创建一个`foo函数`，就会有一个`foo.prototype`


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-c0c316829ee86954.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



	Object.defineProperty(foo.prototype, 'z', {
		get: function() {
			return 1;
		}
	});


通过`defineProperty`用`git`方法来创建`z属性`，`git`方法只是固定的返回1。


	var obj = new foo();

使用`new`方式来创建一个新的对象`obj`，这样这个`obj`对象的原型会指向`foo.prototype`



	console.log(obj.z); //1

当我们去访问`obj.z`的时候`obj`对象上没有`z直接量属性`，所以他会向上去查找，`foo.prototype`原型，发现有一个`z属性`用的是`get`方法，这样就能返回1，

	obj.z = 10;
	console.log(obj.z); //还是输出1

那么我赋值，尝试给`z`赋值为10，如果z不是用get方式赋值的是通过`foo.prototype.z`原型赋值的话，再使用直接量去赋值是可以成功的，值会返回到`obj`这个对象上，但是在这里赋值并没有成功。仍然返回1。

这是因为当`obj`对象上没有z直接量属性的时候，并且他的原型链查找发现有`get`和`set`方法的时候，那么当我们尝试去赋值的时候，实际上会走原型上`get`和`set`方法的，而不会再去通过给当前对象`obj`添加新属性。






	Object.defineProperty(obj, 'z', {
		value: 100,
		configurable: true
	});


通过`Object.defineProperty`给`obj`目标对象去添加一个属性`z`



- value:属性的值给属性赋值
- writable:如果为false，属性的值就不能被重写。
- get: 一旦目标属性被访问就会调回此方法，并将此方法的运算结果返回用户。
- set:一旦目标属性被赋值，就会调回此方法。
- configurable:如果为false，则任何尝试删除目标属性或修改属性以下特性（writable, configurable, enumerable）的行为将被无效化。
- enumerable:是否能在for...in循环中遍历出来或在Object.keys中列举出来。


	console.log(obj.z);//100

这个时候obj.z是`obj`的直接量返回100

	delete obj.z;

由于`configurable: true`所以删除是成功的


	console.log(obj.z);//1

这里因为直接量z被删除们说以他会继续去向上查找`z`的`get`方法。




小案例

	var o={z:2};
	Object.defineProperty(o,'x',{value:1});//默认writable=false只读,configurable=false不可写
	o.z=6;
	o.x=6;
	console.log(o);


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-e7b028717c1bb4ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




案例解释

	var o={};
	Object.defineProperty(o,'x',{value:1});//默认writable=false只读,configurable=false不可写
	var obj=Object.create(o);//创建obj对象以o对象作为原型
	console.log(obj.x);//1   会向上查找返回原型链x属性值1
	obj.x=200; //尝试去赋值200
	console.log(obj.x);//赋值后打印任然是1

	//那么如何去在obj对象添加x属性呢？

	//还是要用Object.defineProperty


	//给obj对象添加一个自己的属性x，这样就可以覆盖掉原型链上的不可写的x，writable:true,configurable:true,value:100
	Object.defineProperty(obj,'x',{writable:true,configurable:true,value:100});
	console.log(obj.x);//100   obj.x是直接量属性
	obj.x=500;   //直接量属性可以赋值
	console.log(obj.x);//500   //修改成功


###JavaScropt属性标签


>属性标签

**怎样去看某一个对象上的属性上都有哪些标签**

	console.log(Object.getOwnPropertyDescriptor({pro:true}, 'pro'));检查一个字面量的对象，添加属性`pro:true`看字面量下的属性`pro`的属性标签
	//configurable:true,enumerable:true,value:true,writable:true
	console.log(Object.getOwnPropertyDescriptor({pro:true}, 'a'));//去查一个根本不存在的属性返回undefined
	//undefined


通过`Object.getOwnPropertyDescriptor(参数1,"参数2")`方法可以返回一个对象，这个对象上会显示 当前这个属性下所有的标签，这个函数会接收两个参数，第一个是你要去判断的对象，第二个是一个字符串的属性名，


- value:属性的值给属性赋值
- writable:如果为false，属性的值就不能被重写。
- get: 一旦目标属性被访问就会调回此方法，并将此方法的运算结果返回用户。
- set:一旦目标属性被赋值，就会调回此方法。
- configurable:如果为false，则任何尝试删除目标属性或修改属性以下特性（writable, configurable, enumerable）的行为将被无效化。
- enumerable:是否能在for...in循环中遍历出来或在Object.keys中列举出来



	console.log(Object.getOwnPropertyDescriptor({pro:true}, 'pro'));检查一个字面量的对象，添加属性`pro:true`看字面量下的属性`pro`的属性标签

得到的结果是

	//configurable:true,enumerable:true,value:true,writable:true






**那么怎么去设置或者说设置对象的属性标签呢？**




	var person = {};
	Object.defineProperty(person, 'name', {
		configurable: false,
		writable: false,
		enumerable: false,
		value: "继小鹏"
	});
	console.log(person.name); //继小鹏
	person.price = 100; //修改不了
	console.log(person.name); //还是继小鹏
	console.log(delete person.name); //false  不可删除


解释例子

创建一个空对象

	var person = {};


通过`Object.defineProperty(参数1,"参数2",{参数1})`第一个参数是要添加属性的对象，第二个参数是一个字符串属性的名字，第三个参数是一个对象，这个对象里面就是具体每一个标签的值，

	Object.defineProperty(person, 'name', {
		configurable: false,//不可删除
		writable: false,//属性的值就不能被重写。
		enumerable: true,//可枚举
		value: "继小鹏"//属性的值给属性赋值
	});



结果符合预期


	console.log(person.name); //继小鹏
	person.price = 100; //修改不了
	console.log(person.name); //还是继小鹏
	console.log(delete person.name); //false  不可删除




**创建新的属性使用`Object.keys`**


	var person = {};
	Object.defineProperty(person, 'name', {
		configurable: false,
		writable: false,
		enumerable: true,
		value: "继小鹏"
	});

	Object.defineProperty(person, 'type', {
		configurable: true,
		writable: true,
		enumerable: false,
		value: "继小鹏222"
	});


	console.log(person);

	console.log(Object.keys(person));



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-8f3025f5cc6a3d15.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


新创建的属性`enumerable: false,`不可枚举，可以通过`Object.keys(person)`的方法来去获取对象上所有的`key`
因为新创建的属性`enumerable: false,`不可枚举所有不可见。


**使用Object.defineProperties()定义多个对象属性标签**


	Object.defineProperties(person, {
		title: {
			value: 'fe',
			enumerable: true,
		},
		corp: {
			value: 'SDF',
			enumerable: true,
		},
		salary: {
			value: '50000',
			enumerable: true,
			writable: true
		}
	});


	console.log(person);
	console.log(Object.getOwnPropertyDescriptor(person, 'corp'));
	console.log(Object.getOwnPropertyDescriptor(person, 'salary'));



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-97898b59fe919abb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




`Object.defineProperties()`这个函数接收两个参数，第一个参数是你要定义属性的对象，第二个参数是一个对象，对象里包含属性值列表集。



**例子**


	var person = {};
	Object.defineProperties(person, {
		title: {
			value: 'fe',
			enumerable: true,
		},
		corp: {
			value: 'SDF',
			enumerable: true,
		},
		salary: {
			value: '50000',
			enumerable: true,
			writable: true
		},
		luck: {
			get: function() {
				return Math.random() > 0.5 ? 'good' : 'bad';
			}
		},
		promote: {
			set: function(level) {
				this.salary *= 1 + level * 0.1;
			}
		}
	});



	console.log(Object.getOwnPropertyDescriptor(person, 'salary'));
	console.log(Object.getOwnPropertyDescriptor(person, 'corp'));
	console.log(person.salary);
	person.promote = 3;
	console.log(person.salary);
	console.log(person.luck);




![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-8fe3925d20d034aa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




###JavaScropt对象标签、对象序列化


>对象标签


对象级别也是有标签的，主要有三种

1. [[proto]]

2. [[class]]

3. [[extensible]]



>原型标签`__proto__`


`__proto__`实际上就是原型，比如说当我们用`new`构造器的方式去实例化函数对象，那么这个实例化对象的原型链会指向构造器的`prototype`，一般的对象也会有原型，指向 `Object.prototype`然后在往上就是`nall`。



>class标签


	var toString = Object.prototype.toString;

	function getType(o) {
		return toString.call(o).slice(8, -1);
	};

	console.log(toString.call(null)); //[object Null]

	console.log(getType(null)); //Null
	console.log(getType(undefined)); //Undefined
	console.log(getType(1)); //Number
	console.log(getType(new Number(1))); //Number
	console.log(typeof new Number(1)); //object
	console.log(getType(true)); //Boolean
	console.log(getType(new Boolean(true))); //Boolean


class标签是没有一个直接的方式去查看他或者是修改他，可以间接的通过`Object.prototype.toString;`的方式来去获取class，



	var toString = Object.prototype.toString;

先把` Object.prototype.toString;`这样一个函数拿到，赋值给`toString`这样方便后面代码简短一些，


	function getType(o) {
		return toString.call(o).slice(8, -1);
	};

定义一个方法，返回`toString.call(o)` 来调用这个函数方法，并且把参数`o`作为`this`传进去，然后用`slice(8, -1);`表示是截取从第八个字符开始，一直到最后。

调用`getType(1); //Number`getType(1)方法会返回类型。




>extensible标签

extensible标签表示你这个对象是否可扩展，言外之意就是说对象上的属性是否可以被继续添加。


	var obj={x:1,y:2};//创建对象
	Object.isExtensible(obj);//true


创建对象obj，通过`Object.isExtensible(obj);`来判断对象是否可扩展，一般情况下默认返回true。表示可以扩展。


**那么怎么样不让他扩展，不可修改不可删除冻结对象**


	var obj={x:1,y:2};//创建对象
	Object.isExtensible(obj);//true   obj对象可扩展

	Object.preventExtensions(obj);    //设置obj对象不可扩展
	Object.isExtensible(obj);//false   obj对象不可扩展
	obj.d=2;   //尝试给obj添加新的属性会发现添加失败
	console.log(obj.d);//undefined

	//如果已经组织对象可扩展，那么对象上的属性标签是否会发生变化呢？

	console.log(Object.getOwnPropertyDescriptor(obj, 'x'));
	//value: 1, writable: true, enumerable: true, configurable: true
	//虽然我们阻止了对象不可添加新属性，但是已有熟悉仍然可以修改和删除，也是可以枚举的。

	//可以通过
	Object.seal(obj);//会把对象上的属性的configurable设置为false不可删除
	console.log(Object.getOwnPropertyDescriptor(obj, 'x'));
	//value: 1, writable: true, enumerable: true, configurable: false
	console.log(Object.isSealed(obj));//true   判断对象是否被隐藏不可删除


	Object.freeze(obj);//让对象冻结不可写
	console.log(Object.getOwnPropertyDescriptor(obj, 'x'));
	//value: 1, writable: false, enumerable: true, configurable: false
	console.log(Object.isFrozen(obj));//true   判断对象是否被不可写




>序列化


	var obj = {
		x: 1,
		y: true,
		z: [1, 2, 3],
		nullval: null
	};
	console.log(obj);
	console.log(JSON.stringify(obj)); //{"x":1,"y":true,"z":[1,2,3],"nullval":null}
	var obj1 = {
		val: undefined,
		a: NaN,
		b: Infinity,
		c: Date()
	};
	console.log(obj1);
	console.log(JSON.stringify(obj1)); //{"a":null,"b":null,"c":"Fri Dec 16 2016 22:25:04 GMT+0800 (中国标准时间)"}

	var obj2 = JSON.parse('{"x":1}');
	console.log(obj2); //Object {x: 1}
	console.log(obj2.x); //1

定义obj 字面量对象，有一些属性，通过`JSON.stringify(obj));`方法返回一个字符串`{"x":1,"y":true,"z":[1,2,3],"nullval":null}`返回的字符串就是这个对象的序列化的结果。

需要注意一点这个序列化是有一些坑的，如果你的属性值是`undefined`的话，那么就不会出现在序列化的结果当中。

如果后端返回一个JSON数据我们怎么把他转换成js对象呢？

使用`JSON.parse('{"x":1}');`方法，需要注意的是合法的json属性必须以双引号引起来。



>序列化-自定义


	var obj = {
		x: 1,
		y: 2,
		o: {
			o1: 1,
			o2: 2,
			toJSON: function() {
				return this.o1 + this.o2;
			}
		}
	};

	console.log(JSON.stringify(obj)); //{"x":1,"y":2,"o":3}


obj对象下有个属性o他的值是一个对象，那么这个对象序列化的结果我可能想要自定义，那么我们只需要在这个当前的层级下写一个`toJSON`他的值是一个函数，`toJSON`是固定这个key一定要这样写，然后这个函数会返回`return this.o1 + this.o2;`这个this会指向当前层级的数也就是o，

这时候去`JSON.stringify(obj));`的时候，这里面的o通过`toJSON`这样一个方法来去返回的。



>其他对象方法

	var obj = {   //定义对象obj
		x: 1,
		y: 2
	};
	obj.toString(); //"[object Object]"   调用对象的toString方法会返回"[object Object]"这样的字符串实际上没有太大意义

	//定义自己对象上个的toString方法
	obj.toString = function() {
		return this.x + this.y;    //返回对象的x和y相加作为结果
	}

	"Result" + obj; //"Result3"   左边是字符串这样会理解为字符串拼接那么他就会调用obj.toString所以返回结果"Result3"
	+ obj; //3  用一员加号操作符是可以把对象尝试转换为数字的，如果定义了obj.toString也会去调用obj.toString返回3

	//valueOf是尝试把对象转换为基本类型的时候会自动去调用的一个函数，这里自定义返回值
	obj.valueOf = function() {
		return this.x + this.y + 100;
	} + obj; //103   再去用一员加号操作符把他转换为数字这次返回的结果是103是从这个valueOf而来的
	"Result" + obj; //"Result103"


需要主要的是这里面valueOf和toString都存在的时候那么不管是一元的加号还是2元的字符串拼接在做具体操作时，都会尝试把对象转换为基本类型，那么他会先去找valueOf。如果valueOf返回的是基本类型那么就以valueOf的值作为结果反之如果valueOf不存在或者返回的是对象，那么就会去找toString。如果valueOf和toString都没有或者都返回的对象，那么就会报错。