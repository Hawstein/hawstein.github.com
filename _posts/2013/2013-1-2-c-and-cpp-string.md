---
layout: post
title: "C/C++字符串处理"
author: "Hawstein"
header-style: text
tags:
  - Algorithm
  - Data Structure
  - C++
---

## 前言

字符串处理是编程中常遇到的问题，这时候如果能熟练地运用C/C++已有的字符串处理函数，
将大大地提高编程效率。

## C++ Strings

需要包含头文件`<string>`，即`#include <string>`，以下是一些常用函数。

* append：在字符串后面追加

```cpp
string s1 = "hello ", s2 = "world ";
char *s3 = "boy ";
s1.append(s2);	//s1: "hello world "
s1.append(s3);	//s1: "hello world boy "
s1.append(s2, 0, 3); //取s2从0开始，长度为3的子串追加到s1: "hello world boy wor"
s2.append(s3, 2); //在s2后追加2个s3(char*类型),s2："world boy boy "
s2.append(3, '!'); //在s2后追加3个感叹号(char类型)，s2："world boy boy !!!"
s1.append(s2.begin(), s2.end()); //在s1后追加s2，使用迭代器
```

* c_str：string转char*。char*转string可以用string的构造函数
```cpp
const char *s2 = s1.c_str(); //返回值是const char*，不可修改。
string s3(s2); // 或者string s3 = s2;
```

关于data()函数和c_str()函数，网上的说法是：

	c_str()返回的指针保证指向一个size() + 1长的空间，而且最后一个字符肯定"\0"；
	而data返回的指针则保证指向一个size()长度的空间，有没有null-terminate不保证，可能有，可能没有，看库的实现了。

so，一般情况下没有使用data的必要。

* find：字符或字符串查找，相当有用的函数。

```cpp
string s1 = "hello world hello!", s2 = "hello";
char *s3 = "boy";
cout<<s1.find(s2);	//从s1的第0个位置开始查找s2，返回第一个匹配串的下标：0
cout<<s1.find(s2, 1);	//从s1的第1个位置开始查找，返回12
cout<<s1.find(s3, 0); //s1中查找s3(char*),失败，返回string::npos(-1)
cout<<s1.find(s3, 0, 10); //s1中查找s3，从位置0开始，查找长度为10
cout<<s1.find('e', l); //从s1的第0个位置开始查找字符'l',返回第一个匹配下标：2
```

* 关于其他字符串查找函数，以下的函数都有与find函数相同的参数列表，
即每个函数有四个重载函数，下面只列出一种。

```cpp
string s1 = "hello world!", s2 = "held";
s1.find_first_of(s2); //在s1中查找第一处匹配s2中任一字符的位置：0(匹配h)
s1.find_first_not_of(s2); //在s1中查找第一个没在s2中出现的字符的位置：4
s1.find_last_of(s2); //同上，返回：10(匹配d)
s1.find_last_not_of(s2); //同上，返回11(匹配!)
```

* getline: 从输入流中读取一行到字符串，具有换行功能。

```cpp
string s;
getline(cin, s); //从标准输入流中读入一行，保存到字符串s
```

* insert：字符串插入
* length: 返回字符串长度

* substr: 返回子串，也是相当有用的一个函数。

```cpp
string s1 = "hello world!";
s2 = s1.substr(6, 5); //从s1中第6个位置开始，取出长度为5的子串：world
s2 = s1.substr(6); //从s1的第6个位置开始，一直取到末尾：world!
```

## Standard C String

* atof：char* 转double
* atoi：char* 转int
* atol：char* 转long

以上函数使用需要包含头文件`<cstdlib>`，即`#include <cstdlib>`。
如果要将string转为double,int或long，只需要先用c_str()将它转为char*,
然后使用上面的函数做转换即可。

```cpp
double d = atof("42.3");
int i = atoi("423");
long l = atol("2313455");
string s = "23.45";
d = atof(s.c_str());
```

* isalnum：如果字符是一个字母或者一个数字，返回非0值；否则返回0
* isalpha：如果字符是一个字母，返回非0值；否则返回0
* isdigit：如果字符是0－9的一个数，返回非0值；否则返回0

以上函数的使用需要包含头文件`<cctype>`，即`#include <cctype>`。

```cpp
cout<<isalnum('A');	//返回非0
cout<<isalpha('1'); //返回0
cout<<isdigit('1'); //返回非0
```
