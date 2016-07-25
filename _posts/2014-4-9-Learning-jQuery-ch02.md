---
layout: post
time: 2014-4-9
title: Learning jQuery第4版中文翻译-第2章选取元素
category: 《Learning jQuery》第4版学习笔记
keywords: jQuery, 概述,介绍
tags: jQuery, CSS, HTML
description: 《Learning jQuery》第4版第2章的中文翻译及学习笔记。
---

jQuery库利用了CSS选择器的强大功能，使开发者能够快速简单地获取文档对象模型(Document Object Model, DOM)中的元素或元素集合。这一章主要介绍了：

+ 页面的元素组织结构
+ 如何利用CSS选择器获取页面元素
+ 在CSS选择器基础上jQuery的一些扩展
+ DOM遍历方法，这些方法能够十分灵活的定位页面元素

# 理解DOM

jQuery最强大的功能之一就是能够非常简单的在DOM中获取元素。DOM作为JavaScript和页面之间的接口，将HTML源文件表达成对象之间连接形成的网络的形式，而不是纯文本。

这个网络像一棵家族树，页面元素是树的结点，因此，当我们表述页面元素之间的关系时，我们采用和家族成员之间的称谓一样的叫法，如父母，孩子等等。

<!--excerpt_separator-->

下面举一个简单的例子帮助理解，比如有如下的html文件：

{% highlight html %}
<html>
<head>
	<title>the title</title>
</head>

<body>
	<div>
		<p>This is a paragraph.</p>
		<p>This is another paragraph.</p>
		<p>This is yet another paragraph.</p>
	</div>
</body>
</html>
{% endhighlight %}

在这个页面里，`<html>`是所有其他元素的祖先，也就是说，其他元素都是`<html>`的后裔，而`<head>`和`<body>`不仅是`<html>`的后裔，还是`<html>`的孩子，`<html>`是它们的父母。`<p>`是`<div>`的孩子和后裔，是`<head>`和`<body>`的后裔，是其他`<p>`的兄弟。

下面这张图用DOM树清楚表示了页面元素之间的关系：

![img](/assets/image/posts/learning-jquery/ch02-1.jpg)

有了这课树，我们就能够使用jQuery的选择器和遍历方法有效地定位页面元素。

# 使用$()

jQuery选择器和方法返回的元素集合常常被表示为一个jQuery对象[^1]，这样有助于我们操控元素，进行处理和修改，我们可以为它们绑定事件，比如添加鼠标点击效果，或者将多个修改操作链起来完成。

[^1]: 注意：jQuery对象和普通的DOM元素及结点列表是不一样的，因此对于一些功能来说，为它们提供了不同的方法。在这一章的最后部分会介绍一些直接获取jQuery对象内部DOM元素的方法。

我们使用`$()`方法来创建一个新jQuery对象，这个方法接收一个CSS选择器作为唯一参数，然后返回一个新的jQuery对象，这个对象指向页面中对应的元素。样式表中使用的选择器都可以作为一个string传给`$()`[^2].

[^2]: 在jQuery中，美元符号(\$)只是jQuery的别名。由于\$()方法在JavaScript库中也很常用，因为当我们在一个页面中同时使用对于一种类似的库时就会发生冲突，这时可以将jQuery代码中的\$替换为jQuery来避免冲突。其他解决方法会在第10章介绍。

基本的选择器有三种：标签名、ID和类。它们可以单独使用，或者和其他结合来用。下表展示三种选择器的用法：

选择器类型 | CSS           | jQuery           | 功能                          |
-----------|---------------|------------------|-------------------------------|
标签名     | p{}           | $('p')           | 取出页面中的所有段落元素      |
ID         | #some-id{}    | $('#some-id')    | 取出ID为some-id的那个元素     |
类         | .some-class{} | $('.some-class') | 取出所有含有some-class类的元素|

</br>

了解了选择器的基础内容后，下面介绍一些进阶功能。

# CSS选择器

jQuery库支持几乎所有CSS1-3的选择器。只要浏览器支持JavaScript，开发者就可以强化他们的页面效果，而不用担心浏览器是否会不支持高级选择器[^3]。

[^3]: 靠谱的jQuery开发者应时常将渐进增强和优雅降级的概念应用到代码中，使得无论用户是否支持JavaScript，页面都能够呈现同样准确的内容，即使不是同样的“漂亮”。渐进增强：从被所有浏览器支持的基本功能开始，逐步地添加那些只有新式浏览器才支持的功能。优雅降级：首先构建站点的完整功能，使其在所有新式浏览器中都能正常工作，然后针对老式浏览器进行测试和修复。

下面使用嵌套的无序列表结构来展示jQuery如何和CSS选择器一起协同工作，这一结构经常出现在网站的导航栏中。

{% highlight html %}
<!-- learning-jquery-ch02.html -->
<!DOCTYPE html>

<html>
<head>
	<meta charset="utf-8">
	<title>Selected Shakespeare Plays</title>

	<link rel="stylesheet" href="styles/learning-jquery-ch02.css" type="text/css" />

	<script src="scripts/jquery-1.11.0.min.js"></script>
	<script src="scripts/learning-jquery-ch02.js"></script>
</head>

<body>
	<div id="container">
		<h2>Selected Shakespeare Plays</h2>
		<ul id="selected-plays">
			<li>Comedies
				<ul>
					<li><a href="/asyoulikeit/">As You Like It</a></li>
					<li>All's Well That Ends Well</li>
					<li>A Midsummer Night's Dream</li>
					<li>Twelfth Night</li>
				</ul>
			</li>
			<li>Tragedies
				<ul>
					<li><a href="hamlet.pdf">Hamlet</a></li>
					<li>Macbeth</li>
					<li>Romeo and Juliet</li>
				</ul>
			</li>
			<li>Histories
				<ul>
					<li>Henry IV (<a href="mailto:henryiv@king.co.uk">email</a>)
						<ul>
							<li>Part I</li>
							<li>Part II</li>
						</ul>
					</li>
					<li><a href="http://www.shakespeare.co.uk/henryv.htm">Henry V</a></li>
					<li>Richard II</li>
				</ul>
			</li>
		</ul>
	</div>
</body>

</html>
{% endhighlight %}

整个列表有一个ID selecting-plays,而其他列表项都没有类，没有应用任何样式时，列表是这样的：

![img](/assets/image/posts/learning-jquery/ch02-2.jpg)

嵌套列表以我们期望的方式呈现，项目符号垂直排列，并根据嵌套层次水平缩进。

## 给列表项层次添加样式

假如我们希望最顶层的列表项-**Comedies**,**Tragedies**,**Histories**水平放置，我们可以先在样式表中定义一个`horizontal`类：

{% highlight CSS %}
/*learning-jquery-ch02.css*/
.horizontal {
     float: left;
     list-style: none;
     margin: 10px;
}
{% endhighlight %}

`horizontal`类将元素一个挨一个靠左边放置，消除列表项的项目符号，并添加10-pixel的边界。

为了展示jQuery选择器的使用方法，我们不在html中直接添加`hotizontal`类，而是在jQuery代码中动态地向顶层列表项添加类：

{% highlight javascript %}
// learning-jquery-ch02.js
$(document).ready( function() {
     $('#selected-plays > li').addClass('horizontal');
});
{% endhighlight %}

如[第1章][1]中介绍的那样，我们的jQuery代码首先调用`$(document).ready()`，这个函数当页面被加载之后立即执行传给它的函数。第二行代码使用子对象选择符`>`来取出所有的顶层列表项（即selected-plays的下一层子元素中的列表项）并把`horizontal`类附给它们，这样，样式表中的定义就起作用了，顶层列表项变为了水平排列，如图：

![img](/assets/image/posts/learning-jquery/ch02-3.jpg)

[1]: http://pinkings.github.io/%E3%80%8Alearning%20jquery%E3%80%8B%E7%AC%AC4%E7%89%88%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/2014/04/02/Learning-jQuery-ch1.html

为其他列表项添加样式（除了顶层列表项之外的）可以由几种方法实现。由于我们已经为顶层列表项添加了`horizontal`类，一种取出所有其他列表项的方法就是使用否定伪类，即取出所有不含`horizontal`类的列表项。注意下面新添加的第三行代码：

{% highlight javascript %}
// learning-jquery-ch02.js
$(document).ready( function() {
     $('#selected-plays > li').addClass('horizontal');
    $('#selected-plays li:not(.horizontal)').addClass('sub-level');
});
{% endhighlight %}

这一次我们取出所有符合下列条件的列表项(<li\>)：

+ 是ID为selected-plays的元素的后裔元素 (`#selected-plays`)
+ 不含有类hotizontal (`:not(.horizontal)`)

并在样式表中添加`sub-level`类的样式：

{% highlight CSS %}
/*learning-jquery-ch02.css*/
.sub-level {
	background: #ccc;
}
{% endhighlight %}

这样当我们为这些列表项添加了`sub-level`类时，它们就会加上灰色背景的效果，如图：

![img](/assets/image/posts/learning-jquery/ch02-3.jpg)

# 属性选择器

属性选择器是CSS选择器的一个十分有用的子集，我们可以通过元素的HTML属性来指定某个元素，比如链接的title属性或者图像的alt属性。比如，要选取出所有带有alt属性的图像，只需写成：

{% highlight javascript %}
$('img[alt]')
{% endhighlight %}

## 给链接添加样式

属性选择器受正则表达式的启发，接受通配符语法来识别字符串起始(\^)或末尾(\$)的字符，还可以用星号(\*)来表达字符串内任意位置的值，及惊叹号(\!)来表达取反的含义。

比如我们想要对不同类型的链接添加不同的样式，我们首先在样式表中添加定义：

{% highlight CSS %}
a {
	color: #00c;
}

a.mailto {
	background: url(../images/email.png) no-repeat right top;
	padding-right: 18px;
}

a.pdflink {
	background: url(../images/pdf.png) no-repeat right top;
	padding-right: 18px;
}

a.henrylink {
	background-color: #fff;
	padding: 2px;
	border: 1px solid #000;
}
{% endhighlight %}

然后我们用jQuery将这三个类——**mailto**,**pdflink**,**henrylink**加到适合的链接中。

给所有的邮件链接添加类，我们需要构造一个选择器，这个选择器寻找所有带有以mailto(`^="mailto:"`)开头的`href`属性的链接元素(a)，如下：

{% highlight javascript %}
$(document).ready( function() {
     $('a[href^="mailto:"]').addClass('mailto');
} );
{% endhighlight %}

由于我们之前定义了样式表，因此信封小图标会出现在邮件链接的后面。

![img](/assets/image/posts/learning-jquery/ch02-5.jpg)

同理，我们寻找href属性以.pdf结尾(`$=".pdf"`)的pdf文件链接，在后面添加一个Adobe Acrobat小图标。

{% highlight javascript %}
$(document).ready( function() {
     $('a[href^="mailto:"]').addClass('mailto');
     $('a[href$=".pdf"]').addClass('pdflink');
} );
{% endhighlight %}

![img](/assets/image/posts/learning-jquery/ch02-6.jpg)

属性选择器也可以结合来用，比如我们要将**henrylink**类加到所有以http开头并且包含henry字符的链接中：

{% highlight javascript %}
$(document).ready( function() {
     $('a[href^="mailto:"]').addClass('mailto');
     $('a[href$=".pdf"]').addClass('pdflink');
     $('a[href^="http"][href*="henry"]').addClass('henrylink');
} );
{% endhighlight %}

![img](/assets/image/posts/learning-jquery/ch02-7.jpg)

# 自定义选择器

除了CSS选择器，jQuery还添加了自己的自定义选择器[^4]，这些自定义选择器以新的方式强化了已有的CSS选择器的功能。

[^4]: jQuery会尽可能使用原始的DOM选择器引擎来寻找页面元素，但是当使用了自定义选择器时，这种原始的极快的方式就不能使用了，因此建议，当原始方式可以达到效果，并且十分关注性能时，避免频繁使用自定义选择器。

大部分自定义选择器可以帮助我们在已经找到的一系列元素中选择其中的一个或几个。自定义选择器的语法和CSS伪类语法一样，以冒号(\:)开头。比如，要选出类horizontal的div元素中的第二个元素，可以这样写：

{% highlight javascript %}
$('div.horizontal:eq(1)')
{% endhighlight %}

注意`:eq(1)`取出集合中的第二个元素，因为javascript数组是从0开始的。相反的，CSS是从1开始的，因此一个CSS选择器`$('div:nth-child(1)')`会取出所有是其父结点的第1个孩子的div元素。不清楚的可以参考[jQuery API文档][2]。

[2]: http://api.jquery.com/category/selectors/ "http://api.jquery.com/category/selectors/"

## 给交替行添加样式

两个十分有用的jQuery自定义选择器是`:odd`和`:even`，我们以下面这个例子来说明：

{% highlight html %}
<!-- learning-jquery-ch02.html -->
<!DOCTYPE html>

<html>
<head>
	<meta charset="utf-8">
	<title>Selected Shakespeare Plays</title>

	<link rel="stylesheet" href="styles/learning-jquery-ch02.css" type="text/css" />

	<script src="scripts/jquery-1.11.0.min.js"></script>
	<script src="scripts/learning-jquery-ch02.js"></script>
</head>

<body>
	<div id="container">

		<h2>Shakespeare's Plays</h2>
		<table>
			<tr>
				<td>As You Like It</td>
				<td>Comedy</td>
				<td></td>
			</tr>
			<tr>
				<td>All's Well that Ends Well</td>
				<td>Comedy</td>
				<td>1601</td>
			</tr>
			<tr>
				<td>Hamlet</td>
				<td>Tragedy</td>
				<td>1604</td>
			</tr>
			<tr>
				<td>Macbeth</td>
				<td>Tragedy</td>
				<td>1606</td>
			</tr>
			<tr>
				<td>Romeo and Juliet</td>
				<td>Tragedy</td>
				<td>1595</td>
			</tr>
			<tr>
				<td>Henry IV, Part I</td>
				<td>History</td>
				<td>1596</td>
			</tr>
			<tr>
				<td>Henry V</td>
				<td>History</td>
				<td>1599</td>
			</tr>
		</table>
		<h2>Shakespeare's Sonnets</h2>
		<table>
			<tr>
				<td>The Fair Youth</td>
				<td>1–126</td>
			</tr>
			<tr>
				<td>The Dark Lady</td>
				<td>127–152</td>
			</tr>
			<tr>
				<td>The Rival Poet</td>
				<td>78–86</td>
			</tr>
		</table>

	</div>
</body>

</html>
{% endhighlight %}

不添加样式时：

![img](/assets/image/posts/learning-jquery/ch02-8.jpg)

我们想给这个表格添加一个条纹的效果。首先在CSS中为奇数行定义alt类：

{% highlight CSS %}
/* learning-jquery-ch02.css */
tr {
	background-color: #fff;
}

.alt {
	background-color: #ccc;
}
{% endhighlight %}

然后在jQuery代码中为所有奇数行添加alt类

{% highlight javascript %}
// learning-jquery-ch02.js
$(document).ready( function() {
	$('tr:even').addClass('alt');
} );
{% endhighlight %}

等等！为什么对奇数行使用`:even`呢？原因就和`:eq()`一样，因为javascript是以0开始的，因此第一行的标号为0(偶数)而第二行的标号为1(奇数)...因此效果如图：

![img](/assets/image/posts/learning-jquery/ch02-9.jpg)

注意第二段的第一行是白色背景，因此第一段的最后一行是灰色背景。如果想避免这种情况，一种办法是使用`"nth-child()`选择器来替代，这种选择器相对于元素各自的父结点来计数，而不是相对于所有被取出的元素计数，这个选择器可以接受`odd`或者`even`作为参数。

{% highlight javascript %}
// learning-jquery-ch02.js
$(document).ready( function() {
	$('tr:nth-child(odd)').addClass('alt');
} );
{% endhighlight %}

这里为了达到和前面相同的效果，要使用`odd`，因为`:nth-child()`是唯一一个以1开始的jQuery选择器。效果如图：

![img](/assets/image/posts/learning-jquery/ch02-10.jpg)

## 根据文本内容查找元素

作为最后一个jQuery自定义选择器的例子，假如出于某些原因我们想要高亮所有**Henry**戏剧，我们需要做的工作只是在CSS中定义一个斜体加粗的类`.highlight{font-weight:bold;font-style:italic;}`之后添加一行jQuery代码：

{% highlight javascript %}
// learning-jquery-ch02.js
$(document).ready( function() {
	$('td:contains(Henry)').addClass('highlight');
} );
{% endhighlight %}

不可否认，不适用jQuery或其他客户端编程也有很多方法可以达到表格条纹化及文字高亮的效果，但是，当表格的内容是自动生成的，我们并不能从html文档或服务端代码获取相关信息时，jQuery结合CSS的方法能够很好的达到效果。

## 表单选择器

自定义选择器的能力不仅限于根据元素的位置来定位元素。比如当处理表单时，jQuery的自定义选择器和互补的CSS3选择器能够简化我们选取元素的工作。下面的表格列出了一些表单选择器：

选择器    | 匹配元素                                          |
----------|---------------------------------------------------|
:input    | 输入域，文本域，选择列表，按钮元素                |
:button   | 带有值为'button'的'type'属性的按钮元素和输入域元素|
:enabled  | 启用的表单元素                                    |
:disabled | 禁用的表单元素                                    |
:checked  | 选中的单选按钮和复选框元素                        |
:selected | 选中的下拉列表中的元素                            |

</br>

表单选择器可以和其他选择器结合来发挥更多的功能，比如取出所有选中的单选按钮（非复选框按钮）`$('input[type="radio"]:checked')`，或者取出所有密码输入和禁用的文字输入域元素`$('input[type="password"],input[type="text"]:disabled')`.[^5]

[^5]: 这里只是介绍一些简单的选择器表达式，更多的将在第9章介绍。


# To Be Continued ...
