---
layout: post
title: 献给写作者的 Markdown 新手指南
category: Markdown
tag: Markdown
---


###标题

这是最为常用的格式，在平时常用的的文本编辑器中大多是这样实现的：输入文本、选中文本、设置标题格式。

而在 Markdown 中，你只需要在文本前面加上 # 即可，同理、你还可以增加二级标题、三级标题、四级标题、五级标题和六级标题，总共六级，只需要增加 # 即可，标题字号相应降低。例如：



	# 一级标题
	## 二级标题
	### 三级标题
	#### 四级标题
	##### 五级标题
	###### 六级标题

###列表

列表格式也很常用，在 Markdown 中，你只需要在文字前面加上 - 就可以了，例如：

	- 文本1
	- 文本2
	- 文本3

- 文本1
- 文本2
- 文本3

如果你希望有序列表，
也可以在文字前面加上 1. 2. 3. 就可以了，例如：

	1. 文本1
	2. 文本2
	3. 文本3

1. 文本1
2. 文本2
3. 文本3

###链接和图片

在 Markdown 中，插入链接不需要其他按钮，你只需要使用 [显示文本](链接地址) 这样的语法即可，例如：

	[简书](http://www.jianshu.com)

在 Markdown 中，插入图片不需要其他按钮，你只需要使用 `![](图片链接地址)` 这样的语法即可，例如：

	![](http://ww4.sinaimg.cn/bmiddle/aa397b7fjw1dzplsgpdw5j.jpg)


注：插入图片的语法和链接的语法很像，只是前面多了一个 `！`。

插入链接和图片的案例截图：

![](http://ww4.sinaimg.cn/bmiddle/aa397b7fjw1dzplsgpdw5j.jpg)

###引用

在我们写作的时候经常需要引用他人的文字，这个时候引用这个格式就很有必要了，在 Markdown 中，你只需要在你希望引用的文字前面加上 `>`就好了，例如：

	> 一盏灯， 一片昏黄； 一简书， 一杯淡茶。 守着那一份淡定， 品读属于自己的寂寞。 保持淡定， 才能欣赏到最美丽的风景！ 保持淡定， 人生从此不再寂寞。

注：`>` 和文本之间要保留一个字符的空格。

最终显示的就是：

> 一盏灯， 一片昏黄； 一简书， 一杯淡茶。 守着那一份淡定， 品读属于自己的寂寞。 保持淡定， 才能欣赏到最美丽的风景！ 保持淡定， 人生从此不再寂寞。

粗体和斜体

Markdown 的粗体和斜体也非常简单，用两个 * 包含一段文本就是粗体的语法，用一个 `*` 包含一段文本就是斜体的语法。例如：

	*一盏灯*， 一片昏黄；**一简书**， 一杯淡茶。 守着那一份淡定， 品读属于自己的寂寞。 保持淡定， 才能欣赏到最美丽的风景！ 保持淡定， 人生从此不再寂寞。

最终显示的就是下文，其中「一盏灯」是斜体，「一简书」是粗体：

*一盏灯*， 一片昏黄；**一简书**， 一杯淡茶。 守着那一份淡定， 品读属于自己的寂寞。 保持淡定， 才能欣赏到最美丽的风景！ 保持淡定， 人生从此不再寂寞。

###代码引用
	####代码引用
	`hello world`
	####多段代码引用
	```
	第一
	第二
	第三
	```

需要引用代码时，如果引用的语句只有一段，不分行，可以用 ` 将语句包起来。
如果引用的语句为多行，可以将```置于这段代码的首行和末行。

最终显示的就是：

####代码引用
`hello world`
####多段代码引用

	第一
	第二
	第三

###表格

相关代码：

	| Tables        | Are           | Cool  |
	| ------------- |:-------------:| -----:|
	| col 3 is      | right-aligned | $1600 |
	| col 2 is      | centered      |   $12 |
	| zebra stripes | are neat      |    $1 |

显示效果：

| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |


相关代码：

	dog | bird | cat
	----|------|----
	foo | foo  | foo
	bar | bar  | bar
	baz | baz  | baz

显示效果：

dog | bird | cat
----|------|----
foo | foo  | foo
bar | bar  | bar
baz | baz  | baz


###显示链接中带括号的图片

![][1]

[1]: http://latex.codecogs.com/gif.latex?\prod%20\(n_{i}\)+1

代码如下:

	![][1]
	[1]: http://latex.codecogs.com/gif.latex?\prod%20\(n_{i}\)+1

![][1]

[1]: http://latex.codecogs.com/gif.latex?\prod%20\(n_{i}\)+1



This is [an example](http://example.com/ "Title") inline link.

[This link](http://example.net/) has no title attribute.


> This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet,
> consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus.
> Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.
> 
> Donec sit amet nisl. Aliquam semper ipsum sit amet velit. Suspendisse
> id sem consectetuer libero luctus adipiscing.

*   Red
*   Green
*   Blue

*   Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
    Aliquam hendrerit mi posuere lectus. Vestibulum enim wisi,
    viverra nec, fringilla in, laoreet vitae, risus.
*   Donec sit amet nisl. Aliquam semper ipsum sit amet velit.
    Suspendisse id sem consectetuer libero luctus adipiscing.


这是一个普通段落：

    这是一个代码区块。

* * *

***

*****

- - -

---------------------------------------

\   反斜线

`   反引号

*   星号

_   底线

{}  花括号

[]  方括号

()  括弧

#   井字号

+   加号

-   减号

.   英文句点

!   惊叹号