---
layout: post
title: Polymer入门——注册元素及生命周期
---

## <span id="register">注册一个自定义元素</span>

要想注册一个自定义元素，使用`Polymer`函数并且传入新元素的原型（prototype），原型必须包含一个`is`属性，它用来指定你的自定义元素的HTML标签名。

另外规范上规定自定义元素的名字**必须包含一个短划线（-）**。

<!--excerpt_separator-->

举例：

{% highlight javascript %}
// register an elementh
MyElement = Polymer({

  is: 'my-element',

  // See below for lifecycle callbacks
  created: function() {
    this.textContent = 'My element!';
  }

});

// create an instance with createElement:
var el1 = document.createElement('my-element');

// ... or with the constructor:
var el2 = new MyElement();
{% endhighlight %}

`Polymer`函数向浏览器注册元素，并且返回一个构造器，这个构造器能够用来在代码里创建自定义元素的新对象。

`Polymer`函数会为你的自定义元素构建原型链（prototype chain），并把它链到Polymer `Base`原型里（这是Polymer的附加功能）。你无法构建自己的原型链，但是可以用行为（behaviors）来在元素之间共享代码。

### 定义一个自定义的构建器

`Polymer`函数会返回一个最基础的构建器，用来实例化自定义元素。如果想要向构建器传入参数来配置新的元素，可以在原型上指定一个自定义的`factoryImpl`函数。

`Polymer`返回的构建器通过`document.createElement`来创建一个实例，然后调用开发者提供的`factoryImpl`函数，其中`this`被绑定到实例对象，任何传入构建器的参数都会被传到`factoryImpl`函数。

举例：

{% highlight javascript %}
MyElement = Polymer({

  is: 'my-element',

  factoryImpl: function(foo, bar) {
    this.foo = foo;
    this.configureWithBar(bar);
  },

  configureWithBar: function(bar) {
    ...
  }

});

var el = new MyElement(42, 'octopus');
{% endhighlight %}

关于自定义构建器有两点说明：

+ `factoryImpl`函数*只*会在使用构建器创建对象的时候被调用，如果元素是从HTML解析器由标记创建的，或者用`document.createElement`创建的话，`factoryImpl`函数是不会被调用的。
+ `factoryImpl`函数会在元素被初始化（已经创建了DOM元素，设置了默认值等等）*之后*被调用。阅读[Ready回调函数及元素初始化](#ready)来了解更多信息。

### 扩展原生的HTML元素

Polymer目前只支持扩展原生的HTML元素（比如`input`，或者`button`，扩展其他自定义元素将在未来的版本中支持），这些原生元素的扩展叫做*类型扩展的自定义元素*。

> 注意：当使用原生的shadow DOM时，对原生元素的扩展将得到不可预知的行为，有时甚至是不被允许的。启用原生shadow DOM来测试你的元素以发现开发中的任何问题。关于启用原生shadow DOM，请见[全局Polymer设置](https://www.polymer-project.org/1.0/docs/devguide/settings)。

要想扩展一个原生的HTML元素，在原型中指定`extends`属性为想要扩展的元素的标签名即可。

举例：

{% highlight javascript %}
MyInput = Polymer({

  is: 'my-input',

  extends: 'input',

  created: function() {
    this.style.border = '1px solid red';
  }

});

var el1 = new MyInput();
console.log(el1 instanceof HTMLInputElement); // true

var el2 = document.createElement('input', 'my-input');
console.log(el2 instanceof HTMLInputElement); // true
{% endhighlight %}

要想在标记中使用类型扩展的元素，使用*原生*的标签并且添加`is`属性来指定扩展类型名：

{% highlight html %}
<input is="my-input">
{% endhighlight %}

### 在主HTML文件中定义元素

> 注意：在主HTML文件中定义元素应该只被用于实验。在发布的产品中，元素应该都是在另外的文件中定义并且被导入（import）到主文件中。

从`HTMLImports.whenReady(callback)`里可以在主HTML文件中定义一个元素，`callback`会在所有被导入的文件都已经结束加载的时候被调用。

{% highlight html %}
<!DOCTYPE html>
<html>
  <head>
    <script src="bower_components/webcomponentsjs/webcomponents-lite.js">
    </script>
    <link rel="import" href="bower_components/polymer/polymer.html">
    <title>Defining a Polymer Element from the Main Document</title>
  </head>
  <body>
    <dom-module id="main-document-element">
      <template>
	    <p>
	      Hi! I'm a Polymer element that was defined in the
	      main document!
	    </p>
      </template>
      <script>
	    HTMLImports.whenReady(function () {
	      Polymer({
		    is: 'main-document-element'
	      });
	    });
      </script>
    </dom-module>
    <main-document-element></main-document-element>
  </body>
</html>
{% endhighlight %}

## 声明周期回调函数

Polymer的Base原型实现了标准的自定义元素声明周期回调函数来完成Polymer内置功能需要做的一些事。Polymer会反过来在你的原型上调用更短名字的生命周期函数。

Polymer添加了一个额外的`ready`回调函数，这个函数会在Polymer已经创建并且初始化了元素的局部DOM时调用。

| 回调函数 | 函数介绍 |
| -------- | -------- |
| `created` | 在元素被创建后，属性被赋值、局部DOM被初始化之前被调用。用于属性赋值前的一次性配置。替代`createdCallback`使用。 |
| --------- | -------- |
| `ready` | 在属性被赋值及局部DOM初始化之后调用。用于局部DOM初始化之后的一次性配置（对于属性值的配置推荐使用`observer`）。 |
| `attached` | 在元素被attach到页面之后被调用，可以在元素的生命周期内被多次调用，第一次\`attached\`调用在\`ready\`调用之后。它的用处包括获取计算出来的样式信息，添加页面级的时间监听器。（如果使用声明的事件处理方法，比如注释的事件监听器或`listener`对象，Polymer会自动在attach时添加监听器并在detach时移除。代替`attachedCallback`使用。 |
| `detached` | 当元素已经被detach的时候调用，可以在元素的生命周期中被多次调用。用于移除添加到`attached`里的监听器。 |
| `attributeChanged` | 当元素的属性被改变时得到调用。用于应对*不*反馈于声明属性的特性的改变（对于声明的属性，Polymer可以自动handle特性的变化，详见attribute deserialization。） |
| ------ | ------ |

举例：

{% highlight javascript %}
MyElement = Polymer({

  is: 'my-element',

  created: function() {
    console.log(this.localName + '#' + this.id + ' was created');
  },

  ready: function() {
    console.log(this.localName + '#' + this.id + ' has local DOM initialized');
  },

  attached: function() {
    console.log(this.localName + '#' + this.id + ' was attached');
  },

  detached: function() {
    console.log(this.localName + '#' + this.id + ' was detached');
  },

  attributeChanged: function(name, type) {
    console.log(this.localName + '#' + this.id + ' attribute ' + name +
      ' was changed to ' + this.getAttribute(name));
  }

});

{% endhighlight %}

### <span id="ready">Ready回调函数及局部元素初始化</span>

`ready`回调函数当Polymer元素的局部DOM被初始化后被调用。

> 什么是局部DOM？局部DOM是由你的元素创建和管理的一个元素的子树，这和元素的children，也就是*轻量级*DOM是区分开来的。详见[局部DOM]()。

元素在以下情况时成为已经*ready*了：

+ 其属性值已经被配置，值和父元素绑定，从特性值中反序列化，或者设置为了默认值。
+ 局部的DOM模板已经被实例化。
+ 所有**元素局部DOM内部**的元素都ready了并且它们的`ready`函数已经被调。

当有必要在元素创建时操控其局部DOM时实现`ready`函数。

{% highlight javascript %}
ready: function() {
  // access a local DOM element by ID using this.$
  this.$.header.textContent = 'Hello!';
}
{% endhighlight %}

> 注意：这个例子使用[自动节点查找]()来访问局部DOM元素。

当给定一棵树时，`ready`函数是按照*页面的顺序*来被调用的，但是兄弟元素间初始化的顺序是不可靠的，或者host元素和它的**轻量级DOM**子元素之间。

### 初始化顺序

元素的基本初始化顺序是：

+ `created`回调函数
+ 局部DOM被初始化（这说明**局部DOM**子元素被创建了，它们的属性值也像模板中指定的一样赋值了，`ready`函数也在它们身上被调用了）
+ `ready`回调函数
+ [factoryImpl callback]()
+ `attached`回调函数

注意，对于一个给定的元素，它的以上生命周期回调函数是按照介绍的来执行的，但是**元素间的初始化顺序**会由于很多其他元素比如浏览器是否支持web components而发生变化。

#### 轻量级DOM子元素的初始化时间

对于轻量级DOM子元素的初始化时间是没有保证的，通常元素按照页面的顺序被初始化，因此子元素通常在父元素之后被初始化。

比如如下`avatar-list`的轻量级DOM：

{% highlight html %}
<avatar-list>
  <my-photo class="photo" src="one.jpg">First photo</my-photo>
  <my-photo class="photo" src="two.jpg">Second photo</my-photo>
</avatar-list>
{% endhighlight %}

`<avatar-list>`的`ready`函数很有可能在它的子元素`<my-photo>`之前被调用。

另外，用户可以在父元素被创建之后的任何时间添加轻量级子元素，一个设计良好的元素应该是能在runtime操控它的轻量级DOM。

为了避免时间问题，可以使用一下策略：

+ 懒惰地handle轻量级DOM子元素。比如，弹出菜单元素可能需要计算其轻量级DOM子元素，通过在菜单打开时计算`children`可以处理用户对菜单元素的添加和移除。
+ 应对子元素的添加和删除可以用[ovserveNodes方法]()

#### 局部DOM子元素的初始化时间

局部DOM的初始化时间为：元素被创建，属性如模版中指定的被赋值，`ready`函数在其父元素的`ready`回调函数被调用之前被调用。

两点注意事项：

+ 当属性值更新时，`dom-repeated`和`dom-if`模版**异步地**创建DOM。例如，如果在你的元素的局部DOM里有一个`dom-repeat`，那么`ready`回调会在其`dom-repeat`结束创建实例之前被调用。如果你需要知道`dom-repeat`和`dom-if`在何时创建和移除模版实例，可以创建`dom-change`事件的监听器。详见[dom-change事件]()。

+ Polymer保证局部DOM子元素的`ready`回调在其父元素之前被调用，然后并不保证`attached`回调，这是原生行为和polyfill行为的一个基本差别。

#### 兄弟元素的初始化事件

对于兄弟元素的初始化事件没有相关保证。这表示兄弟元素可能以任何顺序成为`ready`。

想要在元素初始化时获取兄弟元素，可以在`attached`回调内部调用`async`：

{% highlight javascript %}
attached: function() {
  this.async(function() {
    // access sibling or parent elements here
  });
}
{% endhighlight %}

### 注册回调函数

Polymer同样提供了两个注册时回调函数`beforeRegister`和`registered`。

`beforeRegistered`用于在注册之前转变元素的原型，这在使用ES6类注册元素时非常有用。

可以通过实现`registered`回调在元素被注册时进行一次性初始化，这个对于实现行为非常有用。

## host静态属性

如果自定义元素需要在被创建时设置一些HTML属性的话，这些属性可以在原型中的`hostAttributes`中声明，其中键值为属性名，值为想要赋的值。值应为字符串，因为HTML属性只能是字符串，然而标准的`serialize`方法被用来进行值到字符串的转换，因此`true`将会被序列化为空属性，`false`将会得到没有赋的值，详见[属性序列化]()。

举例：

{% highlight javascript %}
<script>

  Polymer({

    is: 'x-custom',

    hostAttributes: {
      'string-attribute': 'Value',
      'boolean-attribute': true,
      tabindex: 0
    }

  });

</script>
{% endhighlight %}

将得到：

{% highlight html %}
<x-custom string-attribute="Value" boolean-attribute tabindex="0"></x-custom>
{% endhighlight %}

> 注意：`class`属性不能 通过`histAttributes`配置。

## 行为

元素可以通过*行为*来共享代码，行为能够定义属性，声明周期回调函数，事件监听器及其他功能特性。

详见[行为]()。

## 类式构造函数

如果想要配置自定义的元素原型链但**不**立即注册元素，可以使用`Polymer.Class`函数，这个函数的参数和`Polymer`一样，会配置原型链，但*不*注册元素，相反它会返回一个注册函数，这个注册函数可以传入`document.registerElement`来向浏览器注册你的元素，之后可以用它来通过代码生成元素的实例。

如果你想要一步完成定义和注册自定义元素，使用[Polymer函数](#register)。

举例：

{% highlight javascript %}
var MyElement = Polymer.Class({

  is: 'my-element',

  // See below for lifecycle callbacks
  created: function() {
    this.textContent = 'My element!';
  }

});

document.registerElement('my-element', MyElement);

// Equivalent:
var el1 = new MyElement();
var el2 = document.createElement('my-element');
{% endhighlight %}

`Polymer.Class`是希望能够在可预测的未来，当ES6类能够被定义和提供给`document.registerElement`时，提供类似的设计。
