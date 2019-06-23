---
title: UVa 699 The Falling Leaves(递归建树)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Data structure

date: 2014-09-24 00:48:14
---
题意 假设一棵二叉树也会落叶 而且叶子只会垂直下落 每个节点保存的值为那个节点上的叶子数 求所有叶子全部下落后 地面从左到右每堆有多少片叶子

和上一题有点像 都是递归输入的 一个节点(设水平位置为p) 则它的左右儿子节点的水平位置分别为 p-1 p+1 也是可以边输入边处理的 输入完也就得到答案了 注意每个样例后面都有一个空行 包括最后一个

```js 
#include<cstdio>
#include<cstring>
using namespace std;
int le, ri;
const int M = 205;
int a[M];

void build(int p)
{
    int t;
    scanf("%d", &t);
    if(t == -1) return;
    else
    {
        a[p] += t;
        build(p - 1);
        build(p + 1);
        le = le < p ? le : p;
        ri = ri > p ? ri : p;
    }
}

int main()
{
    int cas = 0;
    le = M, ri = 0;
    build(M / 2);

    while(le <= ri)
    {
        printf("Case %d:\n", ++cas);
        for(int i = le; i < ri; ++i)
            printf("%d ", a[i]);
        printf("%d\n\n", a[ri]);

        memset(a, 0, sizeof(a));
        le = M, ri = 0;
        build(M / 2);
    }
    return 0;
}
```

#

****

Each year, fall in the North Central region is accompanied by the brilliant colors of the leaves on the trees, followed quickly by the falling leaves accumulating under the trees. If the same thing happened to binary trees, how large would the piles of leaves become?

We assume each node in a binary tree "drops" a number of leaves equal to the integer value stored in that node. We also assume that these leaves drop vertically to the ground (thankfully, there's no wind to blow them around). Finally, we assume that the nodes are positioned horizontally in such a manner that the left and right children of a node are exactly one unit to the left and one unit to the right, respectively, of their parent. Consider the following tree:

![](../images/dge.org-external-6-p699.gif.png)

The nodes containing 5 and 6 have the same horizontal position (with different vertical positions, of course). The node containing 7 is one unit to the left of those containing 5 and 6, and the node containing 3 is one unit to their right. When the "leaves" drop from these nodes, three piles are created: the leftmost one contains 7 leaves (from the leftmost node), the next contains 11 (from the nodes containing 5 and 6), and the rightmost pile contains 3. (While it is true that only leaf nodes in a tree would logically have leaves, we ignore that in this problem.)

##

The input contains multiple test cases, each describing a single tree. A tree is specified by giving the value in the root node, followed by the description of the left subtree, and then the description of the right subtree. If a subtree is empty, the value -1 is supplied. Thus the tree shown above is specified as 5 7 -1 6 -1 -1 3 -1 -1 . Each actual tree node contains a positive, non-zero value. The last test case is followed by a single -1 (which would otherwise represent an empty tree).

##

For each test case, display the case number (they are numbered sequentially, starting with 1) on a line by itself. On the next line display the number of "leaves" in each pile, from left to right, with a single space separating each value. This display must start in column 1, and will not exceed the width of an 80-character line. Follow the output for each case by a blank line. This format is illustrated in the examples below.

##

5 7 -1 6 -1 -1 3 -1 -1 8 2 9 -1 -1 6 5 -1 -1 12 -1 -1 3 7 -1 -1 -1 -1

##

Case 1: 7 11 3 Case 2: 9 7 21 15