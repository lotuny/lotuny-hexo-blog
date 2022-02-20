---
title: 剑指Offer面试题5：替换空格
date: 2022-02-20 15:07:00
categories: Coding Interview Edition 2
tags:
---



<!--more-->

|请实现一个函数，把字符串中的每个空格替换成"%20"。例如，输入"We are happy."，则输出"We%20are%20happy."。|
|---|

将空格替换成'%20'，就意味着用三个字符替换一个字符，替换完以后字符串会变长。如果输入数组的长度足够长，可以容纳替换空格后的字符串，那么替换可以在原数组中进行，否则需要创建一个新数组。

可以想象，新加入的字符会占用原来空格后面字符的位置，这样就需要我们将空格后面的字符都往后移两位。如果从左往右（从前往后）遍历空格，没遇到一个空格都如前面所说的来处理，当字符串中存在多个空格，那么有一部分字符就需要移动多次。

如果我们从右往左（从后往前）呢？

### 思路
先遍历一遍数组找到空格的数量，并计算出新字符串所需数组长度，这等于原字符串长度加上二倍空格数。我们在这里讨论原数组足够长的情况，以便和原文代码保持一致。这里我们先创建两个指针，p1指向旧字符串最后一位，p2指向新字符串最后一位。然后p1从后往前遍历旧字符串，遇到非空格字符则拷贝到p2指向的位置；遇到空格字符，则在p2指向的位置连续赋值'0'，'2'和'%';遍历直到p1和p2两个指针相遇为止。

### 举例
以为"We are happy."为例，这个字符串的长度为13，其中包含2个空格，则新字符串的长度应该为13+2*2=17。如下图所示，

{% asset_img replace-space.jpg %}

### 原文代码(C/C++)
https://github.com/zhedahht/CodingInterviewChinese2/blob/master/05_ReplaceSpaces/ReplaceSpaces.cpp

### 个人编写代码(C#)
```
public string ReplaceSpace(string s)
{
    if (s == null || s.Length == 0)
        return s;

    int len = s.Length;
    char[] str1 = s.ToCharArray();
    foreach(char c in str1)
    {
        if (c.Equals(' '))
            len += 2;
    }
    
    char[] str2 = new char[len];

    int p1 = str1.Length - 1;
    int p2 = len - 1;
    while(p1 >= 0)
    {
        if (str1[p1].Equals(' '))
        {
            str2[p2--] = '0';
            str2[p2--] = '2';
            str2[p2--] = '%';
        }
        else
        {
            str2[p2--] = str1[p1];
        }

        --p1;
    }

    return new string(str2);
}
```
