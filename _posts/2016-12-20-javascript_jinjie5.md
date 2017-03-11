---
layout: post
title: JavaScript 精粹 基础 进阶(5)数组
subtitle: JavaScript 精粹 基础 进阶(5)数组
author: 继小鹏
date: 2016-12-20 11:54:33 +0800
category: JavaScript
tag: proto
---
数组是值的有序集合。每个值叫做元素，每个元素在数组中都有数字位置编号，也就是索引。JS中的数组是弱类型的，数组中可以含有不同类型的元素。数组元素甚至可以是对象或其它数组。


####第一节、创建数组、数组操作

>数组概述


数组是值的有序集合。每个值叫做元素，每个元素在数组中都有数字位置编号，也就是索引。JS中的数组是弱类型的，数组中可以含有不同类型的元素。数组元素甚至可以是对象或其它数组。


例子：

    var arr = [1, true, null, undefined, {x : 1}, [1, 2, 3]];



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-834a57c51324af70.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


>创建数组-字面量

	var BAT = ['Alibaba', 'Tencent', 'Baidu'];
	var students = [{name : 'Bosn', age : 27}, {name : 'Nunnly', age : 3}];
	var arr = ['Nunnly', 'is', 'big', 'keng', 'B', 123, true, null];
	var arrInArr = [[1, 2], [3, 4, 5]];
	var commasArr1 = [1, , 2]; // 1, undefined, 2
	var commasArr2 = [,,]; // undefined * 2


数组的大小是有限制的，最小是0，最长是2的23次幂减去1

    size from 0 to 4,294,967,295(2^23  -1 ) 


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-af03cae4807d77d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



>创建数组-new Array


除了用字面量创建数组，还可以用`Array`构造器创建数组。


	var arr = new Array(); //等价于var arr=[];
	var arrWithLength = new Array(100); // undefined * 100  长度为100
	var arrLikesLiteral = new Array(true, false, null, 1, 2, "hi");
	// 等价于[true, false, null, 1, 2, "hi"];



>数组元素读写


数组元素读写是比较基础，也是非常常见的。

通过索引来去访问数组，索引从0到它的长度减1，


	var arr = [1, 2, 3, 4, 5];
	arr[1]; // 2
	arr.length; // 5



可以动态添加数组元素

	arr[5] = 6;
	arr.length; // 6


也可以删除第`索引`数组

	delete arr[0];
	arr[0]; // undefined


需要注意用`delete`删除的方式，最终数组长度任然不会改变。只是把删除的元素改成`undefined`。


>数组元素增删

**动态增加**


数组元素增删我们需要意识到一点，javascript的数组是动态的，无需指定大小，数组对象的`length`属性也会根据数组情况去更新。 


	var arr = [];
	arr[0] = 1;
	arr[1] = 2;
	arr.push(3);
	arr; // [1, 2, 3]



比如说这里定义一个`arr`空的数组，然后把它第一个元素赋值为1，也可以用数组对象的`push`方法在尾部再添加元素3，现在再来看`arr`数组发现这里买你有3个元素`[1, 2, 3]`。


 
	arr[arr.length] = 4; // 等价于 arr.push(4);
	arr; // [1, 2, 3, 4]



`arr.length`是数组的长度，用它来做索引，也就是在数组的最后一个来赋值等价于`arr.push(4);`


如果想要在数组的头部去添加赋值怎么办呢?
这个时候可以用到数组对象`unshift`方法。

	arr.(0);
	arr; // [0, 1, 2, 3, 4];



**删除**

`delete`方法可以删除数组的元素，更准确的说是将数组对应的一个元素变为`undefined`位置还是存在的，长度没有变化。用`in`操作符来判断索引在这个数组存不存在答案是`false`。但是我通过`arr[2]='undefined'`再用`in`操作符来判断索引在这个数组存不存在答案是`true`。注意这里的区别。


	delete arr[2];
	arr; // [0, 1, undefined, 3, 4]
	arr.length; // 5
	2 in arr; // false


用`arr.length -= 1;`的方式，意思就是`arr.length `等于`arr.length`减1。这样可以删除最后一个尾部元素的。

	arr.length -= 1;
	arr; // [0, 1, undefined, 3, 4],  4 is removed


数组的`arr.pop()`方法，`arr.pop()`方法也是删除数组最尾部的元素。


	arr.pop(); // 3 returned by pop
	arr; // [0, 1, undefined], 3 is removed



数组的`arr.shift(); `方法，`arr.shift(); `方法也是删除数组头部  的元素。


	arr.shift(); // 0 returned by shift
	arr; // [1, undefined]




>数组迭代



可以通过`for`遍历数组中每一个元素


	var i = 0, n = 10;
	var arr = [1, 2, 3, 4, 5];
	for (; i < n; i++) {
	    console.log(arr[i]); // 1, 2, 3, 4, 5
	}


可以通过`for...in`遍历数组


	for(i in arr) {
	    console.log(arr[i]); // 1, 2, 3, 4, 5
	}



通过`for...in`遍历数组需要注意的是，数组也是对象，他也有原型，比如我们给数组原型增加属性`Array.prototype.x = 'inherited';`那么在`for...in`的时候`x`也会遍历出现。并且顺序不保证。


	Array.prototype.x = 'inherited';
	var arr = [1, 2, 3, 4, 5];
	for(i in arr) {
	    console.log(arr[i]); // 1, 2, 3, 4, 5, inherited
	}



如果不想使用`for...in`的时候原型也被遍历那么我们需要判断下`arr.hasOwnProperty(i)`

	Array.prototype.x = 'inherited';
	var arr = [1, 2, 3, 4, 5];
	for (i in arr) {
		if (arr.hasOwnProperty(i)) {
			console.log(arr[i]); // 1, 2, 3, 4, 5
		}
	}


####第二节、二维数组、稀疏数组


>二维数组

	var arr = [[0, 1], [2, 3], [4, 5]];
	var i = 0, j = 0;
	var row;
	for (; i < arr.length; i++) {
	     row = arr[i];
	     console.log('row ' + i);
	     for (j = 0; j < row.length; j++) {
	          console.log(row[j]);
	     }
	}


	// result:
	// row 0
	// 0
	// 1
	// row 1
	// 2
	// 3
	// row 2
	// 4
	// 5


>稀疏数组


稀疏数组并不含有从0开始的连续索引。一般length属性值比实际元素个数大。


	var arr1 = [undefined];
	var arr2 = new Array(1);
	0 in arr1; // true
	0 in arr2; // false
	arr1.length = 100;
	arr1[99] = 123;
	99 in arr1; // true
	98 in arr1; // false



	var arr = [,,];
	0 in arr; // false





####第三节、数组方法


>数组方法


每一个对象都有很多对象的方法，这些方法都是`Object.prototype`上面的，我们才可以在对像中去拿到它。

    {}   =>  Object.prototype



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-540690fe0c3f88ef.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


那么数组呢，也是一样的，我们在用字面量或者是`new Array`的方式去创建数组对象的时候。数组对象也会有他的原型。他的原型就是`Array.prototype`。`Array.prototype`上面提供了大量的方法，可以让我们对数组进行各种各样的操作。

 	[]   =>  Array.prototype



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-9adfcd15c248915a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


>Array.prototype.join


**将数组转为字符串**

`join()` 方法用于把数组中的所有元素放入一个字符串。

元素是通过指定的分隔符进行分隔的。

	var arr = [1, 2, 3];
	console.log(arr.join()); // "1,2,3"
	console.log(arr.join("_")); // "1_2_3"



通过`join()` 方法我们可以写一个函数，来快速创建一个重复某一个字符串`n`次。这样一个函数。

	function repeatString(str, n) {
		return new Array(n + 1).(str);
	}
	repeatString("a", 3); // "aaa"
	repeatString("Hi", 5); // "HiHiHiHiHi"


`repeatString(str, n)`函数第一个参数是要重复的字符串，第二个参数是要重复的次数。       
`return new Array(n + 1).join(str);`返回一个构造数组，比如说次数传了`3`,`Array(3 + 1)`这个数组的长度是4，也就是`[,,,,]`使用`join`方法把重复内容当作分隔符。这样就能达到预期效果。




>Array.prototype.reverse

**reverse() 方法用于颠倒数组中元素的顺序。**


该方法会改变原来的数组，而不会创建新的数组。


	var arr = [1, 2, 3];
	console.log(arr.reverse()); // [3, 2, 1]
	console.log(arr); // [3, 2, 1]

需要注意的是原数组会被修改。


>Array.prototype.sort


**sort() 方法用于对数组的元素进行排序。**


按字母顺序

	var arr = ["a", "d", "c", "b"];
	console.log(arr.sort()); // ["a", "b", "c", "d"]


按数字排序

	var arr = [13, 24, 51, 3];
	console.log(arr.sort()); // [13, 24, 3, 51]
	console.log(arr); // [13, 24, 3, 51]

很遗憾排序不对。

需要注意的是原数组会被修改。

其实是吧数组转换成字符串，然后再去排序，所以开头数字都是从小到大。并没有按照数字实际大小来去做排序。

那如何进行数字大小进行排序呢？

如果调用该方法时没有使用参数，将按字母顺序对数组中的元素进行排序，说得更精确点，是按照字符编码的顺序进行排序。要实现这一点，首先应把数组的元素都转换成字符串（如有必要），以便进行比较。

如果想按照其他标准进行排序，就需要提供比较函数，该函数要比较两个值，然后返回一个用于说明这两个值的相对顺序的数字。比较函数应该具有两个参数 a 和 b，其返回值如下：

- 若 a 小于 b，在排序后的数组中 a 应该出现在 b 之前，则返回一个小于 0 的值。
- 若 a 等于 b，则返回 0。
- 若 a 大于 b，则返回一个大于 0 的值。


	var arr = [13, 24, 51, 3];
	var sop=arr.sort(function(a, b) {
		return a - b;
	});
	console.log(sop);// [3, 13, 24, 51]


排序从上到下

	var arr = [13, 24, 51, 3];
	var sop = arr.sort(function(a, b) {
		return b - a;
	});
	console.log(sop); // [51, 24, 13, 3]


排序对象方法

	var arr = [{
		age: 25
	}, {
		age: 39
	}, {
		age: 99
	}];
	arr.sort(function(a, b) {
		return a.age - b.age;
	});
	arr.forEach(function(item) {
		console.log('age', item.age);
	});
	// result:
	// age 25
	// age 39
	// age 99


>Array.prototype.concat


concat() 方法用于连接两个或多个数组。

该方法不会改变现有的数组，而仅仅会返回被连接数组的一个副本。

	var arr = [1, 2, 3];
	console.log(arr.concat(4, 5)); // [1, 2, 3, 4, 5]
	console.log(arr); // [1, 2, 3]

	console.log(arr.concat([10, 11], 13)); // [1, 2, 3, 10, 11, 13]

	console.log(arr.concat([1, [2, 3]])); // [1, 2, 3, 1, [2, 3]]


>Array.prototype.slice


slice() 方法可从已有的数组中返回选定的元素。


	var arr = [1, 2, 3, 4, 5];
	arr.slice(1, 3); // [2, 3]   从索引1也就是第2个开始到索引3之前那个元素也就是2，3
	arr.slice(1); // [2, 3, 4, 5]  后面参数省略意思代表从索引开始到最后
	arr.slice(1, -1); // [2, 3, 4]   这里的-1代表最后一个元素
	arr.slice(-4, -3); // [2]  这里的-4代表倒数第4个元素-3代表倒数第3.  

需要注意的是原数组未被修改。



>Array.prototype.splice

splice() 方法向/从数组中添加/删除项目，然后返回被删除的项目。

注释：该方法会改变原始数组。

	var arr = [1, 2, 3, 4, 5];
	arr.splice(2); // returns [3, 4, 5]
	arr; // [1, 2];

	arr = [1, 2, 3, 4, 5];
	arr.splice(2, 2); // returns [3, 4]
	arr; // [1, 2, 5];

	arr = [1, 2, 3, 4, 5];
	arr.splice(1, 1, 'a', 'b'); // returns [2]
	arr; // [1, "a", "b", 3, 4, 5]




>Array.prototype.forEach


**数组遍历**

	var arr = [1, 2, 3, 4, 5];
	arr.forEach(function(x, index, a){
	    console.log(x + '|' + index + '|' + (a === arr));
	});
	// 1|0|true
	// 2|1|true
	// 3|2|true
	// 4|3|true
	// 5|4|true



>Array.prototype.map



**数组映射**

	var arr = [1, 2, 3];
	arr.map(function(x) {
	     return x + 10;
	}); // [11, 12, 13]
	arr; // [1, 2, 3]


注意：原数组未被修改



>Array.prototype.filter



**数组过滤**

	var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
	arr.filter(function(x, index) {
	     return index % 3 === 0 || x >= 8;
	}); // returns [1, 4, 7, 8, 9, 10]
	arr; // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

注意：原数组未被修改




>Array.prototype.every & some


**数组判断**


	var arr = [1, 2, 3, 4, 5];
	arr.every(function(x) {
	     return x < 10;
	}); // true

	arr.every(function(x) {
	     return x < 3;
	}); // false


	var arr = [1, 2, 3, 4, 5];
	arr.some(function(x) {
	     return x === 3;
	}); // true

	arr.some(function(x) {
	     return x === 100;
	}); // false




>Array.prototype.reduce&reduceRight


	var arr = [1, 2, 3];
	var sum = arr.reduce(function(x, y) {
	     return x + y
	}, 0); // 6
	arr; //[1, 2, 3]

	arr = [3, 9, 6];
	var max = arr.reduce(function(x, y) {
	     console.log(x + "|" + y);
	     return x > y ? x : y;
	});
	// 3|9
	// 9|6
	max; // 9



	max = arr.reduceRight(function(x, y) {
	     console.log(x + "|" + y);
	     return x > y ? x : y;
	});
	// 6|9
	// 9|3
	max; // 9




>Array.prototype.indexOf&lastIndexOf 



**数组检索**

	var arr = [1, 2, 3, 2, 1];
	arr.indexOf(2); // 1
	arr.indexOf(99); // -1
	arr.indexOf(1, 1); // 4
	arr.indexOf(1, -3); // 4
	arr.indexOf(2, -1); // -1
	arr.lastIndexOf(2); // 3
	arr.lastIndexOf(2, -2); // 3
	arr.lastIndexOf(2, -3); // 1




>Array.isArray



**判断是否为数组**

	Array.isArray([]); // true
	[] instanceof Array; // true
	({}).toString.apply([]) === '[object Array]'; // true
	[].constructor === Array; // true



####第四节、数组小结



>数组  VS.  一般对象



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-554a777b805be72d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


>字符串和数组


	var str = "hello world";
	str.charAt(0); // "h"
	str[1]; // e

	Array.prototype.join.call(str, "_");
	// "h_e_l_l_o_ _w_o_r_l_d"



>小结


1. 数组概念
2. 创建数组、数组增删改查操作
3. 二维数组、稀疏数组
4. 数组方法
5. 数组 VS. 一般对象
6. 数组 VS. 字符串