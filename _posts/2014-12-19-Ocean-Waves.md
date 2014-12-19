---
layout: post
time: 2014-12-19
title: 海浪
category: 研究
keywords: research,ocean waves
tags: 研究
description: 海浪相关文献翻译及资料整理
---

#海浪

英文原文地址：<http://hyperphysics.phy-astr.gsu.edu/hbase/waves/watwav2.html>

海洋上理想化的行进波的速度依赖于波长，在比较浅的海域，这个速度还依赖于水的深度。波的速度关系表达式为

![img](/assets/image/posts/ocean-waves/1.gif)

![img](/assets/image/posts/ocean-waves/2.jpg)

海浪的这种简化处理不足以描述这个问题的复杂性。这个课题当然已经被彻底的研究过了，但没有一个单一的模型能够适用于海浪的所有情况。一些已经得到的结论将在这里总结一下。

假定海浪遵循基本的波关系式c=fλ，其中c表示波速或者celerity，celerity表示行进波相对于驻波的速度，因此任何当前的或者净水流速度也包括在内。

海浪的形状通常用正弦波来描述，但实验波形一般用次摆线（trochoid）来描述。一个次摆线被定义为：当一个圆圈沿直线往前滚时，圆圈上一点走过的痕迹曲线。

![img](/assets/image/posts/ocean-waves/3.gif)

关于次摆线的补充，请见后面。

当变幅较小时，次摆线在形状上会趋近于正弦曲线，但是你可能会发现它们的形状是不同的，与正弦曲线相比次摆线的波峰更窄一些，当波幅增加时，波峰会变得更陡。

次摆线形状的发现来自于一个观察发现：当浪潮经过时，水中的例子会进行圆周运动，但它们的位置并没有明显的净前进。当波峰经过时，水的运动是朝向前的，当波谷经过时，水的运动朝向后，因此当下一个波峰到来时又回到了初始的位置（实际上，实验显示海浪中的水有略微的前进，但距离相对于整体的圆周运动来说很小）。

![img](/assets/image/posts/ocean-waves/8.gif)

上面转自von Arx的插图显示了沿海浪上不同点水的运动方向。

![img](/assets/image/posts/ocean-waves/9.gif)

Bascom用造波水池研究了水的圆周运动，图中圆圈代表了当一个完整波长通过时介质的完整运动。通常的做法是往水中注入小滴的油，用氧化锌使其变白，并调整密度使其和水的密度一致，这样在水池的两边就能描绘出运动轨迹。实验发现，轨道会沿波浪行进方向有一个略微的行进距离。

事实上，水的圆圈运动在一定深度会扁平化为前后的振荡这一现象能被潜水员在浅礁处很容易的观察到。在上方波浪运动的影响被转化为悬浮在暗礁上方潜水员的前后运动，并且能通过柔软的珊瑚虫和水底植物观察到。

上面所有关于波浪的描述适用于长波长的海浪，von Arx将其称之为“重力波”，意味着它们主要受重力和惯性的控制。它们的波速随波长的增加而增长，这一现象被称作“正常波散”。波长小于1.73cm的波，水的表面张力会施加作用，这种波von Arx称为“毛细波”，这种波的波速随波长的缩短而增加，这一现场被称为“异常波散”。在1.73cm波长，最小的波速为23.1cm/s。下方转自von Arx的图展示了深水速度和波长的关系。

![img](/assets/image/posts/ocean-waves/10.gif)

小的纹波或毛细波被首次观察到是在当风吹过平静的水面时，它们有圆拱形的波峰和v形的槽。Von Arx将这些波形容为就好像风把水抓紧了起来，因为它们数量很多，并且比风跑得慢。

对于一个给定的波长，当波幅增加时，标志海浪形状的次摆线的形状会发生变化。

![img](/assets/image/posts/ocean-waves/11.gif)

当波幅增加时，波峰会变的更窄更陡。Bascom通过造波水池实验证实波高和波长的最大比是1:7，波峰间的最小角度是120°。在这个比率之上波会变得不稳定。

次摆线波浪模型

![img](/assets/image/posts/ocean-waves/12.jpg)

当波朝向海岸行进时，较浅的水会降低波速，因此波长变短，波峰变高。波峰会变得不稳定，并且比底下的水运动的快，因此会向前破碎。


##次摆线

次摆线又称“长(短)幅旋轮线”。
一个动圆沿着一条定直线作无滑动的滚动时，动圆外或动圆内一定点的轨迹。如图建立直角坐标系，设动圆的半径为a，圆心至圆外(内)定点m的距离为b，则次摆线的参数方程为
![img](/assets/image/posts/ocean-waves/5.jpg)
![img](/assets/image/posts/ocean-waves/6.jpg)
b＞a时为长幅旋轮线，b＜a时为短幅旋轮线，b＝a时即为摆线。
{% highlight matlab %}
clear all;clc;
phi=0:pi/20:8*pi;
 
subplot(3,1,1);
b=2;a=4;
x=a*phi-b*sin(phi);
y=a-b*cos(phi);
plot(x,y);axis equal;
 
subplot(3,1,2);
b=4;a=4;
x=a*phi-b*sin(phi);
y=a-b*cos(phi);
plot(x,y);axis equal;
 
subplot(3,1,3);
b=6;a=4;
x=a*phi-b*sin(phi);
y=a-b*cos(phi);
plot(x,y);axis equal;
{% endhighlight %}

matlab画图：
![img](/assets/image/posts/ocean-waves/7.jpg)
