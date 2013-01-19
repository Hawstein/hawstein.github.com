---
layout: post
category: Programming
title: 简明STL教程
---

作者：Hawstein

出处：<http://hawstein.com/posts/a-byte-of-stl.html>

声明：本文采用以下协议进行授权：
[自由转载-非商用-非衍生-保持署名|Creative Commons BY-NC-ND 3.0](http://creativecommons.org/licenses/by-nc-nd/3.0/deed.zh)
，转载请注明作者及出处。

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

1. vector构造函数
	* vector<int> v1;// int可换成其它数据类型，包括自定义的结构体和类
	* vector<int> v2(v1);// 用v1初始化v2
	* vector<int> v1(7, 10); // v1中有7个元素，都是10
	* vector<int> v2(v1.begin(), v1.end()); //使用迭代器初始化
1. assign：给一个vector对象赋值
1. at：返回vector中指定位置的元素
1. back：返回vector中最后一个元素的引用
1. begin：返回指向vector第一个元素的迭代器
1. capacity：返回vector能存储的元素数量(注意与size的差别)
1. clear：移除vector中所有的元素
1. empty：如果vector中没有元素，返回true
1. end：返回指向vector最后元素的后一个位置的迭代器
1. erase：从vector中移除元素
1. front：返回vector中第一个元素的引用
1. insert：往vector中插入元素
1. max_size：返回vector中能存储元素的最大数目
1. pop_back：移除vector中的最后一个元素
1. push_back：向vector的尾部追加一个元素
1. resize：改变vector的大小
1. size：返回vector中的元素数量
1. swap：交换两个vector的内容
