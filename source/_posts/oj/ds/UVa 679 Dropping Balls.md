---
title: UVa 679 Dropping Balls
author: Deng
tags: 
       - Algorithm

category: 
       - Data structure
       - OJ

date: 2014-09-02 09:50:57
---
#

****

A number of *K* balls are dropped one by one from the root of a fully binary tree structure FBT. Each time the ball being dropped first visits a non-terminal node. It then keeps moving down, either follows the path of the left subtree, or follows the path of the right subtree, until it stops at one of the leaf nodes of FBT. To determine a ball's moving direction a flag is set up in every non-terminal node with two values, either **false** or **true**. Initially, all of the flags are **false**. When visiting a non-terminal node if the flag's current value at this node is**false**, then the ball will first switch this flag's value, i.e., from the **false** to the **true**, and then follow the left subtree of this node to keep moving down. Otherwise, it will also switch this flag's value, i.e., from the **true** to the **false**, but will follow the right subtree of this node to keep moving down. Furthermore, all nodes of FBT are sequentially numbered, starting at 1 with nodes on depth 1, and then those on depth 2, and so on. Nodes on any depth are numbered from left to right.

For example, Fig. 1 represents a fully binary tree of maximum depth 4 with the node numbers 1, 2, 3, ..., 15. Since all of the flags are initially set to be **false**, the first ball being dropped will switch flag's values at node 1, node 2, and node 4 before it finally stops at position 8. The second ball being dropped will switch flag's values at node 1, node 3, and node 6, and stop at position 12. Obviously, the third ball being dropped will switch flag's values at node 1, node 2, and node 5 before it stops at position 10.

![](../images/dge.org-external-6-p679.gif.png)

Fig. 1: An example of FBT with the maximum depth 4 and sequential node numbers.

Now consider a number of test cases where two values will be given for each test. The first value is *D*, the maximum depth of FBT, and the second one is *I*, the *I**th* ball being dropped. You may assume the value of *I* will not exceed the total number of leaf nodes for the given FBT.

Please write a program to determine the stop position *P* for each test case.

For each test cases the range of two parameters *D* and *I* is as below:

![\begin{displaymath}2 \le D \le 20, \mbox{ and } 1 \le I \le 524288.\end{displaymath}](../images/dge.org-external-6-679img1.gif.png)

##

Contains l +2 lines. Line 1 *I* the number of test cases Line 2 test case /#1, two decimal numbers that are separatedby one blank ... Line *k*+1 test case /#*k* Line *l*+1 test case /#*l* Line *l*+2 -1 a constant -1 representing the end of the input file

##

Contains l lines. Line 1 the stop position *P* for the test case /#1 ... Line *k* the stop position *P* for the test case /#*k* ... Line *l* the stop position *P* for the test case /#*l*

##

5 4 2 3 4 10 1 2 2 8 128 -1

##

12 7 512 3 255

题意 i个小球在一棵二叉树上下落 第奇数个到达某个节点就往左 否则往右 求最后一个小球下落到最底层所在结点的序号

对于第i个小球 在根结点处 若i是奇数 则它是往左的第(i+1)/2个小球 若i是偶数 则它是往右的第n/2个小球

而对于一个结点k 它的左子结点序号为2/*k 右子节点序号为2/*k+1 每次更新i到到最底层就得出结果了

```js 
#include<cstdio>
int main()
{
    int cas, ans, d, i;
    while (scanf ("%d", &cas), cas + 1)
    {
        while (cas--)
        {
            ans = 1;
            scanf ("%d%d", &d, &i);
            while (--d)
            {
                if (i % 2)
                    ans = ans * 2, i = (i + 1) / 2;
                else
                    ans = ans * 2 + 1, i = i / 2;
            }
            printf ("%d\n", ans);
        }
    }
    return 0;
}
```