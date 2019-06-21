---
title: UVa 122 Trees on the level(建树，层次遍历)
author: Deng
tags: 
       - Algorithm

category: 
       - Data structure
       - OJ

date: 2014-09-23 10:26:45
---
题意 建树并层次遍历输出 (data,pos) pos表示改节点位置 L代表左儿子 R代表右儿子

建树很简单 开始在根节点 遇到L往左走遇到R往右走 节点不存在就新建 走完了就保存改节点的值 输出直接bfs就行了了

```js 
#include<cstdio>
#include<cctype>
#include<cstring>
using namespace std;
const int maxn = 300;

char s[maxn][maxn];
int t, n, lans;
bool comp;

struct Node
{
    int data;
    Node* left, *right;
    bool have;
    Node(): left(NULL), right(NULL), have(false) {};
};

Node *ans[maxn];

bool bfs(Node* root)
{
    int fro = 0, bac = 0;
    ans[fro] = root;
    while(fro <= bac)
    {
        if(!ans[fro]->have) return false;
        if(ans[fro]->left != NULL) ans[++bac] = ans[fro]->left;
        if(ans[fro]->right != NULL) ans[++bac] = ans[fro]->right;
        ++fro;
    }
    lans = bac;
    return true;
}

int main()
{
    while(~scanf("%s", s[0]))
    {
        n = 0;
        Node root, *cur = &root;
        comp = true;
        do
        {
            t = 0;
            int l = strlen(s[n]);
            for(int i = 1; i < l; ++i)
            {
                if(isdigit(s[n][i]))
                {
                    t = t * 10 + s[n][i] - '0';
                }
                else if(s[n][i] == 'L')
                {
                    if(cur->left == NULL)
                        cur->left = new Node;
                    cur = cur->left;
                }
                else if(s[n][i] == 'R')
                {
                    if(cur->right == NULL)
                        cur->right = new Node;
                    cur = cur->right;
                }
            }
            if(cur->have) comp = false;
            else
            {
                cur->data = t;
                cur->have = true;
            }
            cur = &root;
        }
        while(scanf("%s", s[++n]), s[n][1] != ')');

        if(comp) comp = bfs(&root);
        if(comp)
        {
            for(int i = 0; i < lans; ++i)
                printf("%d ", ans[i]->data);
            printf("%d\n", ans[lans]->data);
        }
        else printf("not complete\n");
    }
    return 0;
}
```

#

****

## Background

Trees are fundamental in many branches of computer science. Current state-of-the art parallel computers such as Thinking Machines' CM-5 are based on *fat trees*. Quad- and octal-trees are fundamental to many algorithms in computer graphics.

This problem involves building and traversing binary trees.

## The Problem

Given a sequence of binary trees, you are to write a program that prints a level-order traversal of each tree. In this problem each node of a binary tree contains a positive integer and all binary trees have have fewer than 256 nodes.

In a *level-order* traversal of a tree, the data in all nodes at a given level are printed in left-to-right order and all nodes at level *k* are printed before all nodes at level *k*+1.

For example, a level order traversal of the tree

![picture28](../images/dge.org-external-1-122img1.gif.png)

is: 5, 4, 8, 11, 13, 4, 7, 2, 1.

In this problem a binary tree is specified by a sequence of pairs (*n*,*s*) where *n* is the value at the node whose path from the root is given by the string *s*. A path is given be a sequence of *L*'s and *R*'s where *L* indicates a left branch and *R* indicates a right branch. In the tree diagrammed above, the node containing 13 is specified by (13,RL), and the node containing 2 is specified by (2,LLR). The root node is specified by (5,) where the empty string indicates the path from the root to itself. A binary tree is considered to be *completely specified* if every node on all root-to-node paths in the tree is given a value exactly once.

## The Input

The input is a sequence of binary trees specified as described above. Each tree in a sequence consists of several pairs (*n*,*s*) as described above separated by whitespace. The last entry in each tree is (). No whitespace appears between left and right parentheses.

All nodes contain a positive integer. Every tree in the input will consist of at least one node and no more than 256 nodes. Input is terminated by end-of-file.

## The Output

For each completely specified binary tree in the input file, the level order traversal of that tree should be printed. If a tree is not completely specified, i.e., some node in the tree is NOT given a value or a node is given a value more than once, then the string ``not complete'' should be printed.

## Sample Input

(11,LL) (7,LLL) (8,R) (5,) (4,L) (13,RL) (2,LLR) (1,RRR) (4,RR) () (3,L) (4,R) ()

## Sample Output

5 4 8 11 13 4 7 2 1 not complete