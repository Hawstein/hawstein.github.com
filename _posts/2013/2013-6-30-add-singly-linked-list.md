---
layout: post
category: Programming
title: 求两个单链表的和
---

## 题目

两个单链表（singly linked list），每一个节点里面一个0-9的数字，
输入就相当于两个大数了。然后返回这两个数的和（一个新list）。这两个输入的list
长度相等。 要求是：1. 不用递归。2. 要求算法在最好的情况下，只遍历两个list一次，
最差的情况下两遍。

## 解答

这是陈利人同学今天发在待字闺中的面试编程题目，看了一下解答，
发现要么需要遍历链表两次，要么需要额外的存储空间，难道就没有更优的解法了吗？
想了一下，发现还是有的。

利人同学的分析我就不再复述了，这是传送门：
[戳我吧！](http://chuansong.me/n/89263)

顺便推荐一下陈利人同学的微博：[@陈利人](http://weibo.com/lirenchen)
和他的公众平台：待字闺中。一看这同学的名字就值得关注，利人，你值得拥有。XD

OK，我们把这个问题具体化一下吧：(这里就不再考虑从低到高存等blabla情况)

>> 两个单链表，每个节点存储一个0-9的数字，那么一个单链表就表示一个大数。
>> 从高位到低位存，即表头对应的是这个大数的最高位。两个链表的长度相等，
>> 我们要返回一个新的单链表，是这两个输入链表代表的数的和。我们不能使用递归，
>> 不能使用额外的存储空间，即空间复杂度是O(1)。只遍历输入链表一次，
>> 输出链表也是单链表(没有前向指针)。

既然只能遍历两个输入链表一次，那我们就从高位加起呗。在这种限制条件下，
这是唯一的出路。然后呢？进位咋整？先加高位，再加低位，
低位产生的进位怎么加到高位去？我们可没有前向指针哦亲。既然没有前向指针，
我们就让一个临时指针指向高位，当低位相加产生进位时，我们就可以操作高位了。
让我们看看图示：

	输入链表1： 1 2 3
	输入链表2： 1 2 8
	输出链表：  2 4 
	两个指针：    p q

当指向输出链表当前结点的指针q发现3+8=11，产生进位，指向高位的p就将结点值加1。
**注意，两个0-9的数相加，要么不进位，要么进位为1，只有两种情况**。因此，
我们不用考虑进位是其它数，这一点很重要，后面会看到的。

这样就OK了吗？当然不是，如果你遇上连续进位，怎么破？请看下面的情况：

	输入链表1： 1 2 3 4 5
	输入链表2： 1 7 6 5 9

显然，指向高位的指针p总是紧跟着指向当前结点的指针q是不行的，
这样当遇上连续进位时，比p更高位的位也需要改变。既然p不能紧跟着q，
我们就不让它们紧挨着，给它们产生点距离。考虑一下，什么情况下会产生连续进位？
9! 嗯，遇上9的时候。它要连续进位到哪一位？不为9的那一位。因此，指针p
要停留在和不为9的那一位上，看图示：

	输入链表1： 1 2 3 4 5
	输入链表2： 1 7 6 5 9
	输出链表：  2 9 9 9
	两个指针：  p       q
	
这回当q发现，需要进位了，只需要把p所指结点加1，然后把p，q间的结点都置0即可。
为什么都置0了呢，**因为进位只可能是1**，9+1=10，留在该位的自然是0了。

分析完毕，这种方法在任何时候都只需要遍历输入链表一次，空间复杂度O(1)。

代码如下：

```cpp
#include <iostream>
using namespace std;

struct Node{
    char data; // 保存0-9，1字节即可。
    Node *next;
};

// version 1
Node* AddSinglyLinkedList(Node* list1, Node* list2){
    Node *q1=list1, *q2=list2;
    Node *ans=NULL, *p=NULL, *pre=NULL, *q=NULL;//pre指向q的前驱结点
    bool highest = false;//只有最高位可能有进位，才考虑去掉前导0。0+0=0不去掉
    // 处理最高位的结点
    char first = q1->data + q2->data;
    if(first >= 9){// 和大于等于9，要多开一个结点来保存进位
        highest = true;
        ans = new Node();
        pre = new Node();
        pre->data = first % 10;
        ans->next = pre;
        if(first > 9){// 和大于9，最高位为1
            ans->data = 1;
            p = pre;
        }
        else{ // 和为9，最高位为0，因为有可能连续进位使其变为1，p指向它
            ans->data = 0;
            p = ans;
        }
    }
    else{// 和小于9
        ans = new Node();
        ans->data = first;
        p = pre = ans;
    }
    // 处理后面的结点
    while((q1=q1->next) && (q2=q2->next)){
        q = new Node(); // 当前输出结点
        pre->next = q; // 上一结点指向当前结点
        char num = q1->data + q2->data;
        q->data = num % 10; // q结点存储的值
        if(num > 9){
            p->data = p->data + 1; // p指向的结点值加1
            for(p=p->next; p!=q; p=p->next)// p,q间的结点值置0，p指向q
                p->data = 0;
        }
        else if(num < 9){// 和小于9，p移动到当前位置q
            p = q;
        }
        pre = q; // 更新前一结点pre的指针
    }// num等于9时，p不用更新
    
    if(highest && ans->data == 0) // 如果最高位没有因为连续进位而变成1
        ans = ans->next;
    
    return ans;
}
Node* MakeLinkedList(int d[], int n){
    Node *head = new Node();
    head->data = (char)d[0];
    Node *pre=head, *cur=head;
    for(int i=1; i<n; ++i){
        cur = new Node();
        cur->data = (char)d[i];
        pre->next = cur;
        pre = cur;
    }
    return head;
}

// version 2, 在处理最高位上，比version1要简洁
Node* AddSinglyLinkedList1(Node* list1, Node* list2){
    Node *q1=list1, *q2=list2;
    Node *ans=NULL, *p=NULL, *pre=NULL, *q=NULL;//pre指向q的前驱结点
    
	// 处理最高位的结点，先不考虑进位问题
    char first = q1->data + q2->data;
    ans = new Node();
    ans->data = first; // 第一个结点不管有没进位，都存着先
    p = pre = ans;
    
    // 处理后面的结点
    while((q1=q1->next) && (q2=q2->next)){
        q = new Node(); // 当前输出结点
        pre->next = q; // 上一结点指向当前结点
        char num = q1->data + q2->data;
        q->data = num % 10; // q结点存储的值
        if(num > 9){
            p->data = p->data + 1; // p指向的结点值加1
            for(p=p->next; p!=q; p=p->next)// p,q间的结点值置0，p指向q
                p->data = 0;
        }
        else if(num < 9){// 和小于9，p移动到当前位置q
            p = q;
        }
        pre = q; // 更新前一结点pre的指针
    }// num等于9时，p不用更新
    
    if(ans->data > 9){// 全部加完后，如果最高位需要进位
        q = new Node();
        q->data = 1;
        ans->data = ans->data - 10;
        q->next = ans;
        ans = q;
    }
    
    return ans;
}

int main(){
    int n = 7;
    int a[] = {
        2,0,0,0,7,0,1
    };
    int b[] = {
        7,9,9,9,9,9,9
    };
    Node *list1 = MakeLinkedList(a, n);
    Node *list2 = MakeLinkedList(b, n);
    Node *ans = AddSinglyLinkedList(list1, list2);
    for(; ans; ans=ans->next)
        cout<<(int)ans->data;
    cout<<endl;
    Node *ans1 = AddSinglyLinkedList1(list1, list2);
    for(; ans1; ans1=ans1->next)
        cout<<(int)ans1->data;
    return 0;
}
```

