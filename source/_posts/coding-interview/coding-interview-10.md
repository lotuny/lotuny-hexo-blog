---
title: 剑指Offer面试题10：斐波那契数列
date: 2022-03-06
categories: Coding Interview Edition 2
tags:
---

**递归**和**循环**：如果我们需要重复多次地计算相同的问题，则通常可以选择用递归或者循环两种不同的方法。递归是在一个函数内部调用这个函数本身；而循环是通过设置计算的初始值和终止条件，在一个范围内重复计算。

<!--more-->

## 题目一：求斐波那契数列的第n项
|写一个函数，输入n，求斐波那契数列（Fibonacci）的第n项。斐波那契数列的定义如下：f(0)=0，f(1)=1，f(n)=f(n-1)+f(n-2)。</br>LeetCode力扣链接（注意：该题要求结果取模）：[https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/]|
|---|

从定义中就能看出，斐波那契数列非常适合用递归来解题，也通常用于教科书上来讲解递归函数。但是，由于f(n-1)=f(n-2)+f(n-3)，这意味着用递归在计算f(n)的时候会重复计算f(n-2)，当n非常大的时候，重复计算量将呈指数增长，递归解法的效率十分低下。

是否可以将计算结果存起来，以避免重复计算？

### 思路
从下至上计算，我们可以将f(i)+f(i+1)=f(i+2)中的f(i)和f(i+1)都存起来，等到计算出了f(i+2)，再把f(i)舍弃掉，继续用f(i+1)+f(i+2)来计算f(i+3)。

### 举例
假设n=5，f(0)=0，f(1)=1，那么f(2)=f(0)+f(1)=1，f(3)=f(1)+f(2)=2，f(4)=f(2)+f(3)=3，f(5)=f(3)+f(4)=5。我们可以看到，每次计算只需要存储两个数字即可算出f(n)。

### 原文代码(C/C++)
https://github.com/zhedahht/CodingInterviewChinese2/blob/master/10_Fibonacci/Fibonacci.cpp

### 个人编写代码(C#)
（这里结果没有取模，所以不能直接在力扣上跑通）
```
public class FibonacciSolution
{
    public int Fib(int n)
    {
        if (n < 0)
        {
            return -1;
        }
        else if (n < 2)
        {
            return n;
        }

        var minusOne = 1;
        var minusTwo = 0;
        var result = 0;
        for (int i = 2; i <= n; ++i)
        {
            result = minusOne + minusTwo;
            minusTwo = minusOne;
            minusOne = result;
        }

        return result;
    }
}
```

### 易忽视点
1. 说到存储中间结果，自然就会想到数组，然而这里其实只需要用到两个变量。

## 题目二：青蛙跳台阶问题
|一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个n级的台阶总共有多少种跳法。</br>LeetCode力扣链接（注意：该题要求结果取模）：[https://leetcode-cn.com/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/]|
|---|

由于青蛙一次可以有两种跳法，那么我们可以针对这两种跳法分别分析和计算。假设f(n)为青蛙跳上一个n级台阶的跳法数，那么如果青蛙第一次跳上1级台阶，那么它将会有f(n-1)种跳法；如果青蛙第一次跳上2级台阶，那么它将有f(n-2)种跳法。将这两种情况算在一起，我们会有f(n)=f(n-1)+f(n-2)，可以看出这完全就是斐波那契数列。

注意：f(0)=1，f(1)=1

### 个人编写代码(C#)
（这里结果没有取模，所以不能直接在力扣上跑通）
```
    public class FibonacciSolution
    {
        public int FrogJump(int n)
        {
            if (n == 0)
            {
                return 1;
            }
            else if (n < 3)
            {
                return n;
            }

            var minusOne = 2;
            var minusTwo = 1;
            var result = 0;
            for (int i = 3; i <= n; ++i)
            {
                result = minusOne + minusTwo;
                minusTwo = minusOne;
                minusOne = result;
            }

            return result;
        }
    }
```