---
title: 剑指Offer面试题7：重建二叉树
date: 2022-03-02
categories: Coding Interview Edition 2
tags: Binary Tree（二叉树）
---

**树**是一种常见数据结构，它的逻辑很简单：除根节点外的每个节点只有一个父节点，根节点没有父节点；除叶节点外所有所有节点都有一个或多个子节点，叶节点没有子节点。**二叉树**是树的一种特殊结构，其每个节点最多只能有两个子节点。

<!--more-->

|输入某二叉树的前序遍历和中序遍历的结果，请重建该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如，输入前序遍历序列{1, 2, 4, 7, 3, 5, 6, 8}和中序遍历序列{4, 7, 2, 1, 5, 3, 8, 6}，则重建如下图所示的二叉树并输出它的头节点。</br>LeetCode力扣链接：[https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/]|
|---|

```
// 二叉树节点的定义如下：
struct BinaryTreeNode
{
    int                    m_nValue;
    BinaryTreeNode*        m_pLeft;
    BinaryTreeNode*        m_pRight;
};
```

思考一下两种遍历方式的特点。前序遍历序列中的第一个数字为根节点的值，且紧跟其后的值必然属于它的子节点，同时右子树节点必然只会出现在左子树节点后面；而中序遍历中的根节点的值在序列中间，且左子树节点的值位于根节点的值的左边，右子树节点的值则位于根节点的值的右边。

### 思路
那么，我们可以通过前序遍历序列确定根节点，然后在中序遍历序列中用根节点找到左右子树，再依据找到的左右子树节点数量在前序遍历序列中找到对应的左右子树的前序遍历序列；这时我们会发现两个序列都被一分为二了，并且一一对应。于是我们可以通过不断重建左右子树并返回其根节点，最终重建出一个原始的树。

**注意**：二叉树中不含重复数字是这道题可解的必要条件。

### 举例
以题目为例，我们有前序遍历序列{1, 2, 4, 7, 3, 5, 6, 8}和中序遍历序列{4, 7, 2, 1, 5, 3, 8, 6}。

首先我们从前序遍历序列中找到根节点的值1，然后在中序遍历序列中确定了{4, 7, 2}为左子树节点的中序遍历以及{5, 3, 8, 6}为右子树节点的中序遍历；已知左子树有3个节点，现在回头看前序遍历序列找到根节点1的后3位{2, 4, 7}确定为左子树节点前序遍历序列，紧接着的4位{3, 5, 6, 8}为右子树节点前序遍历序列。现在我们就有了树的根节点以及左右子树的前序遍历序列和中序遍历序列：
根节点：{1}
左子树：前序{2, 4, 7} 中序{4, 7, 2}
右子树：前序{3, 5, 6, 8} 中序{5, 3, 8, 6}

如图所示，我们可以迭代获得所有左右子树的根节点及前中序遍历序列，最终重构出原始的树。
![](/2022/03/02/coding-interview/coding-interview-7/rebuild-tree-example.jpg)

### 原文代码(C/C++)
https://github.com/zhedahht/CodingInterviewChinese2/blob/master/07_ConstructBinaryTree/ConstructBinaryTree.cpp

### 个人编写代码(C#)
```
public class BuildTreeSolution
{
    private int[] preorder;
    private int[] inorder;

    public TreeNode BuildTree(int[] preorder, int[] inorder)
    {
        if (preorder == null || inorder == null || preorder.Length != inorder.Length || inorder.Length == 0)
            return null;

        this.preorder = preorder;
        this.inorder = inorder;
        return BuildTree(0, preorder.Length-1, 0, inorder.Length-1);
    }

    public TreeNode BuildTree(int prestart, int preend, int instart, int inend)
    {
        TreeNode root = new TreeNode(preorder[prestart]);

        // 当前序遍历序列只剩下一个节点
        if (prestart == preend)
        {
            // 当中序遍历序列也只剩下一个节点；且前中序遍历序列中的唯一节点数值相等
            if (instart == inend && preorder[prestart] == inorder[instart])
                return root;
            else
                throw new InvalidOperationException();
        }

        int rootInorder = -1;
        for (int i = instart; i <= inend; ++i)
        {
            if (root.val == inorder[i])
                rootInorder = i;
        }

        // 当在中序遍历序列找不到对应的数字
        if (rootInorder == -1) 
            throw new InvalidOperationException();

        int leftCount = rootInorder - instart;

        if (rootInorder > instart)
            root.left = BuildTree(prestart + 1, prestart + leftCount, instart, rootInorder-1);

        if (rootInorder < inend)
            root.right = BuildTree(prestart + leftCount + 1, preend, rootInorder + 1, inend);

        return root;
    }

    // Definition for a binary tree node.
    public class TreeNode {
        public int val;
        public TreeNode left;
        public TreeNode right;
        public TreeNode(int x) { val = x; }
    }
}
```

### 易忽略点
1. 本题的非法输入情况比较复杂，主要可以分为三种，我们尤其需要注意处理异常：
    - 输入前中序遍历序列为空或者长度为0；
    - 前中序遍历序列长度或数字不一致；
    - 前中序遍历序列中数字的排序存在矛盾。
