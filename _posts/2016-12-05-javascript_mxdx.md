---
layout: post
title: JavaScript面向对象
subtitle: JavaScript面向对象
author: 继小鹏
date: 2016-12-05 17:54:39 +0800
category: JavaScript
tag: JavaScript
---
对象这个词如雷贯耳，同样出名的一句话：XXX语言中一切皆为对象！

对象究竟是什么？什么叫面向对象编程？

####理解对象



>对象（object），台湾译作物件，是面向对象（Object Oriented）中的术语，既表示客观世界问题空间（Namespace）中的某个具体的事物，又表示软件系统解空间中的基本元素。        

>在软件系统中，对象具有唯一的标识符，对象包括属性（Properties）和方法（Methods），属性就是需要记忆的信息，方法就是对象能够提供的服务。在面向对象（Object Oriented）的软件中，对象（Object）是某一个类（Class）的实例（Instance）。 —— 维基百科

对象是从我们现实生活中抽象出来的一个概念，俗话说物以类聚，人以群分，我们也经常说有一类人，他们专业给隔壁家制造惊喜，也就是我们说的老王

这里面就有两个重要概念

1. 类：无论是物以类聚，还是有一类人，这里说的类并不是实际存在的事物，是一些特征、是一些规则等
2. 老王：这是个实物，是现实存在，和类的关系就是符合类的描述

对应到计算机术语，类就是class，定义了一些特点（属性 property）和行为（方法 method），比如说给隔壁制造惊喜的这类人有几个特征

1. 长相文质彬彬，为人和善
2. 姓王

同时这些人还有技能（行为）

1. 帮隔壁修下水道
2. 亲切问候对方儿子

我们刚才就描述了一个类，用代码表示就是

	class LaoWang {
		string name;
		string familyNmae = "wang";
		bool isKind = true;

		LaoWang(string name) {
			this.name = name;
		}

		void fixPipe() {
			statement
		}

		void greetSon() {
			statement
		}
	}

符合这些特点并且有上述行为能力的，我们称之为老王，从描述我们就可以看出来`LaoWang`
不是指某个人，而是指一类人，符合上述描述的都可能是老王！用计算机术语说就是没个活蹦乱跳的老王都是类`LaoWang`的**实例**。用代码描述就是


	LaoWang lw1 = new LaoWang("yi");
	LaoWang lw2 = new LaoWang("er");
	...
	LaoWang lw1000000 = new LaoWang("baiwan");

可以看出我们能够根据类`LaoWang`实例化出成千百万个老王来，老王不是一个人在战斗！

####封装

刚才我们说的已经涉及到了对象的一个重要特性——封装

以前我们可能会有这样的描述

	王一长相文质彬彬，为人和善，姓王，有技能帮隔壁修下水道、亲切问候对方儿子
	王二长相文质彬彬，为人和善，姓王，有技能帮隔壁修下水道、亲切问候对方儿子
	王三长相文质彬彬，为人和善，姓王，有技能帮隔壁修下水道、亲切问候对方儿子
	王四长相文质彬彬，为人和善，姓王，有技能帮隔壁修下水道、亲切问候对方儿子
	...
	王百万长相文质彬彬，为人和善，姓王，有技能帮隔壁修下水道、亲切问候对方儿子

有了对象的思想我们可以这样说了，首先定义一类人


	有那么一类人

	1. 长相文质彬彬，为人和善
	2. 姓王

	同时这些人还有技能（行为）

	1. 帮隔壁修下水道
	2. 亲切问候对方儿子

然后是实例化，也就是对号入座

	王一是老王
	王二是老王
	...
	王百万是老王

也就是我们通过类来描述一套规则，其中包括


1. 属性
2. 行为

对于这个类实例化出的对象，也就是副歌这个类描述的对象，不用去关心对象细节，我们认为符合类的描述，就会有类规定的属性和方法，至于每个方法具体实现细节不去关注，比如老王怎么给人修水管，我知道他有修水管的技能，然后用的时候让他去修就好了（只要不修我家的）

我们称这种隐藏细节的特征叫做封装

####JavaScript 对象

因为JavaScript是基于原型（prototype）的，没有类的概念（ES6有了，这个暂且不谈），我们能接触到的都是对象，真正做到了一切皆为对象

所以我们再说对象就有些模糊了，很多同学会搞混类型的对象和对象本身这个概念，我们在接下来的术语中不提对象，我们使用和Java类似的方式，方便理解

	function People(name) {
		this.name = name;

		this.printName = function() {
			console.log(name);
		};
	}


这是一个函数，也是对象，我们称之为`类`

    var p1 = new People('Byron');

p1是People类new出来的对象，我们称之为`实例`

类和实例的关系用我们码农的专业眼光看起来是这样的


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-00ea0df05798f4bf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

类就是搬砖的模具，实例就是根据模具印出来的砖块，一个模具可以印出（实例化）多个实例，每个实例都符合类的特征，这个例子和我们JavaScript中概念很像

在Java中**类**不能称之为对象，如同`老王`
是一个概念、规则的集合，但是在JavaScript中，本身没有类的概念，我们需要用对象模拟出类，然后用类去创建对象

我们的例子中模具虽然是**“类”**，但同时也是个存在的实物，是个对象，我们为了方便理解，称之为`类`

####Object

我们知道JavaScript有`null`、`undefined`、`number`、`boolean`、`string`五种简单类型，`null`和`undefined`分别表示没有声明和声明后没有初始化的变量、对象，是两个简单的值，其余三个有对应的包装对象`Number`、`Boolean`、`String`

其它的就都是`object`类型了，比如常用的`Array`、`Date`、`RegExp`等，我们最常用的`Function`也是个对象，虽然


    typeof function(){}; // "function"

但是Function实例和其它类型的实例没有什么区别，都是对象，只不过`typeof`操作符对其做了特殊处理

在JavaScript中使用对象很简单，使用new操作符执行`Obejct`函数就可以构建一个最基本的对象

    var obj = new Object();

我们称new 调用的函数为构造函数，构造函数和普通函数区别仅仅在于是否使用了`new`来调用，它们的返回值也会不同

所谓“构造函数”，就是专门用来生成“对象”的函数。它提供模板，作为对象的基本结构。一个构造函数，可以生成多个对象，这些对象都有相同的结构

我们可以通过`.`来位对象添加属性和方法

	obj.name = 'Byron';
	obj.printName = function(){
	    console.log(obj.name);
	};

这么写比较麻烦，我们可以使用字面量来创建一个对象，下面的写法和上面等价

	var obj = {
		name: 'Byron',
		printNmae: function() {
			console.log(obj.name);
		}
	}



####构造对象


我们可以抛开类，使用字面量来构造一个对象

	var obj1 = {
		nick: 'Byron',
		age: 20,
		printName: function() {
			console.log(obj1.nick);
		}
	}
	var obj2 = {
		nick: 'Casper',
		age: 25,
		printName: function() {
			console.log(obj2.nick);
		}
	}

**问题**

这样构造有两个明显问题

1. 太麻烦了，每次构建一个对象都是复制一遍代码
2. 如果想个性化，只能通过手工赋值，使用者必需了解对象详细

这两个问题其实也是我们不能抛开类的重要原因，也是类的作用

####使用函数做自动化

	function createObj(nick, age) {
		var obj = {
			nick: nick,
			age: age,
			printName: function() {
				console.log(this.nick);
			}
		};
		return obj;
	}

	var obj3 = createObj('Byron', 30);
	obj3.printName();

我们通过创建一个函数来实现自动创建对象的过程，至于个性化通过参数实现，开发者不必关注细节，只需要传入指定参数即可

**问题**

这种方法解决了构造过程复杂，需要了解细节的问题，但是构造出来的对象类型都是Object，没有识别度

####有型一些

要想让我们构造出的函数有型一些，我们需要了解一些额外知识

1. function作为构造函数（通过new操作符调用）的时候会返回一个类型为function的name的对象

2. function可以接受参数，可以根据参数来创建相同类型不同值的对象

3. function实例作用域内有一个constructor属性，这个属性就可以指示其构造器

**new**

new 运算符接受一个函数 F 及其参数：new F(arguments...)。这一过程分为三步：

1. 创建类的实例。这步是把一个空的对象的 **proto** 属性设置为 F.prototype 。

2. 初始化实例。函数 F 被传入参数并调用，关键字 this 被设定为该实例。

3. 返回实例。

根据这几个特性，我们可以改造一下创建对象的方式

	function Person(nick, age) {
		this.nick = nick;
		this.age = age;
		this.sayName = function() {
			console.log(this.nick);
		}
	}
	var p1 = new Person();


**instanceof**

instanceof是一个操作符，可以判断**对象**是否为某个类型的实例

    p1 instanceof Person; // true
    p1 instanceof Object;// true

instanceof判断的是对象

    1 instanceof Number; // false

**问题**

构造函数在解决了上面所有问题，同时为实例带来了类型，但可以注意到每个实例printName方法实际上作用一样，但是每个实例要重复一遍，大量对象存在的时候是浪费内存


####构造函数

- 任何函数使用new表达式就是构造函数

- 每个**函数**都自动添加一个名称为prototype
属性，这是一个对象

- 每个**对象**都有一个内部属性__proto__
(规范中没有指定这个名称，但是浏览器都这么实现的) 指向其类型的prototype属性，类的实例也是对象，其****proto****属性指向“类”的prototype

*prototype*

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-8dfce2b2d6264eed.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


通过图示我们可以看出一些端倪，实例可以通过`__prop__`访问到其类型的prototype属性，这就意味着类的prototype对象可以作为一个公共容器，供所有实例访问。

####抽象重复

我们刚才的问题可以通过这个手段解决

- 所有实例都会通过原型链引用到类型的prototype

- prototype相当于特定类型所有实例都可以访问到的一个公共容器

- 重复的东西移动到公共容器里放一份就可以了

看下代码

    function Person(nick, age) {
        this.nick = nick;
        this.age = age;
    }
    Person.prototype.sayName = function() {
        console.log(this.nick);
    }

    var p1 = new Person(1, 2);
    console.log(p1);
    console.dir(p1);
    p1.sayName(); //1


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-eff97ab844140719.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这时候我们对应的关系是这样的


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-15fa541651ea4f67.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


####What's this?

由于运行期绑定的特性，JavaScript 中的 `this` 含义非常多，它可以是全局对象、当前对象或者任意对象，这完全取决于函数的调用方式

随着函数使用场合的不同，this的值会发生变化。但是有一个总的原则，那就是this指的是，调用函数的那个对象

####作为函数调用

在函数被直接调用时`this`绑定到全局对象。在浏览器中，`window` 就是该全局对象


    console.log(this);

    function fn1() {
        console.log(this);
    }

    fn1();


####内部函数

函数嵌套产生的内部函数的`this`不是其父函数，仍然是全局变量

    function fn0() {
        function fn() {
            console.log(this);
        }
        fn();
    }

    fn0();


####setTimeout、setInterval

这两个方法执行的函数this也是全局对象

    document.addEventListener('click', function(e) {
        console.log(this);
        setTimeout(function() {
            console.log(this);
        }, 1000);
    }, false);

####作为构造函数调用

所谓构造函数，就是通过这个函数生成一个新对象（object）。这时，this就指这个新对象

new 运算符接受一个函数 F 及其参数：new F(arguments...)。这一过程分为三步：

1. 创建类的实例。这步是把一个空的对象的 **proto** 属性设置为 F.prototype 。

2. 初始化实例。函数 F 被传入参数并调用，关键字 this 被设定为该实例。

3. 返回实例。

看例子

    function Person(name) {
        this.name = name;
    }
    Person.prototype.printName = function() {
        console.log(this.name);
    };

    var p1 = new Person('Byron');
    var p2 = new Person('Casper');
    var p3 = new Person('Vincent');

    p1.printName();
    p2.printName();
    p3.printName();

####作为对象方法调用


	var obj1 = {
		name: 'Byron',
		fn: function() {
			console.log(this);
		}
	};

	obj1.fn();



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-771cdc3c94b5c461.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**小陷阱**

	var fn2 = obj1;

	fn2();


####DOM对象绑定事件

在事件处理程序中`this`代表事件源DOM对象（低版本IE有bug，指向了window）

	document.addEventListener('click', function(e) {
		console.log(this);
		var _document = this;
		setTimeout(function() {
			console.log(this);
			console.log(_document);
		}, 200);
	}, false);


####Function.prototype.bind

bind，返回一个新函数，并且使函数内部的this为传入的第一个参数

	var obj1 = {
		name: 'Byron',
		fn: function() {
			console.log(this);
		}
	};
	obj1.fn();

	var fn3 = obj1.fn.bind(obj1);
	fn3();



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-70730c54e10f4312.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

####使用call和apply设置this

call apply，调用一个函数，传入函数执行上下文及参数

    fn.call(context, param1, param2...)

    fn.apply(context, paramArray)


语法很简单，第一个参数都是希望设置的`this`对象，不同之处在于`call`方法接收参数列表，而`apply`接收参数数组

    fn2.call(obj1);
    fn2.apply(obj1);


####caller

在函数`A`调用函数`B`时，被调用函数`B`会自动生成一个`caller`属性，指向调用它的函数对象，如果函数当前未被调用，或并非被其他函数调用，则`caller`为`null`


	function fn4() {
		console.log(fn4.caller);

		function fn() {
			console.log(fn.caller);
		}
		fn();
	}

	fn4();

####arguments



1. 在函数调用时，会自动在该函数内部生成一个名为 arguments的隐藏对象

2. 该对象类似于数组，可以使用[]运算符获取函数调用时传递的实参

3. 只有函数被调用时，arguments对象才会创建，未调用时其值为null



	function fn5(name, age) {
		console.log(arguments);
		name = 'XXX';
		console.log(arguments);
		arguments[1] = 30;
		console.log(arguments);
	}
	fn5('Byron', 20);

	function arg() {
		console.log(arguments)
	}
	arg()



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-75bf279d3457b9f9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


####callee

当函数被调用时，它的arguments.callee对象就会指向自身，也就是一个对自己的引用

由于arguments在函数被调用时才有效，因此arguments.callee在函数未调用时是不存在的（即null.callee），且解引用它会产生异常

	function fn6() {
		console.log(arguments.callee);
	}
	fn6();

匿名函数特好用

	var i = 0;
	window.onclick = function() {
		console.log(i);
		if (i < 5) {
			i++;
			setTimeout(arguments.callee, 200); //运行它自己了哈哈
		}
	}


####函数的执行环境

JavaScript中的函数既可以被当作普通函数执行，也可以作为对象的方法执行，这是导致 this 含义如此丰富的主要原因

一个函数被执行时，会创建一个执行环境（ExecutionContext），函数的所有的行为均发生在此执行环境中，构建该执行环境时，JavaScript 首先会创建 `arguments`变量，其中包含调用函数时传入的参数

接下来创建作用域链，然后初始化变量。首先初始化函数的形参表，值为 arguments变量中对应的值，如果 arguments变量中没有对应值，则该形参初始化为`undefined`。

如果该函数中含有内部函数，则初始化这些内部函数。如果没有，继续初始化该函数内定义的局部变量，需要注意的是此时这些变量初始化为 `undefined`，其赋值操作在执行环境（ExecutionContext）创建成功后，函数执行时才会执行，这点对于我们理解JavaScript中的变量作用域非常重要，最后为`this`变量赋值，会根据函数调用方式的不同，赋给`this`全局对象，当前对象等

至此函数的执行环境（ExecutionContext）创建成功，函数开始逐行执行，所需变量均从之前构建好的执行环境（ExecutionContext）中读取


####三种变量



1. 实例变量：（this）类的实例才能访问到的变量

2. 静态变量：（属性）直接类型对象能访问到的变量

3. 私有变量：（局部变量）当前作用域内有效的变量


看个例子

	function ClassA() {
		var a = 1; //私有变量，只有函数内部可以访问
		this.b = 2; //实例变量，只有实例可以访问
	}

	ClassA.c = 3; // 静态变量，也就是属性，类型访问

	console.log(a); // error
	console.log(ClassA.b) // undefined
	console.log(ClassA.c) //3

	var classa = new ClassA();
	console.log(classa.a); //undefined
	console.log(classa.b); // 2
	console.log(classa.c); //undefined


####原型链和继承

在一切开始之前回顾一下`类`、`实例`、`prototype`、`__proto__`的关系

	function Person(nick, age) {
		this.nick = nick;
		this.age = age;
	}
	Person.prototype.sayName = function() {
		console.log(this.nick);
	}

	var p1 = new Person();
	p1.sayName();


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-cc020154b51491d8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



1. 我们通过函数定义了类`Person`，类（函数）自动获得属性`prototype`
2. 每个类的实例都会有一个内部属性`__proto__`，指向类的`prototype`属性


####有趣的现象

我们定义一个数组，调用其`valueOf`方法

    [1, 2, 3].valueOf(); // [1, 2, 3]

很奇怪的是我们在数组的类型Array中并不能找到`valueOf`的定义，根据之前的理论那么极有可能定义在了Array的`prototype`中用于实例共享方法，查看一下



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-d5203db102bae432.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们发现`Array`的`prototype`里面并未包含`valueOf`等定义，那么`valueOf`是哪里来的呢？

一个有趣的现象是我们在Object实例的`__proto__`属性（也就是Object的prototype属性）中找到了找到了这个方法



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-2331a40e4293391b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

那么Array的实例为什么同样可以查找到Object的prototype里面定义的方法呢？

####查找valueOf过程

因为任何类的`prototype`属性本质上都是个类`Object`的实例，所以`prototype`也和其它实例一样也有个`__proto__`内部属性，指向其类型`Object`的prototype

我们大概可以知道为什么了，自己的类的prototype找不到的话，还会找prototypr的类型的prototype属性，这样层层向上查找

大概过程是这样的

1. 记当前对象位obj，查找obj属性、方法，找到后返回

2. 没有找到，通过obj的`__proto__`属性，找到其类型`Array`的`prototype`属性（记为prop）继续查找，找到后返回

3. 没有找到，把prop记为obj做递归重复步骤一，通过类似方法找到prop的类型`Object`的 prototype进行查找，找到返回


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-5a91e992ca39d03c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


####类型

之前介绍过`instanceof`操作符，判断一个对象是不是某个类型的实例

    [1, 2, 3] instanceof Array; //true

可以看到`[1, 2, 3]`是类型`Array`的实例


    [1, 2, 3] instanceof Object; //true

这个结果有些非议所思，怎么又是Array的实例，又是Object的实例，这不是乱套了

其实这个现象在日常生活中很常见其实，比如我们有两种类型


1. 类人猿
2. 动物

我们发现黑猩猩即是类人猿这个类的物种（实例），也是动物的实例

是不是悟出其中的门道了，类人猿是动物的一种，也就是说我们的两个类型之间有一种父子关系

这就是传说中的继承，JavaScript正是通过原型链实现继承机制的


####继承

>继承是指一个对象直接使用另一对象的属性和方法。

JavaScript并不提供原生的继承机制，我们自己实现的方式很多，介绍一种最为通用的

通过上面定义我们可以看出我们如果实现了两点的话就可以说我们实现了继承

1. 得到一个类的属性
2. 得到一个类的方法

我们分开讨论一下，先定义两个个类

	function Person(name, sex) {
		this.name = name;
		this.sex = sex;
	}

	Person.prototype.printName = function() {
		console.log(this.name);
	};

	function Male(age) {
		this.age = age;
	}

	Male.prototype.printAge = function() {
		console.log(this.age);
	};

**属性获取**

对象属性的获取是通过构造函数的执行，我们在一个类中执行另外一个类的构造函数，就可以把属性赋值到自己内部，但是我们需要把环境改到自己的作用域内，这就要借助我们讲过的函数`call`了   

改造一些`Male`


	function Male(name, sex, age){
	    Person.call(this, name, sex);
	    this.age = age;
	}

	Male.prototype.printAge = function(){
	    console.log(this.age);
	};

实例化结果

    var m = new Male('Byron', 'male', 26);
    console.log(m.sex); // "male"


**方法获取**

我们知道类的方法都定义在了prototype里面，所以只要我们把子类的prototype改为父类的prototype的备份就好了

    Male.prototype = Object.create(Person.prototype);


这里我们通过`Object.createclone`了一个新的prototype而不是直接把Person.prtotype直接赋值，因为引用关系，这样会导致后续修改子类的prototype也修改了父类的prototype，因为修改的是一个值

另外`Object.create`是ES5方法，之前版本通过遍历属性也可以实现浅拷贝

这样做需要注意一点就是对子类添加方法，必须在修改其prototype之后，如果在之前会被覆盖掉


	Male.prototype.printAge = function() {
		console.log(this.age);
	};

	Male.prototype = Object.create(Person.prototype);

这样的话，`printAge`方法在赋值后就没了，因此得这么写

	function Male(name, sex, age) {
		Person.call(this, name, sex);
		this.age = age;
	}

	Male.prototype = Object.create(Person.prototype);

	Male.prototype.printAge = function() {
		console.log(this.age);
	};


这样写貌似没问题了，但是有个问题就是我们知道prototype对象有一个属性`constructor`指向其类型，因为我们复制的父元素的prototype，这时候`constructor`属性指向是不对的，导致我们判断类型出错

    Male.prototype.constructor; //Person

因此我们需要再重新指定一下constructor属性到自己的类型

**最终方案**

我们可以通过一个函数实现刚才说的内容

	function inherit(superType, subType) {
		var _prototype = Object.create(superType.prototype);
		_prototype.constructor = subType;
		subType.prototype = _prototype;
	}

使用方式

	function Person(name, sex) {
		this.name = name;
		this.sex = sex;
	}

	Person.prototype.printName = function() {
		console.log(this.name);
	};

	function Male(name, sex, age) {
		Person.call(this, name, sex);
		this.age = age;
	}
	inherit(Person, Male);

	// 在继承函数之后写自己的方法，否则会被覆盖
	Male.prototype.printAge = function() {
		console.log(this.age);
	};

	var m = new Male('Byron', 'm', 26);
	m.printName();

这样我们就在JavaScript中实现了继承


####hasOwnProperty


有同学可能会问一个问题，继承之后Male的实例也有了Person的方法，那么怎么判断某个是自己的还是父类的？

`hasOwnPerperty`是`Object.prototype`的一个方法，可以判断一个对象是否包含自定义属性而不是原型链上的属性，`hasOwnProperty`是JavaScript中唯一一个处理属性但是不查找原型链的函数

    m.hasOwnProperty('name'); // true
    m.hasOwnProperty('printName'); // false
    Male.prototype.hasOwnProperty('printAge'); // true