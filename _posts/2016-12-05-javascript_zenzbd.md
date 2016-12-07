---
layout: post
title: JavaScript 正则表达式
subtitle: JavaScript 正则表达式
author: 继小鹏
date: 2016-12-07 02:22:19 +0800
category: JavaScript
tag: JavaScript
---
正则表达式的主要作用是用来**匹配字符**，由于他简单且功能强大，所以它不仅用在javascript，很多高级语言JAVA,PHP等，也都支持正则表达式。

####工作原理

    通配符匹配技术



####定义正则表达式

JavaScript种正则表达式有两种定义方式，定义一个匹配类似 <%XXX%> 的字符串


>构造函数

    var reg=new RegExp("a","i");
    //RegExp是英文正则表达式的缩写，
    //使用new运算符去实例化这样一个对象就可以了
    //("a","i");后面有两个参数"a"（模式） 描述了表达式的模式。"i"(修饰符) 区分大小写的匹配

>字面量

    var reg=/a/i;
    //正则表达式直接量创建正则表达式，用"/a/"正则内容写在两个斜杠中间，(修饰符)写在斜杠后面。同样可以创建正则表达式


例子：

    var patt1 = new RegExp("a", "i");  //创建匹配字符串a不区分大小写不管a字符在单词任何位置上都是可以匹配的
    var a = "Aser";
    var b = "afgth";
    var c = "sdfga";
    //test检索字符串中指定的值。返回 true 或 false。
    console.log(patt1.test(a)); //true
    console.log(patt1.test(b)); //true
    console.log(patt1.test(c)); //true


>**如果我想匹配`*`号字符怎么办呢**

`*`在这是有特殊意义的，它是去匹配所有字符的。我现在就想要去匹配`*`那就需要用到 **`"\"`转义**，把`*`转义下。

    var reg=/\*/i;
    var a = "As*er";
    console.log(reg.test(a)); //true

现在是匹配`*`号，而不是匹配所有字符

类似`*`的字符，在正则表达式中有很多，这些都是特殊字符和符号。


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-ce2bea41a83c4ba9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这些符号如果要去匹配，都需要用`"\"`反斜杠来进行转义。



**如果我要匹配a-z,26个英文字母，我们怎么来写这个表达式呢？**

>正则表达式给我们提供了**字符类**的工具。


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-a7e12601d6389c26.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




这里面会定义一些字符类


举例：

    var reg=/\w/;
    reg.test('sdf');//true


    var reg=/\W/;
    reg.test('sdf');//false


    var reg=/\s/; 
    reg.test(' ');//true

    var reg=/\S/; 
    reg.test(' ');//false


    var reg=/\d/; 
    reg.test(2);//true

    var reg=/\D/; 
    reg.test(2);//false


    var reg=/[as]/; 
    reg.test('as');//true
    reg.test('gg');//false

    var reg=/[^as]/; 
    reg.test('as');//false
    reg.test('ff');//true



第一个“^”,代表匹配字符串的开头，第二个“^”,代表匹配非方括号中的所有字符

    var re=/^[^\d]\w+/g




>正则表达式给我们提供了**重复类**的工具。



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-4027e38682d32d48.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




用实例来说话。

比如说像开始咋们写的

    var re = /a/i
    //正则表达式直接量创建正则表达式，用"/a/"正则内容写在两个斜杠中间，(修饰符)写在斜杠后面。同样可以创建正则表达式

那么如果发生改变


    var reg=/a{3}/i;
    var a = "aaass";
    var b = "afgth";
    var c = "sdfga";
    console.log(reg.test(a)); //true
    console.log(reg.test(b)); //false
    console.log(reg.test(c)); //false


如果`/a{3}/意思是，要求必须有连续3个字符出现才能够正确的被匹配。 



>正则表达式给我们提供了**选择符**。



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-982fb97fb6ebedac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


比如说我们想匹配`a`字符或者`b`字符。

    var re = /a|b/i;
    re.test('a');//true
    re.test('b');//true
    re.test('c');//false


>定位符


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-2a7fe397a53a95b4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


比如说`var re = /^a/i;`那么就代表a必须出现在这个字符串的开头。         

    var re = /^a/i; 
    re.test('aaaa');//true
    re.test('ba');//false
    re.test('ad');//true


比如说`var re = /a$/i;`那么就代表a必须出现在这个字符串的结尾。

    var re = /a$/i;
    re.test('asd');//false
    re.test('dsa');//true




>正则表达式的**分组**和**标志**




![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-5db78f1630b0eccd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



**标志怎么来用**    


`var re = /a/i;`这里用到了`i`标志，意思就是说，当我去匹配a字符的时候，是不区分大小写的。匹配的字符有小写`a`，大写`A`都能被匹配到。


`var re = /a/ig;`全局匹配，正则表达式它在工作的时候，当你一个字符串去匹配的时候，它是从字符串第一个字母向后去匹配，当它匹配到第一个`a`字符的时候，它就会停下来，那么加了个`g`以后，它就会继续向后匹配`a`字符。


`var re = /a/m;`如果你的字符串是多行的文本，它就会在多行文本进行匹配。




>正则表达式对象的方法


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-281831aafe25f9a5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




**以上是正则表达式的基本用法，我们通过这些知识点再来强化对正则表达式的用法了理解**



首先匹配a字母

    var re=/a/;

能够匹配d到g之间的字符

    var re=/a[d-g]/;

然后把他们俩分成一组

    var re=/(a[d-g])/;

并且这一组要出现两次

    var re=/(a[d-g]){2}/;

不分大小写


    var re=/(a[d-g]){2}/i;


>那么什么样的规则能够匹配正确呢？


    var str = "";
    var re = /(a[d-g]){2}/i;
    console.log(re.test(str));//false



先需要匹配a字符还要匹配d-g之间的字符。这两是一组，这一组要出现两次所以还要出现a和d-g之间的字符。后面可以随便写


    var str = "adaewertt";
    var re = /(a[d-g]){2}/i;
    console.log(re.test(str));//true


这样就可以匹配上。


    var str = "tdaewertt";
    var re = /(a[d-g]){2}/i;
    console.log(re.test(str));//false



    var str = "adraewertt";
    var re = /(a[d-g]){2}/i;
    console.log(re.test(str));//false




>根据要求做正则

做用户名正则


1. 要求是数字，字母（不分大小写），汉字，下划线
2. 长度要求5-25个字符推荐使用中文会员名




`[a-zA-Z_0-9]`同等于`\w`

`\u4e00-\u9fa5`代表所有的中文字符


**匹配全局有字母a-z,A-Z,数字0-9，和中文。如果有就匹配成功**

    var str = '哈哈hi';
    var re=/[\w\u4e00-\u9fa5]/g;
    console.log(re.test(str));//true


**匹配全局*`非`*字母a-z,A-Z,数字0-9，和中文。如果*`非也就是说字母数字中文之外的非法字符了`*就匹配成功**

    var str = '哈哈hi++';
    var re=/[^\w\u4e00-\u9fa5]/g;
    console.log(re.test(str));//true

说明此字段有非法字符



>题外课

如何获取字符长度

>中文是双字节的字符那么如何获取呢？

获取英文字节好获取

    var a='sss';
    console.log(a.length);//3

但是中英文在一起就不对了

    var a='sss订单';
    console.log(a.length);//5

我们可以通过`正则[^\x00-\xff]`来获取，`[^\x00-\xff]`*匹配双字节字符(包括汉字在内)*

>JavaScript replace() 方法

>>replace() 方法用于在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串。

匹配全局双字节字符，把所有的双字节字符替换成xx


    var p = '中q订单';
    console.log(p.replace(/[^\x00-\xff]/g, "xx"));    //xxqxxxx


`xxqxxxx`把中文双字节变成了英文单字节，这样我们就能得出字符的长度了，试一试

    var p = '中q订单';
    console.log(p.replace(/[^\x00-\xff]/g, "xx").length);    //7



>判断相同字符



    function findstr(str, n) {
        var tmp = 0;
        for (var i = 0; i < str.length; i++) {
            if (str.charAt(i) == n) {
                tmp++;
            }
        }
        return tmp;
    }

    var g = "ffffff";

    var m = findstr(g, g[0]);
    if (g.length == m) {
        alert("字符相同")
    } else {
        alert("字符不相同")
    }


`findstr`函数有两个参数，`str`是匹配字符串，`n`是字符串的第一个值。           
先在函数内部声明变量`tmp`               
用`str`传来的字符串长度来循环           
循环内部判断`str`传来的字符串遍历每一个字符看看是不是等于字符串的第一个值      
如果是就让变量`tmp`+1            
否则不加            
然后返回变量`tmp`                 
`var m = findstr(g, g[0]);`m调用findstr穿参并得到返回值，

    if (g.length == m) {
        alert("字符相同")
    } else {
        alert("字符不相同")
    }

如果字符串的长度等于`m`也就是字符串都一样`m`的值才会一样。所以字符串是相同的。               
如果不是那就说明字符串不相同。



>正则判断不能全为数字


`var re=/[\d]/g;`全局匹配数字

`var re=/[^\d]/g;`全局匹配非数字字符


    var re = /[^\d]/g;
    var msg = 12345;
    if (!re.test(msg)) {
        console.log("全是数字大哥")
    }



>正则判断不能全为字母


`var re = /[a-zA-Z]/g;`全局匹配字母

`var re = /[^a-zA-Z]/g;`全局匹配非字母字符


    var re = /[^a-zA-Z]/g; //匹配所有除字母之外的东西
    var msg = "asssssssssss";
    if (!re.test(msg)) {
        console.log("全是字母大哥")
    }



>针对这篇文章，做了个小[demo](https://github.com/huanghanzhilian/RegExp)。欢迎Star。















































