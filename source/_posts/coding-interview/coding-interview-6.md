---
title: 剑指Offer面试题6：从尾到头打印链表
date: 2022-02-20 21:28:00
categories: Coding Interview Edition 2
tags:
---

**链表**是由指针把若干个节点连接而成的链状结构，它是一种动态数据结构，在创建之时无须知道长度。当插入一个节点的时候，只需要为新节点分配内存然后调整指针的指向来确保新节点被链接到了链表中。可以理解为“按需分配”，空间利用率较高。

<!--more-->

|输入一个链表的头节点，从尾到头反过来打印出每个节点的值。</br>LeetCode力扣链接：[https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/]|
|---|

由于这里涉及的链表是单向的，只能从头到尾遍历，题目却要从尾到头打印，这是典型的“后进先出”，所以想到可以用栈来实现。

### 思路
有两种解题方式，一种是循环，另一种是递归。
- 循环：首先遍历链表将各节点入栈，然后再遍历出栈即可。
- 递归：递归的本质是栈结构。我们每访问到一个节点的时候，先递归输出它后面的节点，再输出该节点自身，这样链表的输出结果就反过来了。

虽然递归的代码更简洁，但是当链表很长的时候，就会导致函数调用的层级很深，从而可能导致函数调用栈溢出。

### 原文代码(C/C++)
```
// 链表定义如下:
struct ListNode
{
    int m_nValue;
    ListNode* m_pNext;
}
```
https://github.com/zhedahht/CodingInterviewChinese2/blob/master/06_PrintListInReversedOrder/PrintListInReversedOrder.cpp


### 个人编写代码(C#)
```
// 链表定义如下：
public class ListNode
{
    public int val;
    public ListNode next;
    public ListNode(int x) { val = x; }
}
```

#### 循环解法
C#的Stack类型存在ToArray的方法，相当于替我们封装实现了遍历出栈并生成数组的操作。
```
public int[] ReversePrintIteration(ListNode head)
{
    Stack<int> stack = new Stack<int>();
    while(head != null)
    {
        stack.Push(head.val);
        head = head.next;
    }

    return stack.ToArray();
}
```
由于力扣题库要求返回int数组，所以我们也可以利用数组使用下标直接读取的特性，在第一次遍历的时候不需要将节点的值入栈，只需要获得节点数并创建对应长度的数组，最后对数组从后往前遍历的同时赋上链表第二次遍历的值。
```
public int[] ReversePrintIteration2(ListNode head)
{
    int count = 0;
    ListNode p = head;
    while (p != null)
    {
        ++count;
        p = p.next;
    }

    p = head;
    int[] result = new int[count];
    for(int i = count-1; i >= 0; --i, p = p.next)
    {
        result[i] = p.val;
    }

    return result;
}
```

#### 递归解法
由于力扣题库要求返回int数组而非直接打印，我们需要声明并初始化一个全局int数组变量。
```
public List<int> result = new List<int>();

public int[] ReversePrintRecursion(ListNode head)
{
    ReversePrintRecursionList(head);

    return result.ToArray();
}

private void ReversePrintRecursionList(ListNode head)
{
    if (head == null)
        return;

    ReversePrintRecursionList(head.next);
    result.Add(head.val);
}
```
