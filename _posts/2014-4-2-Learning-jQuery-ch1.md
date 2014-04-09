---
layout: post
time: 2014-4-2
title: Learning jQuery第4版中文翻译-第1章入门介绍
category: 《Learning jQuery》第4版学习笔记
keywords: jQuery, 概述,介绍
tags: jQuery, CSS, HTML
description: 《Learning jQuery》第4版第1章的学习笔记。
---

#jQuery能够做什么？#

###获取元素

如果想要遍历DOM树或者定位到HTML文件结构的某个部分，JavaScript需要写很多行代码才能实现，jQuery提供了十分简易有效的选择器机制来准确地获得文件的某一部分元素用于审查或者操控。

{% highlight javascript %}
$('div.content').find('p'); //找到ID为content的div元素的后代段落元素
{% endhighlight %}

###改变网页的外观

CSS能够影响网页的渲染效果，但是需要考虑不同浏览器的兼容性问题，因为并不是所有的浏览器都依赖于同一套标准。jQuery依赖于同一套标准，因此在不同浏览器中能够得到同样的显示效果。jQuery可以改变部分元素的类或者样式属性，甚至在网页已经加载之后也可以改变。

{% highlight javascript %}
$('ul > li:first').addClass('active'); //向无序列表的第一个子表项添加active类
{% endhighlight %}


###改变网页内容

不只是表面的一些改变，jQuery还可以通过简单的API改变网页的内容，可以添加文字、插入或者替换图片、将列表重新排序，甚至整个文件的结构都可以被重写和扩展。

{% highlight javascript %}
$('#container').append('<a href="more.html">more</a>'); //在container类里面加上一个more链接
{% endhighlight %}


###响应用户的交互

jQuery能够拦截各种事件（如用户点击某个链接）并做相应处理，它的事件处理API能够消除不同浏览器间的不一致性（不同浏览器的兼容经常困扰网页开发人员）。

{% highlight javascript %}
$('button.show-details').click(function() { //当用户点击ID为show-details的按钮时
	$('div.details').show(); //显示ID为details的div元素
});
{% endhighlight %}


###显示网页变化的动态效果

为了有效地实现这些交互行为，设计者还必须给予用户一些视觉反馈。jQuery库提供了一系列动画效果，如淡入淡出和擦除，还有一个工具包用于制作自己的动画。

{% highlight javascript %}
$('div.details').slideDown(); //让ID为details的div元素向下滑出
{% endhighlight %}


###从服务端获取数据而不刷新页面

这种编码模式叫做`Ajax`,最初代表Asynchronous JavaScript and XML,不过现在代表了一系列服务端与客户端之间的沟通技术。jQuery消除了特定浏览器的复杂性，使开发人员关注于服务端的功能。

{% highlight javascript %}
$('div.details').load('more.html #content');  //通过AJAX请求从服务器加载more.html的content类，并把返回的数据放置到ID为details的div元素里
{% endhighlight %}


###简化常见的JavaScript工作

除了对文件的一些操作，jQuery还能强化一些JavaScript结构，如迭代和数组操作。

{% highlight javascript %}
$.each(obj, function(key, value) { //对于每一个obj
	total += value; //将其value值加到total中
});
{% endhighlight %}


#为什么jQuery如此有效#

随着人们对于动态HTML兴趣的复苏，JavaScript框架开始被扩展。有些人专注于上面提到的一两个功能，有些人试图归类所有可能的行为和动画并将其打包。为了保持原有的许多特性，同时又尽可能压缩容量，jQuery采用如下一些策略：

### 利用CSS知识

在CSS选择器定位页面元素机制的基础上，jQuery继承了这种简明易懂的描述文档结构的方式。对于那些想要给自己的页面添加一些动画的开发者来说，jQuery成为了一个很好的入口，因为作为一名专业的网页开发者其必须掌握的就是CSS语法。

### 支持扩展

为了避免特征蠕动[^1]，jQuery将一些特殊功能转移给插件(plugin)来完成。创建新插件的方法非常简单，且有很好的指导文档，它促使很多创造性的有用模块被开发出来。

[^1]: 特征蠕动（Feature Creep），有时候也称为需求漂移或者是范围蠕动，是产品或项目的需要在除了最开始的预见之外，在开发过程中还产生了新的要求的趋势，它导致了刚开始没有计划到的特征，对产品的质量和计划也带来了风险。特征蠕动可能是由客户增长的“需求列表”或者是开发者本身看到改进该产品的机会所带来的。

### 抽象出浏览器怪癖

Web开发的一个不幸的事实是，每一个浏览器相对于标准都有一些偏离。任何web应用的很大一部分工作都用来兼容不同平台上的互异特征，而浏览器的不断发展使得这些基于特定浏览器的代码难以兼容新的特性。jQuery添加了一个抽象层，统一了通用功能，同时减少了代码量，极大的简化了开发者的工作。

### 总是使用“集合”

当我们使用jQuery来找到所有属于collapsible类的元素并且隐藏它们时，不用遍历每一个返回的元素然后一个个隐藏它们，如`.hide()`的一些方法能够直接用于元素集合。这种技术叫做“隐式迭代”，意味着许多循环结构都不用开发者自己写了，从而缩短了代码。

### 允许将多个操作放在一行完成

为了避免临时变量的使用和冗余的重复代码，jQuery的大部分方法都采用一种叫做“链接”(chaining)的编码模式，即对一个对象的大部分操作的结果就是对象自身，可以直接作为另一个操作的输入。

这些策略使得jQuery库尽可能小，同时也使得用户自己的应用代码能够尽可能地简短。jQuery库是开源免费的。

#创建第一个应用jQuery的网页#

##下载jQuery库

jQuery是脚本语言，无需编译链接，直接从网上下载一份jQuery库的代码，并在HTML文件的`<script>`中包含它即可。jQuery的官方网站直接从[jQuery官网][1]上下载最新版本，网站上的非压缩版本用于开发者查看，压缩版本一般用于发布环境。除了从网上下载一份jQuery拷贝放在应用服务器上，也可以通过一些公司的CDNs(Content Delivery Networks)来获取，用的最多的是**[Google CDN][2]**, **[Microsoft CDN][3]**和**[jQuery项目网站][4]**。这些网站在各地提供了强大的，低延迟的服务器，使得用户无论在哪儿都能快速下载文件。从CDN上获取jQuery相对于自己服务器上放置jQuery来说在速度上有优势，但使用本地的拷贝在开发阶段会更方便一些。

[1]: http://jquery.com/ "http://jquery.com/"
[2]: https://developers.google.com/speed/libraries/devguide "https://developers.google.com/speed/libraries/devguide"
[3]: http://www.asp.net/ajaxlibrary/cdn.ashx "http://www.asp.net/ajaxlibrary/cdn.ashx"
[4]: http://code.jquery.com "http://code.jquery.com"

##jQuery版本的选择

以前，jQuery的最好的版本就是最新发布的版本，然而jQuery v2.0发布之后，开发者需要稍稍考虑一下。jQuery v2.0不再支持IE(6, 7, 8)从而为更现代的浏览器提供更加快速、简洁的支持。jQuery v1.x仍然支持这些旧版本的IE浏览器。

另外，使用jQuery v1.9版本之前的开发者需要使用[jQuery Migrate plugin][5]来兼容jQuery v1.10。

[5]: http://jquery.com/upgrade-guide/1.9/#jquery-migrate-plugin "http://jquery.com/upgrade-guide/1.9/#jquery-migrate-plugin" 

##在HTML文件中设置jQuery
下载最新版本的jQuery库，jquery-1.11.0.min.js，放在项目根目录的scripts目录下。我们要写的jQuery文件名为learning-jquery-ch01.js，也放在scripts目录下。本章的第一个例子是一本书的摘录，html文件为：

{% highlight html %}
<!-- learning-jquery-ch01.html -->
<!DOCTYPE html>

<html lang="en">
<head>
	<meta charset="utf-8">
	<title>Through the Looking-Glass</title>

	<link rel="stylesheet" href="styles/learning-jquery-ch01.css" type="text/css" />

	<script src="scripts/jquery-1.11.0.min.js"></script>
	<script src="scripts/learning-jquery-ch01.js"></script>

</head>

<body>
	<div id="container">
		<h1>Through the Looking-Glass</h1>
		<div class="author">by Lewis Carroll</div>

		<div class="chapter" id="chapter-1">
			<h2 class="chapter-title">1. Looking-Glass House</h2>
			<p>There was a book lying near Alice on the table, and while she sat watching the White King (for she was still a little anxious about him, and had the ink all ready to throw over him, in case he fainted again), she turned over the leaves, to find some part that she could read, <span class="spoken">“—for it’s all in some language I don’t know,”</span> she said to herself.</p>
			<p>It was like this.</p>
			<div class="poem">
				<h3 class="poem-title">YKCOWREBBAJ</h3>
				<div id="fred" class="poem-stanza">
					<div>sevot yhtils eht dna ,gillirb sawT'</div>
					<div>;ebaw eht ni elbmig dna eryg diD</div>
					<div>,sevogorob eht erew ysmim llA</div>
					<div>.ebargtuo shtar emom eht dnA</div>
				</div>
			</div>
			<p>She puzzled over this for some time, but at last a bright thought struck her. <span class="spoken">“Why, it’s a Looking-glass book, of course! And if I hold it up to a glass, the words will all go the right way again.”</span></p>
			<p>This was the poem that Alice read.</p>
			<div class="poem">
				<h3 class="poem-title">JABBERWOCKY</h3>
				<div class="poem-stanza">
					<div>'Twas brillig, and the slithy toves</div>
					<div>Did gyre and gimble in the wabe;</div>
					<div>All mimsy were the borogoves,</div>
					<div>And the mome raths outgrabe.</div>
				</div>
			</div>
		</div>
	</div>

</body>
</html>
{% endhighlight %}

相应的CSS文件为：

{% highlight CSS %}

/* learning-jquery-ch01.css */
body {
	font-family: Helvetica, Arial, sans-serif;
	color: #000;
	background: #fff;
}

h1, h2, h3 {
	margin-bottom: .2em;
}

.poem {
	margin: 0 2em;
}

.highlight {
	background-color: #ccc;
	border: 1px solid #888;
	font-style: italic;
	margin: 0.5em 0;
	padding: 0.5em;
}
{% endhighlight %}

目前为止的页面结果如图：

![img](/assets/image/posts/learning-jquery/ch01-1.jpg)

我们可以使用jQuery来为诗歌部分的文字添加新的样式。

##添加我们的jQuery代码

我们自己的代码放在新的js文件中，learning-jquery-ch01.js。在这个例子中，我们只需要添加三行代码：

{% highlight javascript %}
/* learning-jquery-ch01.js */
$(document).ready( function() {
	$('div.poem-stanza').addClass('highlight');
});
{% endhighlight %}

我们来看看这段代码做了什么工作。

**找到诗歌部分的文字**

jQuery最基本的功能就是取出页面的一部分元素。这一功能是用`$()`来实现的，一般括号里以字符串作为参数，可以是任意的CSS选择器表达式。在这个例子里，`div.poem-stanza`表示选择出所有div元素里属于poem-stanza类的元素。这里的选择器比较简单，在第2章Selecting Elements中会涉及到一些非常复杂的选择器相关内容。

`$()`会返回一个新的jQuery对象实例，它封装了我们选择出来的DOM元素（0个或多个），然后我们就可以以很多方式操纵它们。在这个例子里，我们需要修改诗歌正文部分的显示方式，因此首先将诗歌正文部分元素选择出来。

**插入新的类**

![img](/assets/image/posts/learning-jquery/ch01-2.jpg)

`.addClass()`方法为我们选择出来的元素添加CSS类，输入参数可以是要添加的类名字符串（1个或多个），也可以是一个函数（jQuery v1.4以后），该函数返回要添加的类名。同样，`.removeClass()`可以移除类，用法和`.addClass()`一样。

{% highlight javascript %}
// some examples of .addClass() and .removeClass() method
$( "p" ).addClass( "myClass yourClass" ); //example 1
$( "p" ).removeClass( "myClass noClass" ).addClass( "yourClass" ); //example 2
$( "ul li" ).addClass(function( index ) { //example 3
	return "item-" + index;
});
{% endhighlight %}

注意这里我们并不需要写迭代循环来对每一个选出来的元素添加或者移除类，jQuery的隐式迭代实现了这一功能，我们只需要一次调用就可以作用于所有选择出来的元素。这个例子中，将选择出来的诗歌正文部分元素使它们属于highlight类（highlight类的样式已经在CSS文件中设定，灰色背景和一个边框）。

**执行代码**

`$()`和`.addClass()`结合起来就足以实现我们需要的效果了，但是如果我们将这一行代码直接写在js文件的开头，将得不到任何效果。浏览器遇到javascript代码就会立即执行它，而此时浏览器正在处理头部，页面的DOM树还没有准备好，因此我们需要延迟jQuery代码的执行。

通过`$(document).ready()`方法，jQuery允许我们一旦DOM被载入就执行里面的jQuery代码（而非等到所有图片完全加载出来）。本章例子中传给`.ready()`的参数是匿名函数[^2]，这种方法写起来方便，适用于无需**复用**的函数。`.ready()`的参数也可以是一个已经定义的函数名，如：

{% highlight javascript %}
function addHighlightClass() {
    $('div.poem-stanza').addClass('highlight');
}

$(document).ready(addHighlightClass);
{% endhighlight %}

[^2]: 匿名函数，即function()后面直接加上函数的实现

**完成后的作品**

加入jQuery代码之后，页面变成了这个样子：

![img](/assets/image/posts/learning-jquery/ch01-3.jpg)

由于我们在jQuery代码中给它们添加了`highlight`类，诗歌正文部分变为斜体，被包围在一个框里，就像我们在CSS文件中定义的那样。

#纯JavaScript代码 vs jQuery代码

本例的功能也能用纯javascript代码来完成：

{% highlight javascript %}
window.onload = function() {
	var divs = document.getElementsByTagName('div');
	for (var i = 0; i < divs.length; i++) {
		if (hasClass(divs[i], 'poem-stanza')  && !hasClass(divs[i], 'highlight')) {
			divs[i].className += ' highlight';
		}
	}

	function hasClass( elem, cls ) {
		var reClass = new RegExp(' ' + cls + ' ');
		return reClass.test(' ' + elem.className + ' ');
	}
};
{% endhighlight %}

纯javascript的代码要比jQuery繁复的多，另外，纯javascript代码不能处理许多jQuery会为我们解决的问题，比如：

+ 恰当地考虑其他`window.onload`事件处理程序
+ 当DOM准备好时迅速执行
+ 利用现代DOM方法优化元素的获取和其他一些工作

jQuery与实现同等功能的纯javascript代码相比，写起来更简单，更易读，并且执行效率更高。

#使用开发者工具

不同浏览器的开发者工具：

+ [Internet Explorer Developer Tools][6]
+ [Safari Web Inspector][7]
+ [Chrome Developer Tools][8]
+ [Firebug for Firefox][9]
+ [Opera Dragonfly][10]

[6]:  http://msdn.microsoft.com/en-us/library/dd565628.aspx "http://msdn.microsoft.com/en-us/library/dd565628.aspx"
[7]: http://developer.apple.com/technologies/safari/developer-tools.html "http://developer.apple.com/technologies/safari/developer-tools.html"
[8]: https://developers.google.com/chrome-developer-tools/ "https://developers.google.com/chrome-developer-tools/"
[9]: http://getfirebug.com/ "http://getfirebug.com/"
[10]: http://www.opera.com/dragonfly/ "http://www.opera.com/dragonfly/"

这些工具都提供了相似的功能，如：

+ 浏览和修改DOM的各个方面
+ 探索CSS和其展示效果之间的关系
+ 方便地追踪特殊方法的脚本执行情况
+ 中断脚本的执行，查看变量的值

##Chrome Developer Tools

书中介绍了几个关于Chrome Dev Tools的非常常用的debug功能：

+ **Elements**标签页可以查看页面元素的详细信息，主要用于调试CSS样式
+ **Sources**标签页可以查看脚本文件，在代码行号上右键可以添加断电，查看中间变量
+ **Console**标签页可以查看脚本文件中是不是有语法报错，可以实验jQuery代码，选取元素等。在代码中插入`console.log()`会在这里打印日志

</br></br>

>你羡慕别人月薪几万，却不知道他日日加班到深夜的辛苦；你羡慕别人说走就走四处周游的自由，却不知道他为这份自由放弃的东西。我们追求的该是自己的幸福，而不是比别人幸福。奋斗的路上别总急着奔跑，偶尔停下来，听听生活的道理。

</br>
</br>
