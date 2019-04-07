---
layout: post
title: "Cracking the coding interview--问题与解答"
author: "Hawstein"
header-style: text
tags:
  - Algorithm
  - CC150
---

## 前言

《Cracking the coding interview》是一本被许多人极力推荐的程序员面试书籍，
详情可见：<http://www.careercup.com/book>。
里面有150道程序员面试题目及相应的解答。书中大部分是编程题目，
并且配有相应的java程序(有些地方有错误或是有更优的方案)。我把书中的题目做了一遍，
并且记录下来，包含自己对问题的一些思路及看法，许多问题给出了两种以上的解答方案。
由于个人平时使用较多的是C++，所以程序是用C++编写，所有的代码都托管在Github上：

<https://github.com/Hawstein/cracking-the-coding-interview>

记录下来的主要目的是加深自己对问题的理解，如果此外还能对一两个人起到帮助作用，
那真真是再好不过了。:P

## 目录

### Chapter 1 | Arrays and Strings

1.1
[Implement an algorithm to determine if a string has all unique characters. What if you can not use additional data structures?](/2012/11/28/1.1/)

1.2
[Write code to reverse a C-Style String. (C-String means that “abcd” is represented as five characters, including the null character.)](/2012/12/04/1.2/)

1.3
[Design an algorithm and write code to remove the duplicate characters in a string without using any additional buffer. NOTE: One or two additional variables are fine. An extra copy of the array is not.FOLLOW UP Write the test cases for this method.](/2012/12/04/1.3/)

1.4
[Write a method to decide if two strings are anagrams or not.](/2012/12/06/1.4/)

1.5
[Write a method to replace all spaces in a string with ‘%20’.](/2012/12/07/1.5/)

1.6
[Given an image represented by an NxN matrix, where each pixel in the image is 4 bytes, write a method to rotate the image by 90 degrees. Can you do this in place?](/2012/12/08/1.6/)

1.7
[Write an algorithm such that if an element in an MxN matrix is 0, its entire row and column is set to 0.](/2012/12/09/1.7/)

1.8
[Assume you have a method isSubstring which checks if one word is a substring of another. Given two strings, s1 and s2, write code to check if s2 is a rotation of s1 using only one call to isSubstring (i.e., “waterbottle” is a rotation of “erbottlewat”).](/2012/12/10/1.8/)

### Chapter 2 | Linked Lists

2.1
[Write code to remove duplicates from an unsorted linked list. FOLLOW UP How would you solve this problem if a temporary buffer is not allowed?](/2012/12/13/2.1/)

2.2
[Implement an algorithm to find the nth to last element of a singly linked list.](/2012/12/14/2.2/)

2.3
[Implement an algorithm to delete a node in the middle of a single linked list, given only access to that node. EXAMPLE Input: the node ‘c’ from the linked list a->b->c->d->e Result: nothing is returned, but the new linked list looks like a->b->d->e](/2012/12/15/2.3/)

2.4
[You have two numbers represented by a linked list, where each node contains a single digit. The digits are stored in reverse order, such that the 1’s digit is at the head of the list. Write a function that adds the two numbers and returns the sum as a linked list. EXAMPLE
Input: (3 -> 1 -> 5) + (5 -> 9 -> 2)
Output: 8 -> 0 -> 8
](/2012/12/16/2.4/)

2.5
[Given a circular linked list, implement an algorithm which returns node at the beginning of the loop.
DEFINITION
Circular linked list: A (corrupt) linked list in which a node’s next pointer points to an
earlier node, so as to make a loop in the linked list.
EXAMPLE
input: A -> B -> C -> D -> E -> C (the same C as earlier)
output: C
](/2012/12/17/2.5/)

### Chapter 3 | Stacks and Queues

3.1
[Describe how you could use a single array to implement three stacks.
](/2012/12/18/3.1/)

3.2
[How would you design a stack which, in addition to push and pop, also has a function
min which returns the minimum element? Push, pop and min should all operate in
O(1) time.
](/2012/12/19/3.2/)

3.3
[Imagine a (literal) stack of plates. If the stack gets too high, it might topple. 
Therefore, in real life, we would likely start a new stack when the previous stack exceeds
some threshold. Implement a data structure SetOfStacks that mimics this. 
SetOfStacks should be composed of several stacks, and should create a new stack once
the previous one exceeds capacity. SetOfStacks.push() and SetOfStacks.pop() should
behave identically to a single stack (that is, pop() should return the same values as it
would if there were just a single stack).
FOLLOW UP
Implement a function popAt(int index) which performs a pop operation on a specific
sub-stack.
](/2012/12/20/3.3/)

3.4
[In the classic problem of the Towers of Hanoi, you have 3 rods and N disks of different
sizes which can slide onto any tower. The puzzle starts with disks sorted in ascending
order of size from top to bottom (e.g., each disk sits on top of an even larger one). You
have the following constraints:	
(A) Only one disk can be moved at a time.
(B) A disk is slid off the top of one rod onto the next rod.
(C) A disk can only be placed on top of a larger disk.
Write a program to move the disks from the first rod to the last using Stacks.
](/2012/12/21/3.4/)

3.5
[Implement a MyQueue class which implements a queue using two stacks.
](/2012/12/22/3.5/)

3.6
[Write a program to sort a stack in ascending order. You should not make any assump-
tions about how the stack is implemented. The following are the only functions that
should be used to write this program: push | pop | peek | isEmpty.
](/2012/12/23/3.6/)

### Chapter 4 | Trees and Graphs

4.1
[Implement a function to check if a tree is balanced. For the purposes of this question,
a balanced tree is defined to be a tree such that no two leaf nodes differ in distance
from the root by more than one.
](/2012/12/24/4.1/)

4.2
[Given a directed graph, design an algorithm to find out whether there is a route between two nodes.
](/2012/12/25/4.2/)

4.3
[Given a sorted (increasing order) array, write an algorithm to create a binary tree with
minimal height.
](/2012/12/26/4.3/)

4.4
[Given a binary search tree, design an algorithm which creates a linked list of all the
nodes at each depth (i.e., if you have a tree with depth D, you’ll have D linked lists).
](/2012/12/27/4.4/)

4.5
[Write an algorithm to find the ‘next’ node (i.e., in-order successor) of a given node in
a binary search tree where each node has a link to its parent.
](/2012/12/28/4.5/)

4.6
[Design an algorithm and write code to find the first common ancestor of two nodes
in a binary tree. Avoid storing additional nodes in a data structure. NOTE: This is not
necessarily a binary search tree.
](/2012/12/29/4.6/)

4.7
[You have two very large binary trees: T1, with millions of nodes, and T2, with hun-
dreds of nodes. Create an algorithm to decide if T2 is a subtree of T1.
](/2012/12/30/4.7/)

4.8
[You are given a binary tree in which each node contains a value. Design an algorithm to print all paths which sum up to that value. Note that it can be any path in the tree - it does not have to start at the root.](/2012/12/31/4.8/)

### Chapter 5 | Bit Manipulation

5.1
[You are given two 32-bit numbers, N and M, and two bit positions, i and j. Write a
method to set all bits between i and j in N equal to M (e.g., M becomes a substring of
N located at i and starting at j).
EXAMPLE:
Input: N = 10000000000, M = 10101, i = 2, j = 6
Output: N = 10001010100
](/2013/01/01/5.1/)

5.2
[Given a (decimal - e.g. 3.72) number that is passed in as a string, print the binary 
representation. If the number can not be represented accurately in binary, print “ERROR”
](/2013/01/02/5.2/)

5.3
[Given an integer, print the next smallest and next largest number that have the same
number of 1 bits in their binary representation.
](/2013/01/04/5.3/)

5.4
[Explain what the following code does: ((n & (n-1)) == 0).
](/2013/01/04/5.4/)

5.5
[Write a function to determine the number of bits required to convert integer A to
integer B. Input: 31, 14 Output: 2
](/2013/01/04/5.5/)

5.6
[Write a program to swap odd and even bits in an integer with as few instructions as
possible (e.g., bit 0 and bit 1 are swapped, bit 2 and bit 3 are swapped, etc).
](/2013/01/04/5.6/)

5.7
[An array A[1...n] contains all the integers from 0 to n except for one number which is missing. In this problem, we cannot access an entire integer in A with a single operation. The elements of A are represented in binary, and the only operation we can use to access them is “fetch the jth bit of A[i]”, which takes constant time. Write code to find the missing integer. Can you do it in O(n) time?](/2013/01/04/5.7/)

### Chapter 6 | Brain Teasers

[Cracking the coding interview--Q6.1~Q6.6](/2013/01/05/brain-teasers/)

### Chapter 7 | Object Oriented Design

XDDDDDD

### Chapter 8 | Recursion

8.1
[Write a method to generate the nth Fibonacci number.
](/2013/01/08/8.1/)

8.2
[Imagine a robot sitting on the upper left hand corner of an NxN 
grid. The robot can only move in two directions: right and down. How 
many possible paths are there for the robot?
FOLLOW UP
Imagine certain squares are “off limits”, such that the robot can 
not step on them.
Design an algorithm to get all possible paths for the robot.
](/2013/01/09/8.2/)

8.3
[Write a method that returns all subsets of a set.
](/2013/01/10/8.3/)

8.4
[Write a method to compute all permutations of a string.
](/2013/01/11/8.4/)

8.5
[Implement an algorithm to print all valid (e.g., properly opened 
and closed) combinations of n-pairs of parentheses.
EXAMPLE:
input: 3 (e.g., 3 pairs of parentheses)
output: ()()(), ()(()), (())(), ((()))
](/2013/01/12/8.5/)

8.6
[Implement the “paint fill” function that one might see on many 
image editing programs. That is, given a screen (represented by a 2 
dimensional array of Colors), a point, and a new color, fill in the 
surrounding area until you hit a border of that color.
](/2013/01/13/8.6/)

8.7
[Given an infinite number of quarters (25 cents), dimes (10 cents), 
nickels (5 cents) and pennies (1 cent), write code to calculate the
number of ways of representing n cents.
](/2013/01/14/8.7/)

8.8
[Write an algorithm to print all ways of arranging eight queens on 
a chess board so that none of them share the same row, column or 
diagonal.
](/2013/01/15/8.8/)

### Chapter 9 | Sorting and Searching

9.1
[You are given two sorted arrays, A and B, and A has a large enough 
buffer at the end to hold B. Write a method to merge B into A in 
sorted order.
](/2013/01/16/9.1/)

9.2
[Write a method to sort an array of strings so that all the anagrams 
are next to each other.
](/2013/01/17/9.2/)

9.3
[Given a sorted array of n integers that has been rotated an unknown 
number of times, give an O(log n) algorithm that finds an element in 
the array. You may assume that the array was originally sorted in 
increasing order.
EXAMPLE:
Input: find 5 in array (15 16 19 20 25 1 3 4 5 7 10 14)
Output: 8 (the index of 5 in the array)
](/2013/01/18/9.3/)

9.4
[If you have a 2 GB file with one string per line, which sorting 
algorithm would you use to sort the file and why?
](/2013/01/19/9.4/)

9.5
[Given a sorted array of strings which is interspersed with empty 
strings, write a method to find the location of a given string.
Example: find “ball” in [“at”, “”, “”, “”, “ball”, “”, “”, “car”, 
“”, “”, “dad”, “”, “”] will return 4
Example: find “ballcar” in
[“at”, “”, “”, “”, “”, “ball”, “car”, “”,
“”, “dad”, “”, “”] will return -1
](/2013/01/20/9.5/)

9.6
[Given a matrix in which each row and each column is sorted, write a 
method to find an element in it.
](/2013/01/21/9.6/)

9.7
[A circus is designing a tower routine consisting of people standing 
atop one another’s shoulders. For practical and aesthetic reasons, 
each person must be both shorter and lighter than the person below 
him or her. Given the heights and weights of each person in the 
circus, write a method to compute the largest possible number of 
people in such a tower.
EXAMPLE:
Input (ht, wt): (65, 100) (70, 150) (56, 90) (75, 190) (60, 95) (68, 110)
Output: The longest tower is length 6 and includes from top to bottom: (56, 90)
(60,95) (65,100) (68,110) (70,150) (75,190)
](/2013/01/22/9.7/)

### Chapter 10 | Mathematical

[Cracking the coding interview--Q10.1~Q10.7
](/2013/01/25/ctci-ch10-math/)

### Chapter 11 | Testing

[Cracking the coding interview--Q11.1~Q11.6
](/2013/01/26/ctci-ch11-testing/)

### Chapter 12 | System Design and Memory Limits

12.1
[If you were integrating a feed of end of day stock price 
information (open, high, low,and closing price) for 5,000 
companies, how would you do it? You are responsible for
the development, rollout and ongoing monitoring and maintenance 
of the feed. Describe the different methods you considered and 
why you would recommend your approach. The feed is delivered once 
per trading day in a comma-separated format via an FTP site. The 
feed will be used by 1000 daily users in a web application.
](/2013/01/27/12.1/)

12.2
[How would you design the data structures for a very large 
social network (Facebook,LinkedIn, etc)? Describe how you would 
design an algorithm to show the connection, or path, between two 
people (e.g., Me -> Bob -> Susan -> Jason -> You).
](/2013/01/28/12.2/)

12.3
[Given an input file with four billion integers, provide an 
algorithm to generate an integer which is not contained in the 
file. Assume you have 1 GB of memory.
FOLLOW UP
What if you have only 10 MB of memory?
](/2013/01/29/12.3/)

12.4
[You have an array with all the numbers from 1 to N, where N is 
at most 32,000. The array may have duplicate entries and you do 
not know what N is. With only 4KB of memory available, how would 
you print all duplicate elements in the array?
](/2013/01/30/12.4/)

12.5
[If you were designing a web crawler, how would you avoid getting 
into infinite loops?
](/2013/01/31/12.5/)

12.6
[You have a billion urls, where each is a huge page. How do you 
detect the duplicate documents?
](/2013/02/01/12.6/)

12.7
[You have to design a database that can store terabytes of data. 
It should support efficient range queries. How would you do it?
](/2013/02/02/12.7/)

### Chapter 13 | C++

13.1
[Write a method to print the last K lines of an input file using C++.
](/2013/02/10/13.1/)

13.2
[Compare and contrast a hash table vs. an STL map. How is a hash 
table implemented?If the number of inputs is small, what data 
structure options can be used instead of a hash table?
](/2013/02/11/13.2/)

13.3
[How do virtual functions work in C++?
](/2013/02/12/13.3/)

13.4
[What is the difference between deep copy and shallow copy? Explain 
how you would use each.
](/2013/02/13/13.4/)

13.5
[What is the significance of the keyword “volatile” in C?
](/2013/02/13/13.5/)

13.6
[What is name hiding in C++?
](/2013/02/14/13.6/)

13.7
[Why does a destructor in base class need to be declared virtual?
](/2013/02/14/13.7/)

13.8
[Write a method that takes a pointer to a Node structure as a 
parameter and returns a complete copy of the passed-in data 
structure. The Node structure contains two pointers to other Node 
structures.
](/2013/02/14/13.8/)

13.9
[Write a smart pointer (smart_ptr) class.
](/2013/02/14/13.9/)

### Chapter 14 | Java

Pass

### Chapter 15 | Databases

15.1
[Write a method to find the number of employees in each department.
](/2013/02/14/15.1/)

15.2
[What are the different types of joins? Please explain how they 
differ and why certain types are better in certain situations.
](/2013/02/15/15.2/)

15.3
[What is denormalization? Explain the pros and cons.
](/2013/02/16/15.3/)

15.4
[Draw an entity-relationship diagram for a database with companies, 
people, and professionals (people who work for companies).
](/2013/02/16/15.4/)

15.5
[Imagine a simple database storing information for students’ grades. 
Design what this database might look like, and provide a SQL query 
to return a list of the honor roll students (top 10%), sorted by 
their grade point average.
](/2013/02/17/15.5/)

### Chapter 16 | Low Level

16.1
[Explain the following terms: virtual memory, page fault, thrashing.
](/2013/02/08/16.1/)

16.5
[Write a program to find whether a machine is big endian or little 
endian.
](/2013/02/09/16.5/)

16.10
[Write a function called my2DAlloc which allocates a two dimensional 
array. Minimize the number of calls to malloc and make sure that the 
memory is accessible by the notation arr[i][j].	
](/2013/02/09/16.10/)

### Chapter 17 | Networking

17.1
[Explain what happens, step by step, after you type a URL into a 
browser. Use as much detail as possible.	
](/2013/02/03/17.1/)

17.2
[Explain any common routing protocol in detail. For example: BGP, 
OSPF, RIP.
](/2013/02/04/17.2/)

17.3
[Compare and contrast the IPv4 and IPv6 protocols.
](/2013/02/05/17.3/)

17.4
[What is a network / subnet mask? Explain how host A sends a 
message / packet to host B when: (a) both are on same network 
and (b) both are on different networks.
Explain which layer makes the routing decision and how.
](/2013/02/06/17.4/)

17.5
[What are the differences between TCP and UDP? Explain how TCP 
handles reliable delivery (explain ACK mechanism), flow control 
(explain TCP sender’s / receiver’s window) and congestion control.	
](/2013/02/07/17.5/)

### Chapter 18 | Threads and Locks

18.1
[What’s the difference between a thread and a process?
](/2013/02/18/18.1/)

18.2
[How can you measure the time spent in a context switch?
](/2013/02/18/18.2/)

18.3
[Implement a singleton design pattern as a template such that, for 
any given class Foo, you can call Singleton::instance() and get a 
pointer to an instance of a singleton of type Foo. Assume the 
existence of a class Lock which has acquire() and release()
methods. How could you make your implementation thread safe and 
exception safe?
](/2013/02/19/18.3/)

18.5
[Suppose we have the following code:...
i) 	Can you design a mechanism to make sure that B is executed after 
A, and C is executed after B?
ii) Suppose we have the following code to use class Foo. We do not 
know how the threads will be scheduled in the OS.
Can you design a mechanism to make sure that all the methods will be 
executed in sequence?
](/2013/02/19/18.5/)

### Chapter 19 | Moderate

19.1
[Write a function to swap a number in place without temporary 
variables.
](/2013/02/20/19.1/)

19.2
[Design an algorithm to figure out if someone has won in a game of 
tic-tac-toe.
](/2013/02/20/19.2/)

19.3
[Write an algorithm which computes the number of trailing zeros in n 
factorial.
](/2013/02/20/19.3/)

19.4
[Write a method which finds the maximum of two numbers. You should 
not use if-else or any other comparison operator.	
](/2013/02/21/19.4/)

19.5
[The Game of Master Mind is played as follows:	
The computer has four slots containing balls that are red (R), 
yellow (Y), green (G) or blue (B). For example, the computer might 
have RGGB (e.g., Slot #1 is red, Slots #2 and #3 are green, Slot #4 
is blue).
You, the user, are trying to guess the solution. You might, for 
example, guess YRGB. When you guess the correct color for the 
correct slot, you get a “hit”. If you guess a color that exists but 
is in the wrong slot, you get a “pseudo-hit”. For example, the
guess YRGB has 2 hits and one pseudo hit.
For each guess, you are told the number of hits and pseudo-hits.
Write a method that, given a guess and a solution, returns the 
number of hits and pseudo hits.
](/2013/02/21/19.5/)

19.7
[You are given an array of integers (both positive and negative). 
Find the continuous sequence with the largest sum. Return the sum.	
](/2013/02/22/19.7/)

19.8
[Design a method to find the frequency of occurrences of any given 
word in a book.
](/2013/02/22/19.8/)

19.10
[Write a method to generate a random number between 1 and 7, given a 
method that generates a random number between 1 and 5 (i.e., 
implement rand7() using rand5()).	
](/2013/02/23/19.10/)

19.11
[Design an algorithm to find all pairs of integers within an array 
which sum to a specified value.	
](/2013/02/23/19.11/)

### Chapter 20 | Hard

20.1
[Write a function that adds two numbers. You should not use + or any 
arithmetic operators.
](/2013/02/24/20.1/)

20.2
[Write a method to shuffle a deck of cards. It must be a perfect 
shuffle - in other words, each 52! permutations of the deck has to 
be equally likely. Assume that you are given a random number 
generator which is perfect.
](/2013/02/24/20.2/)

20.3
[Write a method to randomly generate a set of m integers from an 
array of size n. Each element must have equal probability of being 
chosen.
](/2013/02/25/20.3/)

20.4
[Write a method to count the number of 2s between 0 and n.
](/2013/03/01/20.4/)

20.5
[You have a large text file containing words. Given any two words, 
find the shortest distance (in terms of number of words) between 
them in the file. Can you make the searching operation in O(1) time? 
What about the space complexity for your solution?
](/2013/03/02/20.5/)

20.6
[Describe an algorithm to find the largest 1 million numbers in 1 
billion numbers. Assume that the computer memory can hold all one 
billion numbers.
](/2013/03/03/20.6/)

20.7
[Write a program to find the longest word made of other words in a 
list of words.
EXAMPLE
Input: test, tester, testertest, testing, testingtester
Output: testingtester
](/2013/03/04/20.7/)

20.8
[Given a string s and an array of smaller strings T, design a method 
to search s for each small string in T.
](/2013/03/05/20.8/)

20.9
[Numbers are randomly generated and passed to a method. Write a 
program to find and maintain the median value as new values are 
generated.
](/2013/03/06/20.9/)

20.11
[Imagine you have a square matrix, where each cell is filled with 
either black or white. Design an algorithm to find the maximum 
subsquare such that all four borders are filled with black pixels.
](/2013/03/07/20.11/)

20.12
[Given an NxN matrix of positive and negative integers, write code 
to find the submatrix with the largest possible sum.	
](/2013/03/08/20.12/)
