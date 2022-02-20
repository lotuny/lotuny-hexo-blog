---
title: 剑指Offer面试题4：二维数组中的查找
date: 2022-02-20 02:35:19
categories: Coding Interview Edition 2
tags: Array
---

在C#语言里面，有两种表达**二维数组**的方式，一种称为交错数组（Jagged Array），另一种称为多维数组（Multi-Dimensional Array）。其中，交错数组可以理解为一维数组，其元素也为数组，各元素之间大小可能不同；多维数组具有多个维度，每一维度的各元素都具有相同大小。

<!--more-->

|在一个二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序牌组。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数，如果存在则返回true，否则返回false。</br>LeetCode力扣链接：[https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/]|
|---|

如下面的二维数组，每行每列都是递增排序。
1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;9
2&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;9&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;12
4&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;7&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;10&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;13
6&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;11&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;15

<!-- |     |     |     |     |     |     |     |     |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|  1  |  2  |   8 |   9 |     |     |     |     |
|  2  |  4  |   9 |  12 |     |     |     |     |
|  4  |  7  |  10 |  13 |     |     |     |     |
|  6  |  8  |  11 |  15 |     |     |     |     | -->

解题的关键是这里的递增排序。假设目标数字和二维数组中的被中的数字不相同时，如果前者较大，那么它有可能出现在这一元素的右边或者下面，否则将可能出现在左边或上面。如果我们随机挑选二维数组中的数字进行比较，那么就使得可能出现目标数字的待搜索区域重叠，增加了搜索的复杂程度。

那么有没有这么一个二维数组中的位置，使得其元素与目标数字进行比较时，不管前后者哪个比较大，待搜索区域只有一块？

### 思路
我们发现，二维数组左下角的位置没有左边和下面，而右上角的位置没有右边和上面，正好满足我们的需求。拿右上角举例，当目标数字较大时，它只可能出现在下面，较小时只可能出现在左边。这就意味着每次比较都会筛掉一行或者一列。循环比较直到要超越边界，就能确定数组中是否存在目标数字。

### 举例
以题目中的二维数组为例子，从左往右为第一到第四列，从上到下为第一到第四行。假设我们要找的目标数字为5（不存在），从右上角的9开始比较起，发现5比9小，那么5绝对不会出现在9所在的那一列（第四列）；我们继续跟9左边的8进行比较，发现5还是比8小，排除第三列；和8左边的2比较，发现5比较大，那么2所在的第一行也可以被排除了；和2下面的4比较，发现5更大，排除第二行；再和4下面的7比较，发现5较小，排除第二列；和7左边的4比较，发现5更大，排除第三行；和4下面的6比较，发现5更小；由于6的左边已经没有元素了，这时候我们可以跳出循环认为该二维数组中不存在数字5。

### 原文代码(C/C++)
https://github.com/zhedahht/CodingInterviewChinese2/blob/master/04_FindInPartiallySortedMatrix/FindInPartiallySortedMatrix.cpp

### 个人编写代码(C#)
这里的代码输入与原文有所不同，主要是为了和力扣题目保持一致，所以输入为二维数组和目标数字。
```
public bool FindNumberIn2DArray(int[][] matrix, int target)
{
    if (matrix == null || matrix.Length == 0 || matrix[0].Length == 0)
        return false;
    int row = 0;
    int col = matrix[0].Length - 1;
    while(row < matrix.Length && col >= 0)
    {
        if (target > matrix[row][col])
            ++row;
        else if (target < matrix[row][col])
            --col;
        else
            return true;
    }
    return false;
}
```
