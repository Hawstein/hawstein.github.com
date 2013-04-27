---
layout: post
category: Programming
title: Cracking the coding interview--Q11.1~Q11.6
---

## 题目11.1

原文：

Find the mistake(s) in the following code:	

	unsigned int i;
	for (i = 100; i <= 0; --i)
		printf(“%d\n”, i);

## 解答11.1

简单题。不过不能粗心，否则可能会以为把i <= 0改为i >= 0就OK了。
这就把一个错误改成了另一个错误。因为i的数据类型是unsigned int，
是恒大于等于0的，这一改就把本来一次都不执行的循环，变成了执行无限次的循环了。
做这种题目，最好看一句，就思考一下这一句可能会带来的限制或是这一句可能的考点XD.
如下：

	unsigned int i; //unsigned int类型，无符号整型，恒大于等于0
	for (i = 100; i <= 0; --i) //错误，100<=0为假，循环一次也不执行。
							  //改为i>=0,错误，因为i恒>=0，无限循环
							  //改为i>0，正确，输出100到1的数。
		printf(“%d\n”, i);

上题还可以这样改：

	int i;
	for (i = 100; i >= 0; --i)
		printf(“%d\n”, i);

输出100到0的数。

## 题目11.2


## 解答11.2


## 题目11.3


## 解答11.3


## 题目11.4


## 解答11.4


## 题目11.5


## 解答11.5


## 题目11.6


## 解答11.6


[Cracking the coding interview--问题与解答](/posts/ctci-solutions-contents.html)

全书的C++代码托管在Github上：

<https://github.com/Hawstein/cracking-the-coding-interview>
