---
layout: post
time: 2014-2-13
title: 《程序员面试宝典》第8章笔记勘误
category: 《程序员面试宝典》笔记
keywords: 程序员面试宝典,循环,递归
tags: 笔试面试,C/C++
description: 《程序员面试宝典》第8章的学习笔记和书中部分勘误。
---

#第八章 循环、递归与概率#

##8.2 典型递归问题##

**面试例题1**

书中给出的解法比较难懂，从网上找到一种好理解的方法。

{% highlight C++ %}
#include <iostream>
#include <vector>
using namespace std;

void print(vector<int> v){ //输出次序串
	for(vector<int>::iterator it = v.begin(); it != v.end(); it++)
		cout << *it << " ";
	cout << endl;
}

void match(char str1[], char str2[], int index1, int index2, vector<int> &v){
	if(str1 == NULL || str2 == NULL)
		return ;

	if(str1[index1] == '\0' || str2[index2] == '\0'){ //其中一个字符串扫描完毕
		if(str2[index2] == '\0'){ //str2扫描完毕，输出v中的次序串
			print(v);
		}
		return ;
	}

	if(str1[index1] == str2[index2]){
		v.push_back(index1+1);
		match(str1, str2, index1+1, index2+1, v); //开始找str2里下一个字符在str1中的位置
		v.pop_back();
	}

	match(str1, str2, index1+1, index2, v); //str1继续往后扫描，寻找当前str2中的字符str2[index2]
}

int main(){
	char* str1 = "abdb";
	char* str2 = "abc";
	vector<int> v;
	match(str1, str2, 0, 0, v);

	return 0;
}
{% endhighlight %}

##8.3 循环与数组问题##

**面试例题2**

这道题是实现一个zigzag数组，理解书上a[i][j]数值的算法实在得费一番功夫。这里给出一种简明好理解的解法。

{% highlight C++ %}
/*
zigzag数组是一个“之”字形排列的数组，如8*8的zigzag数组： 
    1     5     6    14    15    27    28
    4     7    13    16    26    29    42
    8    12    17    25    30    41    43
   11    18    24    31    40    44    53
   19    23    32    39    45    52    54
   22    33    38    46    51    55    60
   34    37    47    50    56    59    61
   36    48    49    57    58    62    63
*/

#include <iostream>
#include <iomanip>

using namespace std;

int main()
{
    int N;
    int s,i,j,dir;
    int squa;
    cout<<"将要实现N*N的zigzag矩阵,请输入N([1 100]):";
    cin>>N;

    int a[100][100];

	for(int i = 0; i < 100; i ++)
		for(int j = 0; j < 100; j++)
			a[i][j] = 0;
    
    squa=N*N;
    i=0;     //行 
    j=0;     //列 
    s=0;     //计数 
    dir=0;  //四个行进方向0（right）,1（left_down）,2（down）,3（right_up）
    while(s<squa)
    {
       //数组赋值
       a[i][j]=s;
       
       //设置下一点（位置和方向） 
       switch(dir)
       {
          case 0:
                //位置 
                j++;
                //行进方向 
                if(0==i)
                    dir=1;
                if(N-1==i)
                    dir=3;
                break;
          case 1:
                i++;
                j--;   
                if(N-1==i)
                       dir=0;
                else if(0==j)
                       dir=2;
                break;
          case 2:
                i++;
                if(0==j)
                    dir=3;
                if(N-1==j)
                    dir=1;
                break;
          case 3:
                i--;
                j++;
                if(N-1==j)
                       dir=2;
                else if(0==i)
                       dir=0;
                break;
          default:
                break; 
       }
       s++;                        
    }
    
    cout<<"***********************************************"<<endl;
    cout<<N<<"*"<<N<<"的zigzag矩阵:"<<endl;
    for(i=0;i<N;i++)
    {
         for(j=0;j<N;j++)
                cout<<setw(5)<<a[i][j];
         cout<<endl;
    }
    
	return 0;
}
{% endhighlight %}

用这种方法也可以简单地实现螺旋数组，即8.4节的扩展部分。

{% highlight C++ %}
/*
螺旋数组从首坐标开始顺时针依次增大，如5*5的螺旋数组： 
    1     2     3     4     5
   16    17    18    19     6
   15    24    25    20     7
   14    23    22    21     8
   13    12    11    10     9

*/

#include <iostream>
#include <iomanip>

using namespace std;

int main()
{
    int N;
    int s,i,j,dir;
    int squa;
    cout<<"将要实现N*N的螺旋矩阵,请输入N([1 100]):";
    cin>>N;

    int a[100][100];

	for(int i = 0; i < 100; i ++)
		for(int j = 0; j < 100; j++)
			a[i][j] = 0;
    
    squa=N*N;
    i=0;     //行 
    j=0;     //列 
    s=1;     //计数 
    dir=0;  //四个行进方向0（right）,1（down）,2（left）,3（up）
    while(s<=squa)
    {
       //数组赋值
       a[i][j]=s;
       
       //设置下一点（位置和方向） 
       switch(dir)
       {
          case 0:
                //位置 
                j++;
                //行进方向 
                if(N-1==j || a[i][j+1] != 0)
                    dir=1;
                break;
          case 1:
                i++;
                if(N-1==i || a[i+1][j] != 0)
                       dir=2;
                break;
          case 2:
                j--;
                if(0==j || a[i][j-1] != 0)
                    dir=3;
                break;
          case 3:
                i--;
                if(a[i-1][j] != 0)
                       dir=0;
                break;
          default:
                break; 
       }
       s++;                        
    }
    
    cout<<"*************************************************************"<<endl;
    cout<<N<<"*"<<N<<"的螺旋矩阵:"<<endl;
    for(i=0;i<N;i++)
    {
         for(j=0;j<N;j++)
                cout<<setw(5)<<a[i][j];
         cout<<endl;
    }
    
	return 0;
}
{% endhighlight %}


<br></br>

![imge](/assets/image/daodao/3.jpg)
