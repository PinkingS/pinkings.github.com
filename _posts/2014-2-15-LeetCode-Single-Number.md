---
layout: post
time: 2014-2-15
title: LeetCode --- Single Number
category: LeetCode解题报告
keywords: LeetCode,笔试面试,算法
tags: LeetCode,异或,
description: 巧用异或找出“落单”的那个数
---

###Single Number###

Given an array of integers, every element appears twice except for one. Find that single one.

**Note:**

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

###思路：###

数组中元素都出现两次，只有一个数只出现一次，题目要求用线性时间复杂度算法找出这个数。

利用异或运算， `a^a=0`, `a^0=a`，两个相同的数异或之后为0，“落单”的数与0异或还是它自己，所以解法就是将数组中所有的数异或起来，得出的结果就是那个只出现一次的数。

###考查知识点：位运算###

###难度 : ★ ☆ ☆ ☆ ☆  ###
</br>
###解题代码：###

{% highlight C++ %}
class Solution {
public:
    int singleNumber(int A[], int n) {
        int ret = 0;
		for(int i = 0; i < n; i++){
        	ret ^= A[i];
        }
        return ret;
    }
};
{% endhighlight %}

</br>

![img](/assets/image/daodao/4.jpg) 
