---
layout: post
category: Programming
title: 简明STL教程
---

## 前言

本文通过实例介绍STL的用法，没有长篇大论，没有深入源码，纯粹的“知其然”文章，
目的是看完这文章后即可使用STL中的容器及算法。STL提供了许多常用的数据结构及算法，
而且我相信其中许多的数据结构及算法都是CSer们学过的(比如栈，队列等)，
使用STL可以避免每次都重新去实现它们(而且你的实现有可能很糟)。
前面已经说了，这只是一篇“知其然”的文章，如果想“知其所以然”，
推荐阅读侯捷的《STL源码剖析》或是直接阅读STL源码。

## 目录

1. [vector](#vector)
1. [list](#list)
1. [stack](#stack)
1. [queue](#queue)
1. [deque](#deque)
1. [map](#map)
1. [set](#set)

## <a id="vector">vector</a>

vector：一个有着N个(或更多)连续存储的元素的数组。

常用成员函数：

* vector构造函数
```cpp
vector<int> v1;	//int可换成其它数据类型，包括自定义的结构体和类
vector<int> v2(v1);	//用v1初始化v2
vector<int> v1(7, 10);	//v1中有7个元素，都是10
vector<int> v1(7);	//v1中有7个元素，都是默认值0
vector<int> v2(v1.begin(), v1.end());	//使用迭代器初始化
```

* assign：给一个vector对象赋值
```cpp
v1.assign(7, 10);	//v1中有7个元素，都是10
v2.assign(v1.begin(), v1.end());	//将v1中的值赋给v2
```

* at：返回vector中指定位置的元素
```cpp
v1.at(7);	//与v1[7]等价，但使用at更安全，因为当下标越界时会抛出异常
```

* back：返回vector中最后一个元素的引用
```cpp
v1.back();	//返回v1中最后一个元素
```

* begin：返回指向vector第一个元素的迭代器
```cpp
vector<int>::iterator it = v1.begin();	//迭代器it指向v1的第一个元素
cout<<*it<<endl;	//打印第一个元素
```

* capacity：返回vector能存储的元素数量(注意与size的差别)
```cpp
vector<int> v1(10);
cout<<v1.capacity()<<endl;	//输出v1的容量10
```

* clear：移除vector中所有的元素
```cpp
vector<int> v1(7, 1);	//v1中有7个1
v1.clear();	//清除v1中元素，此时v1的capacity为7，size为0
```

* empty：如果vector中没有元素，返回true
```cpp
if(!v1.empty()) cout<<v1.back();	//如果v1不为空，打印v1的最后一个元素
```

* end：返回指向vector最后元素的后一个位置的迭代器
```cpp
vector<int>::iterator it;
for(it=v1.begin(); it!=v1.end(); ++it)	//将v1中的元素都打印出来
	cout<<*it<<endl;
```

* erase：从vector中移除元素
```cpp
vector<int>::iterator bit = v1.begin();
v1.erase(bit);	//移除v1中的第一个元素
v1.erase(v1.begin(), v1.end());	//移除v1中的所有元素
```

* front：返回vector中第一个元素的引用
```cpp
v1.front();	//返回v1中的第一个元素
```

* insert：往vector中插入元素
```cpp
vector<int> v1(5, 1);	//v1: 1,1,1,1,1
vector<int> v2(3, 7);	//v2: 7,7,7
v1.insert(v1.begin(), 100);//往v1的第一个位置插入100，100,1,1,1,1,1
v1.insert(v1.begin(), 4, 100);//插入4个100, 100,100,100,100,1,1,1,1,1
v1.insert(v1.end(), v2.begin(), v2.end());//1,1,1,1,1,7,7,7
```

* max_size：返回vector中能存储元素的最大数目(注意与capacity和size的区别)
```cpp
vector<int> v1(700);
v1.max_size();	//我的计算机返回的是：1073741823
```

* pop_back：移除vector中的最后一个元素
```cpp
v1.pop_back();	//移除v1的最后一个元素
```

* push_back：向vector的尾部追加一个元素
```cpp
vector<int> v1(5, 1);	//v1: 1,1,1,1,1
v1.push_back(7);	//v1: 1,1,1,1,1,7
```

* size：返回vector中的元素数量
```cpp
vector<int> v1(5, 1);	//capacity: 5, size: 5
v1.clear();
cout<<v1.capacity()<<" "<<v1.size(); //capacity: 5, size: 0
```

* swap：交换两个vector的内容
```cpp
vector<int> v1, v2;
v1.push_back(111);	//v1: 111
v2.push_back(222);	//v2: 222
v1.swap(v2);	//v1: 222, v2: 111
```

