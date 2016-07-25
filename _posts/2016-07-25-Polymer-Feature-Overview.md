---
layout: post
title: Polymer功能概述——Polymer库
---

Polymer库提供了一系列用于创建自定义元素的功能，这些功能能够使用户更加简单快速地创建和标准DOM元素一样工作的自定义元素。类似于标准的DOM元素，Polymer元素可以：

+ 使用constructor或者`document.createElement`来实例化
+ 使用attributes或者properties来配置属性
+ 在元素内部添加DOM元素
+ 响应属性或特性的变化（比如向DOM元素填充数据，或者触发事件）
+ 用内部默认的样式或者外部设置风格
+ 响应于改变内部状态的方法

<!--excerpt_separator-->

基本的Polymer元素定义如下：

{% highlight html %}
<dom-module id="element-name">

      <template>
        <style>
          /* CSS rules for your element */
        </style>

        <!-- local DOM for your element -->

        <div>{{greeting}}</div> <!-- data bindings in local DOM -->
      </template>

      <script>
        // element registration
        Polymer({
          is: "element-name",

          // add properties and methods on the element's prototype

          properties: {
            // declare properties for the element's public API
            greeting: {
              type: String,
              value: "Hello!"
            }
          }
        });
      </script>

    </dom-module>
{% endhighlight %}

后续的指南将把Polymer的功能划分成以下几个部分来介绍：

+ 注册元素及生命周期。注册元素需要将一个自定义的元素名字与class或prototype相关联，另外元素提供回调函数来管理生命周期，使用行为来共享代码。
+ 声明的属性。声明的属性可以通过标记来配置，可以选择性地支持变动观察者（change observer）、双向数据绑定及特性反射。也可以声明计算属性和只读属性。
+ 局部DOM。局部DOM是由元素创建和管理的DOM。
+ 事件。将事件监听器附在host对象和局部DOM子对象。事件重定向。
+ 数据绑定。属性绑定。特性绑定。
+ 行为。行为是可以被用于Polymer元素的可重用的代码模块。
+ 通用功能。辅助公共任务的方法。
+ 实验性功能和元素。实验性的模板和样式功能。功能分层。

原文英文链接：<https://www.polymer-project.org/1.0/docs/devguide/feature-overview>
