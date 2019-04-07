---
layout: post
title: "聚合数(aggregated number)"
author: "Hawstein"
header-style: text
tags:
  - Algorithm
---

## 题目

原文：

We will name a number "aggregated number" if this number has the following
attribute:

just like the Fibonacci numbers

1,1,2,3,5,8,13.....

the digits in the number can divided into several parts, and the later part is the sum of the former parts.

like 112358, because 1+1=2, 1+2=3, 2+3=5, 3+5=8

122436, because 12+24=36

1299111210, because 12+99=111, 99+111=210

112112224, because 112+112=224

so can you provide a function to check whether this number is aggregated number
or not?

译文：

如果一个数具有以下属性，我们就称之为聚合数(aggregated number):

就如斐波那契数列一样：

1,1,2,3,5,8,13.....

这个数可以被分为几个部分，后面部分的数字是前面部分数字之和(本文假设每个数字是前
两个数字之和)

比如以下几个数都是聚合数(aggregated number):

112358, 因为 1+1=2, 1+2=3, 2+3=5, 3+5=8

122436, 因为 12+24=36

1299111210, 因为 12+99=111, 99+111=210

112112224, 因为 112+112=224

那么你可以写一个函数来判断一个数是否是聚合数(aggregated number)吗？

## 解答

看完题目后，想到以下几点：

1. 数字有多大？如果给你一个几十位的大数字，即使long long也放不下，你要怎么处理？
1. 分成的几个部分一定是非降的吗？题目可没这么说，所以12113也应该是聚合数。
1. 由于题目要将一个数分成几个部分操作，用数组或字符串来维护这个数会更方便。

直接从这个数本身下手不太好处理，那么我们可以换另一种思路。我先从这个数里取出第一
和第二部分，有了这两个部分，我们正向地就可以构造出一个聚合数。
给定两个初始值，可以构造无数个聚合数。所以，我们用原来数字的长度做限制，
构造一个长度不大于它的聚合数，然后再比较它们是不是一样。遍历一遍可能的初始值，
即可求解。

代码如下：

```cpp
#include <iostream>
#include <sstream>
#include <string>
#include <cstdlib>
using namespace std;

bool match(int i, int j, string text)
{
    string first = text.substr(0, i);
    string second = text.substr(i, j);
    string str = "";
    str += first + second;
    while(str.length() < text.length())
    {
        long long th = atoll(first.c_str())+atoll(second.c_str());
        stringstream ss;
        string third = "";
        ss << th;
        ss >> third;
        str += third;
        first = second;
        second = third;
    }
    return str.length()==text.length() && str == text;
}

bool isAggregatedNum(string text)
{
    int len = text.length()/2;
    for(int i=1; i<=len; ++i)
        for(int j=1; j<=len; ++j)
            if(match(i, j, text))
                return true;
    return false;
}

int main()
{
    string s = "1111111111111111111111111112222222222222222222222222223333333333333333333333";
    cout << isAggregatedNum(s) << endl;
    return 0;
}
```

以上代码基于这样的假设：这个数分解成部分后，每个部分都不会超过long long
的表示范围。如果超出了，程序将无法给出正解答案。那么，如果我要判断的数非常大，
分解出的部分数字就已经超出long long的表示范围(或是unsigned long long)，
应该如何解决呢？那就干脆全用字符串来表示！至于加法，再写一个函数来实现即可。

代码如下：

```cpp
#include <iostream>
#include <string>
using namespace std;

string add(string a, string b)
{
    int la = a.length();
    int lb = b.length();
    int num = la > lb ? la : lb;
    char c[num+2];
    int k = 0, cc = 0;
    while(la>0 && lb>0)
    {
        int t = a[(la--)-1] + b[(lb--)-1] + cc - 96;
        c[k++] = t%10 + 48;
        cc = t/10;
    }
    while(la>0)
    {
        int t = a[(la--)-1]+cc-48;
        c[k++] = t%10 + 48;
        cc = t/10;
    }
    while(lb>0)
    {
        int t = b[(lb--)-1]+cc-48;
        c[k++] = t%10 + 48;
        cc = t/10;
    }
    if(cc != 0) c[k++] = cc + 48;
    string s = "";
    for(int i=0; i < k; ++i)
    {
        s = c[i] + s;
    }
    return s;
}
bool match(int i, int j, string text)
{
    string first = text.substr(0, i);
    string second = text.substr(i, j);
    string str = "";
    str += first + second;
    while(str.length() < text.length())
    {
        string third = add(first, second);
        str += third;
        first = second;
        second = third;
    }
    return str.length()==text.length() && str == text;
}

bool isAggregatedNum(string text)
{
    int len = text.length()/2;
    for(int i=1; i<=len; ++i)
        for(int j=1; j<=len; ++j)
            if(match(i, j, text))
                return true;
    return false;
}

int main()
{
    string s = "111111111111111111111111111111111112222222222222222222222222222222222233333333333333333333333333333333333";
    cout << isAggregatedNum(s) << endl;
    return 0;
}
```

第二个程序中要判断的数字非常大，使用第一个程序得到的是错误的判断，
认为它不是聚合数。而第二个程序无论数字多大，都可以给出正确的判断。

以上两个程序运行的时间主要花费在isAggregatedNum函数中的两个for循环。
两个for循环的时间复杂度为O(n^2 )，match函数用时O(n)，
所以该程序的时间复杂度为O(n^3 )。

## 题目来源

<http://www.careercup.com/question?id=14948278>
