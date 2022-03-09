---
title: 剑指Offer面试题10：斐波那契数列
date: 2022-03-06
categories: Coding Interview Edition 2
tags:
---

斐波那契数列

<!--more-->

## 题目一：求斐波那契数列的第n项
|写一个函数，输入n，求斐波那契数列（Fibonacci）的第n项。斐波那契数列的定义如下：f(0)=0，fn(1)=1，f(n)=f(n-1)+f(n-2)。</br>LeetCode力扣链接（注意：该题要求结果取模）：[https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/]|
|---|

### 思路


### 举例


### 原文代码(C/C++)
https://github.com/zhedahht/CodingInterviewChinese2/blob/master/10_Fibonacci/Fibonacci.cpp

### 个人编写代码(C#)
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
1. 说到储存中间结果，自然就会想到数组，然而这里其实只需要用到两个变量。

## 题目二：青蛙跳台阶问题
|一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个n级的台阶总共有多少种跳法。|
|---|

### 思路


### 举例


### 个人编写代码(C#)
