---
title: 剑指Offer面试题8：二叉树的下一个节点
date: 2022-03-03
categories: Coding Interview Edition 2
tags: Binary Tree（二叉树）
---

二叉树中序遍历：先遍历左子树，再遍历根节点，最后遍历右子树。

<!--more-->

|给定一棵二叉树和其中一个子节点，如何找出中序遍历的下一个节点？树中的节点除了有两个分别指向左、右子节点的指针，还有一个指向父节点的指针。|
|---|

```
// 二叉树节点的定义如下：
struct BinaryTreeNode
{
    int                    m_nValue;
    BinaryTreeNode*        m_pLeft;
    BinaryTreeNode*        m_pRight;
    BinaryTreeNode*        m_pParent;
};
```

### 思路
![](/2022/03/03/coding-interview/coding-interview-8/binary-tree-example.png)
我们根据上图给出的例子可以先来分析一下给定的节点的情形：
1. 节点拥有右子节点（节点b）：
    该节点的下一个中序遍历节点为右子树的最左节点（节点h）。
2. 节点没有右子节点：
    1. 节点是父节点的左子节点（节点d）：
    该节点的下一个中序遍历节点为父节点（节点b）。
    2. 节点是父节点的右子节点（节点i）:
    沿着该节点的父节点指针向上遍历，直到遇到第一个其父节点的左子节点是其本身的节点，那么这个节点的父节点就是下一个中序遍历节点（节点a）。

### 原文代码(C/C++)
https://github.com/zhedahht/CodingInterviewChinese2/blob/master/08_NextNodeInBinaryTrees/NextNodeInBinaryTrees.cpp

### 个人编写代码(C#)
```
public class NextInorderNodeSolution
{
    public TreeNode FindNextInorderNode(TreeNode current)
    {
        if (current == null)
            return null;

        if (current.right != null)
        {
            return current.right;
        }
        else if (current.parent != null)
        {
            TreeNode child = current;
            TreeNode parent = current.parent;
            while (parent != null && parent.left != child)
            {
                child = child.parent;
                parent = child.parent;
            }

            return child.parent;
        }
            
        return null;
    }

    // Definition for a binary tree node with a pointer to its parent .
    public class TreeNode {
        public int val;
        public TreeNode left;
        public TreeNode right;
        public TreeNode parent;
        public TreeNode(int x) { val = x; }
    }
}
```

### 易忽略点
1. 在例子分析中提到的情形二，其下两种情形在写代码时可以一起进行判断，因为它们本质上都是寻找第一个左子节点为遍历中节点的节点。情形2.1可以看作是2.2的特例，看作是2.2遍历到最后一个节点的情况。