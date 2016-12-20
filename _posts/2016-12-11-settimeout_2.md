---
layout: post
title: 常见的BOM功能
subtitle: 常见的BOM功能
author: 继小鹏
date: 2016-12-11 05:53:29 +0800
category: JavaScript
tag: BOM
---
window对象是BOM的核心，window对象指当前的浏览器窗口。


####window对象



>window对象方法:



方法|描述
---|---
alert()|显示带有一段消息和确认按钮的警告框
prompt()|显示可提示用户的对话框
comfirm()|显示带有一段消息以及确认按钮和取消按钮的对话框
open()|打开一个新的浏览器窗口或查找一个已命名的窗口
close()|关闭浏览器窗口
print()|打印当前窗口内容
focus()|把键盘焦点给予一个窗口
blur()|把键盘焦从顶层窗口移开
moveBy()|可相对窗口的当前坐标把他移动指定的像素
moveTo()|把窗口的左上角移动到一个指定的坐标
resizeBo()|按照指定的像素调整窗口大小
resizeTo()|把窗口的大小调整到指定的高度和宽度
scrollBy()|按照指定的像素来滚动内容
scrollTo()|把内容滚动到指定的坐标
setInterval()|每隔指定的时间执行代码
setTimeout()|在指定的延迟时间之后来执行代码
clearInterval()|取消setInterval()的设置
clearTimeout()|取消setTimeout()的设置



	<input type="button" id="but" onclick="but()" value="按钮">
	<script type="text/javascript">
		function but() {
			window.open('http://www.huanghanlian.com/', '_blank', 'height=400,width=600,top=100,left=0');
		}
	</script>



>计时器setInterval()


在执行时,从载入页面后每隔指定的时间执行代码。

**语法:**

    setInterval(代码,交互时间);

**参数说明：**

1. 代码：要调用的函数或要执行的代码串。

2. 交互时间：周期性执行或调用表达式之间的时间间隔，以毫秒计（1s=1000ms）。

**返回值:**

一个可以传递给 clearInterval() 从而取消对"代码"的周期性执行的值。

**调用函数格式(**假设有一个clock()函数**):**

setInterval("clock()",1000)
或
setInterval(clock,1000)


例子1

	<form>
		<input type="text" id="clock" size="50"  />
	</form>
	<script type="text/javascript">
	var int = setInterval(clock, 100)

	function clock() {
		var time = new Date();
		document.getElementById("clock").value = time;
	}
	</script>


例子2

	<input type="text" id="clock" size="50"  />
	<script type="text/javascript">
		var attime;

		function clock() {
			var time = new Date();
			attime = time.getHours() + ":" + time.getMinutes() + ":" + time.getSeconds();
			document.getElementById("clock").value = attime;
		}
		setInterval(clock, 100)
	</script>



>取消计时器clearInterval()


clearInterval() 方法可取消由 setInterval() 设置的交互时间。

**语法：**

    clearInterval(id_of_setInterval)

**参数说明:**
id_of_setInterval：由 setInterval() 返回的 ID 值。

每隔 100 毫秒调用 clock() 函数,并显示时间。当点击按钮时，停止时间,代码如下:


	<form>
		<input type="text" id="clock" size="50"  />
		<input type="button" value="Stop" onclick="clearInterval(i)" />
	</form>
	<script type="text/javascript">
		function clock() {
			var time = new Date();
			document.getElementById("clock").value = time;
		}
		// 每隔100毫秒调用clock函数，并将返回值赋值给i
		var i = setInterval("clock()", 100);
	</script>


>计时器setTimeout()

setTimeout()计时器，在载入后延迟指定时间后,去执行一次表达式,仅执行一次。

**语法:**

    setTimeout(代码,延迟时间);

**参数说明：**

1. 要调用的函数或要执行的代码串。
2. 延时时间：在执行代码前需等待的时间，以毫秒为单位（1s=1000ms)。

当我们打开网页3秒后，在弹出一个提示框，**代码如下:**


	<script type="text/javascript">
		setTimeout("alert('Hello!')", 3000);
	</script>


当按钮start被点击时，setTimeout()调用函数，在5秒后弹出一个提示框。


	<form>
		<input type="button" value="start" onClick="tinfo()">
	</form>
	<script type="text/javascript">
		function tinfo() {
			var t = setTimeout("alert('Hello!')", 5000);
		}
	</script>


要创建一个运行于无穷循环中的计数器，我们需要编写一个函数来调用其自身。在下面的代码，当按钮被点击后，输入域便从0开始计数。


	<form>
		<input type="text" id="txt" />
		<input type="button" value="Start" onClick="numCount()" />
	</form>

	<script type="text/javascript">
		var num = 0;
		function numCount() {
			document.getElementById('txt').value = num;
			num = num + 1;
			setTimeout("numCount()", 1000);
		}
	</script>


或者


	<form>
		<input type="text" id="txt" />
		<input type="button" value="Start" onClick="numCount()" />
	</form>

	<script type="text/javascript">
		var num = 0;
		function numCount() {
			document.getElementById('txt').value = num;
			num = num + 1;
			setTimeout(numCount, 1000);
		}
	</script>


>取消计时器clearTimeout()

setTimeout()和clearTimeout()一起使用，停止计时器。

**语法:**

    clearTimeout(id_of_setTimeout)

**参数说明:**
**id_of_setTimeout：**由 setTimeout() 返回的 ID 值。该值标识要取消的延迟执行代码块。

下面的例子和上节的无穷循环的例子相似。唯一不同是，现在我们添加了一个 "Stop" 按钮来停止这个计数器：

	<!DOCTYPE html>
	<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>Document</title>
	</head>
	<body>
		<form>
		    <input type="text" id="txt">
		    <input type="button" value="Stop" onClick="stopCount()">
		</form>

		<script type="text/javascript">
		var num = 0,
			i;

		function timedCount() {
			document.getElementById('txt').value = num;
			num = num + 1;
			i = setTimeout(timedCount, 1000);
		}
		setTimeout(timedCount, 1000);

		function stopCount() {
			clearTimeout(i);
		}
		</script>
	</body>
	</html>



例子2

	<!DOCTYPE html>
	<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>Document</title>
	</head>
	<body>
	<form>
		<input type="text" id="count" />
		<input type="button" value="Start" onClick="startCount()" />
		<input type="button" value="Stop" onClick="stopCount()"  />
	</form>

	<script type="text/javascript">
		var num = 0;
		var i;

		function startCount() {
			clearTimeout(i);
			document.getElementById('count').value = num;
			num = num + 1;
			i = setTimeout("startCount()", 1000);
		}

		function stopCount() {
			clearTimeout(i);
		}
	</script>
	</body>
	</html>



####History 对象

history对象记录了用户曾经浏览过的页面(URL)，并可以实现浏览器前进与后退相似导航的功能。


**注意:从**窗口**被打开的那一刻开始记录，每个浏览器窗口、每个标签页乃至每个框架，都有自己的history对象与特定的window对象关联。**

**语法：**

    window.history.[属性|方法]

**注意：**window可以省略。


**History 对象属性**


属性|描述
---|---
length|返回浏览器历史列表中的url数量


**History 对象方法**



对象|描述
---|---
back()|加载history列表中的前一个url
forward()|加载history列表中的下一个url
go()|加载history列表中的某个具体页面



使用length属性，当前窗口的浏览历史总长度


	<script type="text/javascript">
		var HL = window.history.length;
		document.write(HL);
	</script>




>返回前一个浏览的页面


back()方法，加载 history 列表中的前一个 URL。

**语法：**

    window.history.back();

比如，返回前一个浏览的页面，**代码如下：**

    window.history.back();

**注意：等同于点击浏览器的倒退按钮。**

back()相当于go(-1),**代码如下:**

    window.history.go(-1);


	<input type="button" onclick="backs()" value="上一步">
	<script type="text/javascript">
		function backs(){
			window.history.back();
		}
	</script>


>返回下一个浏览的页面



forward()方法，加载 history 列表中的下一个 URL。

如果倒退之后，再想回到倒退之前浏览的页面，则可以使用forward()方法,**代码如下:**

    window.history.forward();

**注意：等价点击前进按钮。**

forward()相当于go(1),**代码如下:**

    window.history.go(1);



>返回浏览历史中的其他页面

go()方法，根据当前所处的页面，加载 history 列表中的某个具体的页面。

**语法：**

    window.history.go(number);

**参数：**


number|参数说明
---|---
1|前一个，go(1)等价forward()
0|当前页面
-1|后一个，go(-1)等价back()

浏览器中，返回当前页面之前浏览过的第二个历史页面，**代码如下：**

window.history.go(-2);

**注意：和在浏览器中单击两次后退按钮操作一样。**

同理，返回当前页面之后浏览过的第三个历史页面，**代码如下：**

    window.history.go(3);




####Location对象

location用于获取或设置窗体的URL，并且可以用于解析URL。

**语法:**

location.[属性|方法]

**location对象属性图示:**


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3877962-0cd937d9758dc31d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**location 对象属性：**


属性|描述
---|---
hash|设置或返回从井号(#)开始的url(锚)
host|设置或返回主机名和当前url的端口号
hostname|设置或返回当前url的主机名
href|设置或返回完整的url
pathname|设置或返回当前url的路径部分
port|设置或返回url的端口号
protocol|设置或返回当前url的协议
search|设置或返回从问号(?)开始的url(查询部分)


	<script type="text/javascript">
		console.log(window.location.hash);
		console.log(window.location.host);
		console.log(window.location.hostname);
		console.log(window.location.href);
		console.log(window.location.pathname);
		console.log(window.location.port);
		console.log(window.location.protocol);
		console.log(window.location.search);
	</script>

输出
      
	?1:9 
	?1:10 localhost
	?1:11 localhost
	?1:12 http://localhost/ajax_php/?1
	?1:13 /ajax_php/
	?1:14 
	?1:15 http:
	?1:16 ?1


**location 对象方法:**


属性|描述
---|---
assign()|加载新的文档
reload()|重新加载当前文档
replace()|用新的文档替换当前文档

	//刷新
	function reloadPage() {
		window.location.reload()
	}
	//刷新



####Navigator对象

Navigator 对象包含有关浏览器的信息，通常用于检测浏览器与操作系统的版本。

**对象属性:**


属性|描述
---|---
appCodeName;|浏览器代码名的字符串表示
appName;|返回浏览器的名称
appVersion;|返回浏览器的平台和版本信息
platform;|返回运行浏览器的操作系统平台
userAgent;|返回由客户机发送服务器的user-agent头部的值



查看浏览器的名称和版本，**代码如下:**


	var browser = navigator.appName;
	var version = navigator.appVersion;
	var appCode = navigator.appCodeName;
	var platSys = navigator.platform;
	var usersAg = navigator.userAgent;
	document.write("浏览器名称：" + browser + "<br />" + "平台和版本信息：" + version + "<br />" + "浏览器代码名称字符：" + appCode + "<br />" + "浏览器操作系统平台：" + platSys + "<br />" + "服务器头部的值：" + usersAg)



**userAgent**

返回用户代理头的字符串表示(就是包括浏览器版本信息等的字符串)

**语法**

    navigator.userAgent

几种浏览的user_agent.，像360的兼容模式用的是IE、极速模式用的是chrom的内核。


浏览器|userAgent
---|---
Chrome|Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2840.87 Safari/537.36
Firefox|Mozilla/5.0 (Windows NT 6.1; WOW64; rv:50.0) Gecko/20100101 Firefox/50.0
IE8|Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.1; WOW64; Trident/4.0; SLCC2; .NET CLR 2.0.50727; .NET CLR 3.5.30729; .NET CLR 3.0.30729; .NET4.0C; .NET4.0E)


使用userAgent判断使用的是什么浏览器

	<!DOCTYPE HTML>
	<html>
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
		<title>navigator</title>
		<script type="text/javascript">
			function validB() {
				var u_agent = navigator.userAgent;
				var B_name = "不是想用的主流浏览器!";
				if (u_agent.indexOf("Firefox") > -1) {
					B_name = "Firefox";
				} else if (u_agent.indexOf("Chrome") > -1) {
					B_name = "Chrome";
				} else if (u_agent.indexOf("MSIE") > -1 && u_agent.indexOf("Trident") > -1) {
					B_name = "IE(8-10)";
				}
				document.write("浏览器:" + B_name + "<br>");
				document.write("u_agent:" + u_agent + "<br>");
			}
		</script>
	</head>
	<body>
		<form>
		<input type="button" value="查看浏览器" onClick=validB()   >
		</form>
	</body>
	</html>



####screen对象

screen对象用于获取用户的屏幕信息。

**语法：**

    window.screen.属性

**对象属性:**


属性|描述
---|---
availHeight|窗口可以使用的屏幕高，单位像素
availWidth |窗口可以使用的屏幕宽，单位像素
colorDepth|用户浏览器表示的颜色位数，通常为32位(每像素的位数)
pixelDepth|用户浏览器表示的颜色位数，通常为32位(每像素的位数)(IE不支持)
height|屏幕的高，单位像素
width|屏幕的宽，单位像素

	<!DOCTYPE HTML>
	<html>
	<head>
	    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
	    <title>navigator</title>
	    <script type="text/javascript">
	        document.write("窗口可以使用的屏幕宽高: ");
	        document.write(window.screen.availWidth + "*" + screen.availHeight);
	        document.write("<br>");
	        document.write("屏幕的宽高: ");
	        document.write(window.screen.width + "*" + window.screen.height);
	        document.write("<br>");
	        document.write("用户浏览器表示的颜色位数: ");
	        document.write(window.screen.colorDepth);
	        document.write("<br>");
	        document.write("用户浏览器表示的颜色位数（ps：IE不支持？亲测好像支持唉）: ");
	        document.write(screen.pixelDepth);
	    </script>
	</head>
	<body>

	</body>
	</html>



BOM例子

	<!DOCTYPE html>
	<html>
	<head>
		<title>浏览器对象</title>
		<meta http-equiv="Content-Type" content="text/html; charset=gkb"/>
	</head>
	<body>
		<!--先编写好网页布局-->
		<h1>操作成功</h1>
		<p><span id="sd">5</span >秒后返回主页<a href="javascript:back();">返回</a> </p>

		<script type="text/javascript">
			var n = document.getElementById("sd").innerHTML;
			function count() {
				n--;
				document.getElementById("sd").innerHTML = n;
				if (n == 0) {
					location.assign("http://www.huanghanlian.com/");
				}

			}
			setInterval("count()", 1000);
			//通过window的location和history对象来控制网页的跳转。
			function back() {
				window.history.back();
			}
		</script>
	</body>
	</html>