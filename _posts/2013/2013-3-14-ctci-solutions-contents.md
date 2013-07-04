---
layout: post
category: Programming
title: Cracking the coding interview--问题与解答
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

**Chapter 1 | Arrays and Strings**

1.1
[Implement an algorithm to determine if a string has all unique 
characters. What if you can not use additional data structures?
](/posts/1.1.html)

1.2
[Write code to reverse a C-Style String. (C-String means that “abcd” 
is represented as five characters, including the null character.)
](/posts/1.2.html)

1.3
[Design an algorithm and write code to remove the duplicate characters in a string
without using any additional buffer. NOTE: One or two additional variables are fine.
An extra copy of the array is not.
FOLLOW UP
Write the test cases for this method.
](/posts/1.3.html)

1.4
[Write a method to decide if two strings are anagrams or not.
](/posts/1.4.html)

1.5
[Write a method to replace all spaces in a string with ‘%20’.
](/posts/1.5.html)

1.6
[Given an image represented by an NxN matrix, where each pixel in the image is 4
bytes, write a method to rotate the image by 90 degrees. Can you do this in place?
](/posts/1.6.html)

1.7
[Write an algorithm such that if an element in an MxN matrix is 0, its entire row and
column is set to 0.
](/posts/1.7.html)

1.8
[Assume you have a method isSubstring which checks if one word is a substring of
another. Given two strings, s1 and s2, write code to check if s2 is a rotation of s1 using
only one call to isSubstring (i.e., “waterbottle” is a rotation of “erbottlewat”).
](/posts/1.8.html)

**Chapter 2 | Linked Lists**

2.1
[Write code to remove duplicates from an unsorted linked list.
FOLLOW UP
How would you solve this problem if a temporary buffer is not allowed?
](/posts/2.1.html)

2.2
[Implement an algorithm to find the nth to last element of a singly linked list.
](/posts/2.2.html)

2.3
[Implement an algorithm to delete a node in the middle of a single linked list, given
only access to that node.
EXAMPLE
Input: the node ‘c’ from the linked list a->b->c->d->e
Result: nothing is returned, but the new linked list looks like a->b->d->e
](/posts/2.3.html)

2.4
[You have two numbers represented by a linked list, where each node contains a single 
digit. The digits are stored in reverse order, such that the 1’s digit is at the head of
the list. Write a function that adds the two numbers and returns the sum as a linked
list.
EXAMPLE
Input: (3 -> 1 -> 5) + (5 -> 9 -> 2)
Output: 8 -> 0 -> 8
](/posts/2.4.html)

2.5
[Given a circular linked list, implement an algorithm which returns node at the beginning of the loop.
DEFINITION
Circular linked list: A (corrupt) linked list in which a node’s next pointer points to an
earlier node, so as to make a loop in the linked list.
EXAMPLE
input: A -> B -> C -> D -> E -> C (the same C as earlier)
output: C
](/posts/2.5.html)

**Chapter 3 | Stacks and Queues**

3.1
[Describe how you could use a single array to implement three stacks.
](/posts/3.1.html)

3.2
[How would you design a stack which, in addition to push and pop, also has a function
min which returns the minimum element? Push, pop and min should all operate in
O(1) time.
](/posts/3.2.html)

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
](/posts/3.3.html)

3.4
[In the classic problem of the Towers of Hanoi, you have 3 rods and N disks of different
sizes which can slide onto any tower. The puzzle starts with disks sorted in ascending
order of size from top to bottom (e.g., each disk sits on top of an even larger one). You
have the following constraints:	
(A) Only one disk can be moved at a time.
(B) A disk is slid off the top of one rod onto the next rod.
(C) A disk can only be placed on top of a larger disk.
Write a program to move the disks from the first rod to the last using Stacks.
](/posts/3.4.html)

3.5
[Implement a MyQueue class which implements a queue using two stacks.
](/posts/3.5.html)

3.6
[Write a program to sort a stack in ascending order. You should not make any assump-
tions about how the stack is implemented. The following are the only functions that
should be used to write this program: push | pop | peek | isEmpty.
](/posts/3.6.html)

**Chapter 4 | Trees and Graphs**

4.1
[Implement a function to check if a tree is balanced. For the purposes of this question,
a balanced tree is defined to be a tree such that no two leaf nodes differ in distance
from the root by more than one.
](/posts/4.1.html)

4.2
[Given a directed graph, design an algorithm to find out whether there is a route between two nodes.
](/posts/4.2.html)

4.3
[Given a sorted (increasing order) array, write an algorithm to create a binary tree with
minimal height.
](/posts/4.3.html)

4.4
[Given a binary search tree, design an algorithm which creates a linked list of all the
nodes at each depth (i.e., if you have a tree with depth D, you’ll have D linked lists).
](/posts/4.4.html)

4.5
[Write an algorithm to find the ‘next’ node (i.e., in-order successor) of a given node in
a binary search tree where each node has a link to its parent.
](/posts/4.5.html)

4.6
[Design an algorithm and write code to find the first common ancestor of two nodes
in a binary tree. Avoid storing additional nodes in a data structure. NOTE: This is not
necessarily a binary search tree.
](/posts/4.6.html)

4.7
[You have two very large binary trees: T1, with millions of nodes, and T2, with hun-
dreds of nodes. Create an algorithm to decide if T2 is a subtree of T1.
](/posts/4.7.html)

4.8
[You are given a binary tree in which each node contains a value. Design an algorithm
to print all paths which sum up to that value. Note that it can be any path in the tree
- it does not have to start at the root.
](/posts/4.8.html)

**Chapter 5 | Bit Manipulation**

5.1
[You are given two 32-bit numbers, N and M, and two bit positions, i and j. Write a
method to set all bits between i and j in N equal to M (e.g., M becomes a substring of
N located at i and starting at j).
EXAMPLE:
Input: N = 10000000000, M = 10101, i = 2, j = 6
Output: N = 10001010100
](/posts/5.1.html)

5.2
[Given a (decimal - e.g. 3.72) number that is passed in as a string, print the binary 
representation. If the number can not be represented accurately in binary, print “ERROR”
](/posts/5.2.html)

5.3
[Given an integer, print the next smallest and next largest number that have the same
number of 1 bits in their binary representation.
](/posts/5.3.html)

5.4
[Explain what the following code does: ((n & (n-1)) == 0).
](/posts/5.4.html)

5.5
[Write a function to determine the number of bits required to convert integer A to
integer B. Input: 31, 14 Output: 2
](/posts/5.5.html)

5.6
[Write a program to swap odd and even bits in an integer with as few instructions as
possible (e.g., bit 0 and bit 1 are swapped, bit 2 and bit 3 are swapped, etc).
](/posts/5.6.html)

5.7
[An array A[1...n] contains all the integers from 0 to n except for one number 
which is missing. In this problem, we cannot access an entire integer in A with 
a single operation. The elements of A are represented in binary, and the only 
operation we can use to access them is “fetch the jth bit of A[i]”, which
takes constant time. 
Write code to find the missing integer. Can you do it in O(n) time?
](/posts/5.7.html)

**Chapter 6 | Brain Teasers**

[Cracking the coding interview--Q6.1~Q6.6](/posts/brain-teasers.html)

**Chapter 7 | Object Oriented Design**

XDDDDDD

**Chapter 8 | Recursion**

8.1
[Write a method to generate the nth Fibonacci number.
](/posts/8.1.html)

8.2
[Imagine a robot sitting on the upper left hand corner of an NxN 
grid. The robot can only move in two directions: right and down. How 
many possible paths are there for the robot?
FOLLOW UP
Imagine certain squares are “off limits”, such that the robot can 
not step on them.
Design an algorithm to get all possible paths for the robot.
](/posts/8.2.html)

8.3
[Write a method that returns all subsets of a set.
](/posts/8.3.html)

8.4
[Write a method to compute all permutations of a string.
](/posts/8.4.html)

8.5
[Implement an algorithm to print all valid (e.g., properly opened 
and closed) combinations of n-pairs of parentheses.
EXAMPLE:
input: 3 (e.g., 3 pairs of parentheses)
output: ()()(), ()(()), (())(), ((()))
](/posts/8.5.html)

8.6
[Implement the “paint fill” function that one might see on many 
image editing programs. That is, given a screen (represented by a 2 
dimensional array of Colors), a point, and a new color, fill in the 
surrounding area until you hit a border of that color.
](/posts/8.6.html)

8.7
[Given an infinite number of quarters (25 cents), dimes (10 cents), 
nickels (5 cents) and pennies (1 cent), write code to calculate the
number of ways of representing n cents.
](/posts/8.7.html)

8.8
[Write an algorithm to print all ways of arranging eight queens on 
a chess board so that none of them share the same row, column or 
diagonal.
](/posts/8.8.html)

**Chapter 9 | Sorting and Searching**

9.1
[You are given two sorted arrays, A and B, and A has a large enough 
buffer at the end to hold B. Write a method to merge B into A in 
sorted order.
](/posts/9.1.html)

9.2
[Write a method to sort an array of strings so that all the anagrams 
are next to each other.
](/posts/9.2.html)

9.3
[Given a sorted array of n integers that has been rotated an unknown 
number of times, give an O(log n) algorithm that finds an element in 
the array. You may assume that the array was originally sorted in 
increasing order.
EXAMPLE:
Input: find 5 in array (15 16 19 20 25 1 3 4 5 7 10 14)
Output: 8 (the index of 5 in the array)
](/posts/9.3.html)

9.4
[If you have a 2 GB file with one string per line, which sorting 
algorithm would you use to sort the file and why?
](/posts/9.4.html)

9.5
[Given a sorted array of strings which is interspersed with empty 
strings, write a method to find the location of a given string.
Example: find “ball” in [“at”, “”, “”, “”, “ball”, “”, “”, “car”, 
“”, “”, “dad”, “”, “”] will return 4
Example: find “ballcar” in
[“at”, “”, “”, “”, “”, “ball”, “car”, “”,
“”, “dad”, “”, “”] will return -1
](/posts/9.5.html)

9.6
[Given a matrix in which each row and each column is sorted, write a 
method to find an element in it.
](/posts/9.6.html)

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
](/posts/9.7.html)

**Chapter 10 | Mathematical**

[Cracking the coding interview--Q10.1~Q10.7
](/posts/ctci-ch10-math.html)

**Chapter 11 | Testing**

[Cracking the coding interview--Q11.1~Q11.6
](/posts/ctci-ch11-testing.html)

**Chapter 12 | System Design and Memory Limits**

12.1
[If you were integrating a feed of end of day stock price 
information (open, high, low,and closing price) for 5,000 
companies, how would you do it? You are responsible for
the development, rollout and ongoing monitoring and maintenance 
of the feed. Describe the different methods you considered and 
why you would recommend your approach. The feed is delivered once 
per trading day in a comma-separated format via an FTP site. The 
feed will be used by 1000 daily users in a web application.
](/posts/12.1.html)

12.2
[How would you design the data structures for a very large 
social network (Facebook,LinkedIn, etc)? Describe how you would 
design an algorithm to show the connection, or path, between two 
people (e.g., Me -> Bob -> Susan -> Jason -> You).
](/posts/12.2.html)

12.3
[Given an input file with four billion integers, provide an 
algorithm to generate an integer which is not contained in the 
file. Assume you have 1 GB of memory.
FOLLOW UP
What if you have only 10 MB of memory?
](/posts/12.3.html)

12.4
[You have an array with all the numbers from 1 to N, where N is 
at most 32,000. The array may have duplicate entries and you do 
not know what N is. With only 4KB of memory available, how would 
you print all duplicate elements in the array?
](/posts/12.4.html)

12.5
[If you were designing a web crawler, how would you avoid getting 
into infinite loops?
](/posts/12.5.html)

12.6
[You have a billion urls, where each is a huge page. How do you 
detect the duplicate documents?
](/posts/12.6.html)

12.7
[You have to design a database that can store terabytes of data. 
It should support efficient range queries. How would you do it?
](/posts/12.7.html)

**Chapter 13 | C++**

13.1
[Write a method to print the last K lines of an input file using C++.
](/posts/13.1.html)

13.2
[Compare and contrast a hash table vs. an STL map. How is a hash 
table implemented?If the number of inputs is small, what data 
structure options can be used instead of a hash table?
](/posts/13.2.html)

13.3
[How do virtual functions work in C++?
](/posts/13.3.html)

13.4
[What is the difference between deep copy and shallow copy? Explain 
how you would use each.
](/posts/13.4.html)

13.5
[What is the significance of the keyword “volatile” in C?
](/posts/13.5.html)

13.6
[What is name hiding in C++?
](/posts/13.6.html)

13.7
[Why does a destructor in base class need to be declared virtual?
](/posts/13.7.html)

13.8
[Write a method that takes a pointer to a Node structure as a 
parameter and returns a complete copy of the passed-in data 
structure. The Node structure contains two pointers to other Node 
structures.
](/posts/13.8.html)

13.9
[Write a smart pointer (smart_ptr) class.
](/posts/13.9.html)

**Chapter 14 | Java**

Pass

**Chapter 15 | Databases**

15.1
[Write a method to find the number of employees in each department.
](/posts/15.1.html)

15.2
[What are the different types of joins? Please explain how they 
differ and why certain types are better in certain situations.
](/posts/15.2.html)

15.3
[What is denormalization? Explain the pros and cons.
](/posts/15.3.html)

15.4
[Draw an entity-relationship diagram for a database with companies, 
people, and professionals (people who work for companies).
](/posts/15.4.html)

15.5
[Imagine a simple database storing information for students’ grades. 
Design what this database might look like, and provide a SQL query 
to return a list of the honor roll students (top 10%), sorted by 
their grade point average.
](/posts/15.5.html)

**Chapter 16 | Low Level**

16.1
[Explain the following terms: virtual memory, page fault, thrashing.
](/posts/16.1.html)

16.5
[Write a program to find whether a machine is big endian or little 
endian.
](/posts/16.5.html)

16.10
[Write a function called my2DAlloc which allocates a two dimensional 
array. Minimize the number of calls to malloc and make sure that the 
memory is accessible by the notation arr[i][j].	
](/posts/16.10.html)

**Chapter 17 | Networking**

17.1
[Explain what happens, step by step, after you type a URL into a 
browser. Use as much detail as possible.	
](/posts/17.1.html)

17.2
[Explain any common routing protocol in detail. For example: BGP, 
OSPF, RIP.
](/posts/17.2.html)

17.3
[Compare and contrast the IPv4 and IPv6 protocols.
](/posts/17.3.html)

17.4
[What is a network / subnet mask? Explain how host A sends a 
message / packet to host B when: (a) both are on same network 
and (b) both are on different networks.
Explain which layer makes the routing decision and how.
](/posts/17.4.html)

17.5
[What are the differences between TCP and UDP? Explain how TCP 
handles reliable delivery (explain ACK mechanism), flow control 
(explain TCP sender’s / receiver’s window) and congestion control.	
](/posts/17.5.html)

**Chapter 18 | Threads and Locks**

18.1
[What’s the difference between a thread and a process?
](/posts/18.1.html)

18.2
[How can you measure the time spent in a context switch?
](/posts/18.2.html)

18.3
[Implement a singleton design pattern as a template such that, for 
any given class Foo, you can call Singleton::instance() and get a 
pointer to an instance of a singleton of type Foo. Assume the 
existence of a class Lock which has acquire() and release()
methods. How could you make your implementation thread safe and 
exception safe?
](/posts/18.3.html)

18.5
[Suppose we have the following code:...
i) 	Can you design a mechanism to make sure that B is executed after 
A, and C is executed after B?
ii) Suppose we have the following code to use class Foo. We do not 
know how the threads will be scheduled in the OS.
Can you design a mechanism to make sure that all the methods will be 
executed in sequence?
](/posts/18.5.html)

**Chapter 19 | Moderate**

19.1
[Write a function to swap a number in place without temporary 
variables.
](/posts/19.1.html)

19.2
[Design an algorithm to figure out if someone has won in a game of 
tic-tac-toe.
](/posts/19.2.html)

19.3
[Write an algorithm which computes the number of trailing zeros in n 
factorial.
](/posts/19.3.html)

19.4
[Write a method which finds the maximum of two numbers. You should 
not use if-else or any other comparison operator.	
](/posts/19.4.html)

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
](/posts/19.5.html)

19.7
[You are given an array of integers (both positive and negative). 
Find the continuous sequence with the largest sum. Return the sum.	
](/posts/19.7.html)

19.8
[Design a method to find the frequency of occurrences of any given 
word in a book.
](/posts/19.8.html)

19.10
[Write a method to generate a random number between 1 and 7, given a 
method that generates a random number between 1 and 5 (i.e., 
implement rand7() using rand5()).	
](/posts/19.10.html)

19.11
[Design an algorithm to find all pairs of integers within an array 
which sum to a specified value.	
](/posts/19.11.html)
