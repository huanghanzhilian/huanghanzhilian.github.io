---
layout: post
title: JavaScript 误区
subtitle: JavaScript 误区
author: 继小鹏
date: 2016-12-11 05:56:58 +0800
category: JavaScript
tag: JavaScript
---
接触JavaScript两年多遇到过各种错误，其中有一些让人防不胜防，原来对JavaScript的误会如此之深，仅以此文总结一下常见的各种想当然的误区

####String replace


string的replace方法我们经常用，替换string中的某些字符，语法像这样子

    string.replace(subStr/reg,replaceStr/function)

第一个参数是要查找的字符串或者一个正则表达式，第二个参数是想替换成的字符串或一个方法，我们可以这么使用

    "I'm Byron".replace("B","b") // I'm byron

结果和我们想得一样，但是

    "I'm a student, and you?".replace("n","N"); // I'm a studeNt, and you?

和我们预期的不一样，第二个‘n’没有被替换。**字符串的 replace 方法如果第一个参数传入字符串，那么只有第一个匹配项会被替换。如果要替换全部匹配项，需要传入一个 RegExp 对象并指定其 global 属性。**

    "I'm a student, and you?".replace(/n/g,"N"); // I'm a studeNt, aNd you?

这样才可以达到我们目的，关于string replace方法详细使用可以看看[JavaScript string 的replace]()



####typeof


关于typeof最容易让人误会的就是typeof是操作符，并不是函数，也就是说我们在判断一变量类型时没必要加括号，直接使用即可

    typeof 1; //number
    typeof(1); //number, 没必要这么写，但是也没错，等于是给变量或常量包装了一下

typeof 的返回值是字符串，在大多数情况下返回的和我们预期结果是一样的，但是有几点注意的地方,我们知道JavaScript有几种基本类型，number、string、boolean、null、undefined，再就是对象object了，我们看几个例子

	typeof 123; //number
	typeof NaN //number
	typeof "123" //string
	typeof false; //boolean
	typeof undefined; //undefined
	typeof null //object
	typeof new Array(); //object
	typeof(new Array()); //觉得不够清晰可以这么用，结果是一样的
	typeof(function() {}); //function
	typeof a; //undefined

1. 123是个数字返回number

2. 虽然NaN表示不是数字，但是typeof仍然会返回number

3. “123” 变成了字符串，所以返回string

4. false 类型就是boolean

5. undefined的类型就是undefined，这个类型的变量只能有一个字面值”undefined”

6. null的类型并不是null，而是object，所以我们不要寄希望与typeof帮我们判断null

7. typeof如果判断是对象只会返回object,而不会返回Array、Date的具体类型

9. function也是object的一种，按说也应该直接返回object。但是typeof对它区别对待了，返回了function，我们可以用此判断变量是否为函数

10. 没定义的变量同样返回undefined



####if和==

在javascript中if并不仅仅判断boolean决定真假，0、NaN、””、undefined、null、false都会被认为是假


	if (!false) {
		console.log(1);
	};
	if (!0) {
		console.log(2);
	};
	if (!"") {
		console.log(3);
	};
	if (!undefined) {
		console.log(4);
	};
	if (!null) {
		console.log(5);
	};

	if (!NaN) {

		console.log(6);

	};


在console中123456都会被打印出来

但是这并不意味着这些值就==false


	0 == false; //true
	"0" == false; //true,这个也是true
	undefined == false //false
	null == false //false
	null == null //true
	NaN == NaN //false
	null == undefined //true

纳尼！！！

**相等运算符 （==、!=） ** 如果两表达式的类型不同，则试图将它们**转换为字符串、数字或 Boolean 量**。

NaN 与包括其本身在内的任何值都不相等。

负零等于正零。

null 与 null 和 undefined 相等。

**相同的字符串、数值上相等的数字、相同的对象、相同的 Boolean 值或者（当类型不同时）能被强制转化为上述情况之一，均被认为是相等的**。其他比较均被认为是不相等的。

所以说 0==”0”; 返回的也会是true。

那么应该则么判断两个变量究竟是不是一回事儿呢

**恒等运算符 （===、!==） **

除了不进行类型转换，并且类型必须相同以外，这些运算符与相等运算符的作用是一样的，这下就搞定了。


####Date 对象

我们可以这样构造一个Date对象

	new Date() //Date {Fri Aug 02 2013 16:50:33 GMT+0800 (China Standard Time)}
	new Date(milliseconds) //Date {Fri Aug 02 2013 16:53:26 GMT+0800 (China Standard Time)}
	new Date("2013/08/02") //Date {Fri Aug 02 2013 00:00:00 GMT+0800 (China Standard Time)}
	new Date(year, month, day, hours, minutes, seconds, ms)


前三种方式没有什么问题，但第四种得到的结果回合我们预期的不一致

    new Date(2013,08,02) //Date {Mon Sep 02 2013 00:00:00 GMT+0800 (China Standard Time)}

我们可以看到，传入的月份是08，返回的结果却是九月。这是因为**Date对象的月份是从0开始计数的（天却不是），即0代表一月，1代表二月…11代表12月**。在调用Date实例的getMonth方法时尤其要注意

    var d = new Date(2012, 4, 15); // 2012年5月15日
    alert(d.getMonth()); // 结果为4



####Date.parse

Date.parse方法可以识别两种格式的字符串参数*（标准的长日期格式，比如带星期的那种，也可以识别，不过不常用）*：

1. "M/d/yyyy": 美国的日期显示格式。如果年传入2位则作为 19xx 处理

2. "yyyy-MM-dd" 或 "yyyy/MM/dd": 注意月和日都必须是两位

**Date.parse 的返回结果不是一个Date对象，而是从1970-01-01午夜（GMT）到给定日期之间的毫秒数**。可以用Date的构造函数将其转换为Date对象。


	new Date(Date.parse("8/2/2012")); // 正确识别为2012年8月2日
	new Date(Date.parse("2012-08-02")); // 正确识别为2012年8月2日
	new Date(Date.parse("2012-8-2")); // 不能识别


####for…in 遍历数组

for…in用来遍历一个对象中的成员（属性，方法），如果用来遍历数组的到的结果并不是预期中数组每项的值，方法神马的会被遍历出来

	Array.prototype.contains = function(item) {
		for (var i = 0; i <= this.length - 1; i++) {
			if (this[i] == item)
				return this[i];
		}
	}
	var staff = ["Staff A", "Staff B"];
	// Normal Enumeration: Only the 2 items are enumerated
	for (var i = 0; i <= staff.length - 1; i++) {
		var singleStaff = staff[i];
		alert(singleStaff);
	}
	// for...in Enumeration: the method "contains" are enumerated, too
	for (var singleStaff in staff) {
		alert(singleStaff);
	}



事实上很多时候我们都会给数组加上其他属性。比如 jQuery 对象就是一个数组对象加上一些扩展方法；再比如 String.prototype.match 方法返回值就是一个数组（正则表达式及其子表达式的匹配项）加上 index 和 input 两个属性。


####parseInt

参数|描述
----|----
string|必需。要被解析的字符串。
radix |1. 可选。表示要解析的数字的基数。该值介于 2 ~ 36 之间。
radix |2. 如果省略该参数或其值为 0，则数字将以 10 为基础来解析。
radix |3. 如果它以 “0x” 或 “0X” 开头，将以 16 为基数。如果该参数小于 2 或者大于 36，则 parseInt() 将返回 NaN。

当参数 *radix* 的值为 0，或没有设置该参数时，parseInt() 会根据 *string* 来判断数字的基数。

举例，如果 *string（开头结尾空格自动省略）* 以 "0x" 开头，parseInt() 会把 *string* 的其余部分解析为十六进制的整数。如果 *string* 以 0 开头，那么 ECMAScript v3 允许 parseInt() 的一个实现把其后的字符解析为八进制或十六进制的数字。如果 *string* 以 1 ~ 9 的数字开头，parseInt() 将把它解析为十进制的整数。如果字符串的第一个字符不能被转换为数字，那么 parseFloat() 会返回 NaN。


	parseInt("10");        //返回 10
	parseInt("19",10);        //返回 19 (10+9)
	parseInt("11",2);        //返回 3 (2+1)
	parseInt("17",8);        //返回 15 (8+7)
	parseInt("1f",16);        //返回 31 (16+15)
	parseInt("010");        //未定：返回 10 或 8


####setTimeout/setInterval执行时机


setTimeout()和setInterval()经常被用来处理延时和定时任务。**setTimeout**() 方法用于在指定的毫秒数后调用函数或计算表达式,而**setInterval**()则可以在每隔指定的毫秒数循环调用函数或表达式，直到clearInterval把它清除。

**JavaScript其实是运行在单线程的环境中的**，这就意味着定时器仅仅是**计划**代码在未来的某个时间执行，而**具体执行时机是不能保证的**，因为页面的生命周期中，不同时间可能有其他代码在控制JavaScript进程。在页面下载完成后代码的运行、事件处理程序、Ajax回调函数都是使用同样的线程，实际上浏览器负责进行排序，指派某段程序在某个时间点运行的**优先级**。

我们可以可以把JavaScript想象成在时间线上运行。当页面载入的时候首先执行的是页面生命周期后面要用的方法和变量声明和数据处理，在这之后JavaScript进程将等待更多代码执行。当进程空闲的时候，下一段代码会被触发

除了主JavaScript进程外，还需要一个在进程下一次空闲时执行的代码队列。随着页面生命周期推移，代码会按照执行顺序添加入队列，例如当按钮被按下的时候他的事件处理程序会被添加到队列中，并在下一个可能时间内执行。在接到某个Ajax响应时，回调函数的代码会被添加到队列。**JavaScript中没有任何代码是立即执行的，但一旦进程空闲则尽快执行。**定时器对队列的工作方式是当特定时间过去后将代码插入，这并不意味着它会马上执行，只能表示它尽快执行。

关于setTimeout/setInterval执行时机详细说明可以看看 [setTimeout()和setInterval() 何时被调用执行](http://www.huanghanlian.com/javascript/2016/12/11/settimeout-1.html)


####预解析


	console.log(a); //Error:a is not defined ,直接报错，下面语句没法执行，一下结果为注释该句后结果
	console.log(b) //undefined
	var b = "Test";
	console.log(b); //Test


很奇怪前两句变量a,b都没有声明，第一句却报错，第二句能够输出undefined？这是因为**JavaScript是解释型语言，但它并不是直接逐步执行的，JavaScript解析过程分为先后两个阶段，一个是预处理阶段，另外一个就是执行阶段。在预处理阶段JavaScript解释器将完成把JavaScript脚本代码转换到字节码，然后第二阶段JavaScript解释器借助执行环境把字节码生成机械码，并顺序执行。**

也就说JavaScript值执行第一句语句之前就已经将函数/变量声明预处理了，var b=”Test” 相当于两个语句，var 
b;（undefined结果的来源，在执行第一句语句之前已经解析），b=”Test”（这句是顺序执行的，在第二句之后执行）。这也是为什么我们可以在方法声明语句之前就调用方法的原因。



	showMsg(); // This is message
	function showMsg() {
		alert('This is message');
	}



####块级作用域


**JavaScript没有块级作用域，只有函数级作用域，这就意味着{}在JavaScript中只能起到语法块的作用，而不能起到作用域块作用**


	if (true) { //语法块，保证{}内代码if条件成立执行
		//...
		var a = 3;
	}
	console.log(a); //3

上面例子可以清楚看到属于window的console.log方法依然可以访问貌似是局部变量的a


####闭包

首先从一个经典错误谈起，页面上有若干个div， 我们想给它们绑定一个onclick方法，于是有了下面的代码


	<div id="divTest">
	    <span>0</span> <span>1</span> <span>2</span> <span>3</span>
	</div>
	<div id="divTest2">
	    <span>0</span> <span>1</span> <span>2</span> <span>3</span>
	</div>
	<script type="text/javascript">
	$(document).ready(function() {
		var spans = $("#divTest span");
		for (var i = 0; i < spans.length; i++) {
			spans[i].onclick = function() {
				alert(i);
			}
		}
	});
	</script>


很简单的功能可是却偏偏出错了，每次alert出的值都是4，简单的修改就好使了


	var spans2 = $("#divTest2 span");
	$(document).ready(function() {
		for (var i = 0; i < spans2.length; i++) {
			(function(num) {
				spans2[i].onclick = function() {
					alert(num);
				}
			})(i);
		}
	});



闭包是指有权限访问另一个函数作用域的变量的函数，创建闭包的常见方式就是在一个函数内部创建另一个函数,只要存在调用内部函数的可能，JavaScript就需要保留被引用的函数。而且JavaScript运行时需要跟踪引用这个内部函数的所有变量，直到最后一个变量废弃，JavaScript的垃圾收集器才能释放相应的内存空间。

这笔书的很肤浅，想简单了解闭包可以看看[JavaScript 闭包究竟是什么](http://www.huanghanlian.com/javascript/2016/12/05/javascript-bbssm.html)