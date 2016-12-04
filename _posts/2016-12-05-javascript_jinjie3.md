---
layout: post
title: JavaScript 精粹 基础 进阶(3)语句
subtitle: JavaScript 精粹 基础 进阶(3)语句
author: 继小鹏
date: 2016-12-05 02:23:26 +0800
category: JavaScript
tag: JavaScript
---
JavaScript程序由语句组成，语句遵守特定的语法规则。例如：if语句, while语句, with语句等等。

####语句

>语句、严格模式
    
**JavaScript程序由语句组成，语句遵守特定的语法规则。例如：if语句, while语句, with语句等等。**

*语句种类*



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-fc0d134cf88a6bbf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**块 block**

*块语句常用于组合0 ~ 多个语句。块语句用一对花括号定义。*

    语法：
    {
      语句1;
      语句2;
      ...
      语句n;
    }
    比如
    {
        var str = "hi";
        console.log(str);
    }//但在实际开发中，很少单独使用块语句
    而是用if...else  或者for  while去结合起来使用，

    if(true){
    //然后{}是一个块，
    然后{里面可以放多条语句}
     console.log('hi');
    }


**声明语句 var**

    声明变量语句
    var a=1;

    在一个var里也可以声明多个变量
    var a=b=1;
    这段语句确实创建了a,b两个变量并且赋值为1，但是其中b实际上是隐式的创建了全局变量，不信看下面的例子

    function foo() {
        var a = b = 1;
    }
    foo();

    console.log(typeof a);  // ‘undefined’
    console.log(typeof b);  // ‘number  

    在一条语句里定义多个变量的方式使用，号分隔
    var a=1,b=1;
    
 

**try catch语句**

    try {    //工作流程首先执行try,catch中的代码
        throw "test";    //如果抛出了异常  ，如果没有发生异常catch中的代码就会被忽略掉
    } catch (ex) {    //会由catch去捕获
        console.log(ex); // test    //并执行
    } finally {    //finally 不管有没有异常，finally 都会执行
        console.log('finally');
    }

    javascript中try...catch可以有三种形式
    try {
        throw "test";
    } catch (ex) {
        console.log(ex); // test
    }

    try {
        throw "test";
    }finally {
        console.log('finally');
    }

    //还有一种就是两者皆有
    try {
        throw "test";
    } catch (ex) {
        console.log(ex); // test
    } finally {
        console.log('finally');
    }

*例子1*

    try {
        // do sth.
    } finally {
        console.log('finally');
    }
    //不管有没有异常最后都有执行finally中的内容

*例子2*

    try {
        try {
            throw new Error("oops");
        }
        finally {
            console.log("finally");
        }
        //内部try没有catch，那么他会跳到最近的catch，也就是外层的catch去处理，再跳出block之前需要先执行finally
    }
    catch (ex) {
        console.error("outer", ex.message);
    }
    //所以这里的执行结果
    "finally"
    "outer" "oops"

 
*例子3*

    try {
      try {
        throw new Error("oops");
      }
      catch (ex) {
        console.error("inner", ex.message);
      }
      finally {
        console.log("finally");
      }
    }
    catch (ex) {
      console.error("outer", ex.message);
    }//由于内部异常已经处理过，所以不会再跳出到外部处理
    执行结果
    "inner" "oops"
    "finally"


*例子4*

    try {
      try {
        throw new Error("oops");
      }
      catch (ex) {
        console.error("inner", ex.message);
        throw ex;
      }
      finally {
        console.log("finally");
      }
    }
    catch (ex) {
      console.error("outer", ex.message);
    }//由于内部catch再次抛出异常就会由外部catch来处理
    执行结果是
    "inner" "oops"
    "finally"
    "outer" "oops"


**function语句**

*函数声明*

    fd(); // true
    function fd() {
        // do sth.
        return true;
    }

*函数表达式*

    fe(); // TypeError
    var fe = function() {  //定义变量fe，然后把一个匿名函数赋值给fe
        // do sth.
    };

*两者区别在函数声明前面调用函数也是可以的，函数表达式就不可以。*

**for...in语句**

    var p;
    var obj = {x : 1, y: 2}

    for (p in obj) {
    }


1. 顺序不确定
2. enumerable为false时不会出现
3. for in对象属性时受原型链影响

**switch语句**

*例子1*
    var val = 2;

    switch(val) {
        case 1:
            console.log(1);
            break;
        case 2:
            console.log(2);    //val==2,会执行这段代码
            break;             //使用 break 来阻止代码自动地向下一个 case 运行。
        default:               //使用 default 关键词来规定匹配不存在时做的事情
            console.log(0);
            break;
    }


*例子2*

    var val = 2;

    switch(val) {
        case 1:
            console.log(1);
        case 2:
            console.log(2);
        default:
            console.log(0);
    }
    //输出2,0

*例子3*

    var val = 2;

    switch(val) {
        case 1:
        case 2:
        case 3:
            console.log(123);
            break;
        case 4:
        case 5:
            console.log(45); 
            break;
        default:
            console.log(0);
    }
    //输出123


**循环语句while**

    while (条件)
    {
        需要执行的代码
    }

*例子*

    var x="",i=0;
    while (i<5){
        x=x + "该数字为 " + i ;
        i++;
    }
    console.log(x);

**do/while 循环**

    do
    {
        需要执行的代码
    }
    while (条件);

*例子*

    var x="",i=0;
    do{
        x=x + "该数字为 " + i;
        i++;
    }
    while (i<5)  
    console.log(x)
    //该数字为 0该数字为 1该数字为 2该数字为 3该数字为 4

**JavaScript for 循环**

    cars=["BMW","Volvo","Saab","Ford"];
    for (var i=0;i<cars.length;i++){
        console.log(cars[i])
    }
    // BMW
    // Volvo
    // Saab
    // For

>不同类型的循环

JavaScript 支持不同类型的循环：

**for** - 循环代码块一定的次数

**for/in** - 循环遍历对象的属性

**while** - 当指定的条件为 true 时循环指定的代码块

**do/while** - 同样当指定的条件为 true 时循环指定的代码块




**with语句**


*with 语句可以方便地用来引用某个特定对象中已有的属性，但是不能用来给对象添加属性。要给对象创建新的属性，必须明确地引用该对象 *

语法格式 

    with(object instance) 
    { 
            //代码块 
    }  

*举例 *

    function Lakers() {
       this.name = "kobe bryant";
       this.age = "28";
       this.gender = "boy";
    }
    var people=new Lakers();
    console.log(people)
    with(people)
    {
       var str = "姓名: " + name + "<br>";
       str += "年龄：" + age + "<br>";
       str += "性别：" + gender;
       document.write(str);
    }
    // 姓名: kobe bryant
    // 年龄：28
    // 性别：boy

1. 让JS引擎优化更难
2. 可读性差
3. 可被变量定义代替
4. 严格模式下被禁用

>JavaScript 严格模式

*严格模式是一种特殊的执行模式，
它修复了部分语言上的不足，提供更强的错误检查，并增强安全性。*

    function func() {
        'use strict';
    }

    'use strict';
    function func() {
      
    }


**不允许用with**

    !function() {
         with({x : 1}) {
               console.log(x);
          }
    }();
    //输出1

    !function() {
        'use strict';
         with({x : 1}) {
                console.log(x);
          }
    }();
    SyntaxError报错


**不允许未声明的变量被赋值**

    !function() {
         x = 1;    //全局变量
          console.log(window.x);
    }();
    //输出1

    !function() {
        'use strict';
         x = 1;
          console.log(window.x);
    }();
    //ReferenceError
    //在use strict如果没有被声明的变量会报错


**arguments变为参数的静态副本**

arguments.length 属性返回函数调用过程接收到的参数个数：
arguments属性返回函数调用过程接收到的值

*一般模式下*

    !function(a) {
        arguments[0] = 100;//给函数第一个值变成100
        console.log(a);//输出的a变成100
    }(1);
    //输出100

    !function(a) {
        arguments[0] = 100;
        console.log(a);
    }();//如果()没有传值arguments长度为空所以更改arguments的值不成立
    输出undefined


*严格模式下*

    !function(a) {
        'use strict';
        arguments[0] = 100;
        console.log(a);
    }(1);
    //1

    !function(a) {
        'use strict';
        arguments[0].x = 100;
        console.log(a.x);
    }({x:1});
    //100

**delete参数、函数名报错**

一般模式下

    !function(a) {
        console.log(delete a);
    }(1);
    //false

'use strict';模式下

    !function(a) {
        'use strict';
        delete a;
    }(1);
    SyntaxError报错语法错误



**delete不可配置的属性报错**

一般模式下

    !function(a) {
        var obj = {};
        Object.defineProperty(obj, 
            'a', {configurable : false});
        console.log(delete obj.a);
    }(1);
    //false


'use strict';模式下

    !function(a) {
        'use strict';
        var obj = {};
        Object.defineProperty(obj, 
            'a', {configurable : false});
        delete obj.a;
    }(1);
    //TypeError报错


**对象字面量重复属性名报错**

一般模式下

    !function() {
        var obj = {x : 1, x : 2};//对象属性字面量去重复去写属性，这样也是合法的，属性以最后一个为准
        console.log(obj.x);
    }();
    //输出为2

'use strict';模式下

    !function() {
        'use strict';
        var obj = {x : 1, x : 2};
    }();

    //严格模式下
    //SyntaxError报错

**禁止八进制字面量**

一般模式下

    !function() {
        console.log(0123);//8进制的值
    }();
    //输出83

在严格模式下是不允许8进制

    !function() {
        'use strict';
        console.log(0123);
    }();
    //SyntaxError报错语法错误


**eval, arguments变为关键字，不能作为变量、函数名**

一般模式

    !function() {
        function eval(){}
        console.log(eval);
    }();
    //function eval(){}


严格模式

    !function() {
        'use strict';
        function eval(){}
    }();
    //SyntaxError语法错误


**eval独立作用域**

一般模式

    !function() {
        eval('var evalVal = 2;');      //在eval函数下定义变量evalVal等于数字2
        console.log(typeof evalVal);   //evalVal所在的函数内部仍然可以拿到 evalVal
    }();
    //所以输出number

严格模式

    !function() {
        'use strict';
        eval('var evalVal = 2;');    //eval代码会在独立作用域执行
        console.log(typeof evalVal); //这里拿evalVal是拿不到的
    }();
    //所以输出undefined


####严格模式总结

>不允许用with
所有变量必须声明, 赋值给为声明的变量报错，而不是隐式创建全局变量。
eval中的代码不能创建eval所在作用域下的变量、函数。而是为eval单独创建一个作用域，并在eval返回时丢弃。
函数中得特殊对象arguments是静态副本，而不像非严格模式那样，修改arguments或修改参数变量会相互影响。
删除configurable=false的属性时报错，而不是忽略
禁止八进制字面量，如010 (八进制的8)
eval, arguments变为关键字，不可作为变量名、函数名等
>>一般函数调用时(不是对象的方法调用，也不使用apply/call/bind等修改this)this指向null，而不是全局对象。
若使用apply/call，当传入null或undefined时，this将指向null或undefined，而不是全局对象。
试图修改不可写属性(writable=false)，在不可扩展的对象上添加属性时报TypeError，而不是忽略。
arguments.caller, arguments.callee被禁用


**严格模式是一种特殊的运行模式，
它修复了部分语言上的不足，提供更强的错误检查，并增强安全性。**