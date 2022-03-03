---
title: 剑指Offer面试题9：用两个栈实现队列
date: 2022-03-03 22:14:13
categories: Coding Interview Edition 2
tags:
---

**栈**是一个非常常见的数据结构，它的特点是先进后出，即最后被压入（push）栈的元素会第一个被弹出（pop）。它被广泛应用于计算机领域，比如操作系统会给每个线程创建一个栈来存储函数调用时各个函数的参数、返回地址及临时变量等。

<!--more-->

## 题目一：用两个栈实现队列

|用两个栈实现一个队列。队列的声明如下，请实现它的两个函数appendTail和deleteHead，分别完成在队列尾部插入节点和在队列头部删除节点的功能。</br>LeetCode力扣链接：[https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/]|
|---|
```
template <typename T> class CQueue
{
    public:
        CQueue(void);
        ~CQueue(void);
    
        void appendTail(const T& node);
        T deleteHead();

    private:
        stack<T> stack1;
        statck<T> stack2;
}
```

用两个栈实现一个队列，我们不妨尝试将两个栈“拼接”成一个队列。

![](/2022/03/03/coding-interview/coding-interview-9/queue-and-two-stacks.png)

如图，一个队列是先进先出的，如同一条通道，两边都是开口；而一个栈是后进先出的，如同一个杯子，只有一边是开口。我们将两个栈的头部连接在一起，就得到一个至少两边都有开口的管道，只不过中间被“堵塞”住了。我们现在要解决的问题就是如何“疏通”这条管道，使得从第一个栈Stack1进入的元素可以进入到第二个栈Stack2。

### 思路
当新元素到来时，我们可以将其压入Stack1。而当需要删除元素时，我们会非常直观地想到将Stack1中的元素依次弹出并压入Stack2，这时由于两个栈的摆放方向相反，压入Stack2的元素排列顺序并没有改变，相当于元素穿越了两个栈的头部。
因此，每当有新元素进来时我们都可以将其压入Stack1；当需要删除元素时，只需要删除返回Stack2尾部的第一个元素即可，如果Stack2为空就需要先将Stack1中元素全部压入Stack2。

### 举例
假设我们现在有这些操作：
1. 往队列中按顺序加入元素a、b、c；
2. 删除队列中最先进入的元素a；
3. 往队列中加入元素d。

![](/2022/03/03/coding-interview/coding-interview-9/example-steps.png)
如图，当新元素a、b、c到来时，我们将其压入Stack1；当需要删除队列头部的元素a时，由于Stack2为空，我们需要先将Stack1中的元素全部弹出并压入Stack2，再弹出Stack2最后一个元素a；当新元素d进来时，只需要将其直接压入Stack1即可；如果在此基础上再对队列进行删除操作，由于Stack2不为空，这时直接弹出Stack2最后一个元素即可。

### 原文代码(C/C++)
https://github.com/zhedahht/CodingInterviewChinese2/blob/master/09_QueueWithTwoStacks/QueueWithTwoStacks.cpp

### 个人编写代码(C#)
```
public class CQueue
{
    private Stack<int> stack1 = new Stack<int>();
    private Stack<int> stack2 = new Stack<int>();

    public void AppendTail(int value)
    {
        stack1.Push(value);
    }

    public int DeleteHead()
    {
        if (stack2.Count == 0)
        {
            while (stack1.Count > 0)
            {
                stack2.Push(stack1.Pop());
            }
        }

        if (stack2.Count > 0)
            return stack2.Pop();
        else
            return -1;
    }
}
```

## 题目二：用两个队列实现栈

|用两个队列实现一个栈，分别完成在栈的尾部插入节点（Push）、查看栈的尾部节点（Top）、在栈的尾部删除节点（Pop）和检查栈是否为空（Empty）的功能。</br>LeetCode力扣链接：[https://leetcode-cn.com/problems/implement-stack-using-queues/]|
|---|

由于队列先进先出的特点，元素在队列之间转移的时候并不会改变顺序，所以每当需要进行删除操作的时候，我们都需要将元素从一个队列转移到另一个队列，并保留最后一个元素进行删除弹出。

### 思路
和“用两个栈使用队列”类似，这道题也需要用到两个队列之间来回转移元素。当新元素进来时，我们将其压入任意一个队列Queue1；当需要删除元素的时候，我们将Queue1中的n-1个元素全部转移到Queue2中，弹出Queue1中最后一个元素并返回。

### 个人编写代码(C#)
```
public class CQueue
{
    private Queue<int> queue1 = new Queue<int>();
    private Queue<int> queue2 = new Queue<int>();

    public void Push(int x)
    {
        queue1.Enqueue(x);
    }

    public int Pop()
    {
        Top();

        return queue1.Dequeue();
    }

    public int Top()
    {
        if (queue1.Count == 0 && queue2.Count > 0)
        {
            Queue<int> temp = queue1;
            queue1 = queue2;
            queue2 = temp;
        }

        while (queue1.Count > 1)
        {
            queue2.Enqueue(queue1.Dequeue());
        }

        return queue1.Peek();
    }

    public bool Empty()
    {
        return queue1.Count == 0 && queue2.Count == 0;
    }
}
```