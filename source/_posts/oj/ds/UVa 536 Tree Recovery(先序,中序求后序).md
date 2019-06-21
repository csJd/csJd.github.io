---
title: UVa 536 Tree Recovery(先序,中序求后序)
author: Deng
tags: 
       - Algorithm

category: 
       - Data structure
       - OJ

date: 2015-01-23 20:25:47
---
题意 给你二叉树的先序序列和中序序列 求它的后序序列

先序序列的第一个一定是根 中序序列根左边的都属于根的左子树 右边的都属于右子树 递归建树就行了

```js 
#include <bits/stdc++.h>
using namespace std;
typedef struct TNode
{
    char data;
    TNode *lc, *rc;
} node, *BTree;

void build(BTree &t, string pre, string in)
{
    t = new node;
    t->lc = t->rc = NULL, t->data = pre[0];
    int k = in.find(pre[0]);
    if(k > 0) build(t->lc, pre.substr(1, k), in.substr(0, k));
    if(k < in.size() - 1) build(t->rc, pre.substr(k + 1), in.substr(k + 1));
}

void postTral(BTree &t)
{
    if(!t) return;
    postTral(t->lc);
    postTral(t->rc);
    putchar(t->data);
}

int main()
{
    string pre, in;
    while(cin >> pre >> in)
    {
        BTree t;
        build(t, pre, in);
        postTral(t);
        puts("");
    }
    return 0;
}
```

#

****

Little Valentine liked playing with binary trees very much. Her favorite game was constructing randomly looking binary trees with capital letters in the nodes.

This is an example of one of her creations:

D / \ / \ B E / \ \ / \ \ A C G / / F

To record her trees for future generations, she wrote down two strings for each tree: a preorder traversal (root, left subtree, right subtree) and an inorder traversal (left subtree, root, right subtree).

For the tree drawn above the preorder traversal is DBACEGF and the inorder traversal is ABCDEFG.

She thought that such a pair of strings would give enough information to reconstruct the tree later (but she never tried it).

Now, years later, looking again at the strings, she realized that reconstructing the trees was indeed possible, but only because she never had used the same letter twice in the same tree.

However, doing the reconstruction by hand, soon turned out to be tedious.

So now she asks you to write a program that does the job for her!

##

The input file will contain one or more test cases. Each test case consists of one line containing two strings preord and inord, representing the preorder traversal and inorder traversal of a binary tree. Both strings consist of unique capital letters. (Thus they are not longer than 26 characters.)

Input is terminated by end of file.

##

For each test case, recover Valentine's binary tree and print one line containing the tree's postorder traversal (left subtree, right subtree, root).

##

DBACEGF ABCDEFG BCAD CBAD

##

ACBFGED CDAB