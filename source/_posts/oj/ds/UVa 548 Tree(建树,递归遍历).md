---
title: UVa 548 Tree(建树,递归遍历)
author: Deng
tags: 
       - Algorithm

category: 
       - Data structure
       - OJ

date: 2014-09-23 20:22:28
---
题意 给你一个树的中序遍历和后序遍历 某个节点的权值为从根节点到该节点所经过节点的和 求权值最小的叶节点的值 如果存在多个 输出值最小的那个

把树建好就好说了 递归递归dfs msun保存最小叶节点权值 ans保存答案

```js 
#include<cstdio>
#include<cctype>
#include<cstring>
using namespace std;
const int maxn = 10005, INF = 0x3f3f3f3f;
int in[maxn], post[maxn], msum, ans;

struct node
{
    int val;
    node *left, *right;
    node(): left(NULL), right(NULL) {}
};

node* build(int le, int ri, int pos)
{
    node *cur = new node;
    cur->val = post[pos];
    if(le == ri) return cur;
    int i = le;
    while(in[i] != post[pos]) ++i;
    if(i == le) cur->left = NULL;
    else  cur->left = build(le, i - 1, pos - 1 - (ri - i));
    if(i == ri) cur->right = NULL;
    else cur->right = build(i + 1, ri, pos - 1);
    return cur;
}

void dfs(node *cur, int sum)
{
    int t = cur->val;
    if(!(cur->left || cur->right))
    {
        if(sum + t < msum || (sum + t == msum && t < ans))
            msum = sum + t, ans = t;
    }
    if(cur->left != NULL) dfs(cur->left, sum + t);
    if(cur->right != NULL) dfs(cur->right, sum + t);

}

int main()
{
    char s[maxn*10];
    while(gets(s) != NULL)
    {
        int t = 0, l = 0;
        for(int i = 0; s[i] != '\0'; ++i)
            if(isdigit(s[i])) t = t * 10 + s[i] - '0';
            else  in[++l] = t, t = 0;
        in[++l] = t;

        gets(s);
        l = t = 0;
        for(int i = 0; s[i] != '\0'; ++i)
            if(isdigit(s[i])) t = t * 10 + s[i] - '0';
            else post[++l] = t, t = 0;
        post[++l] = t;

        node *root = build(1, l, l);
        ans = msum = INF;
        dfs(root, 0);
        printf("%d\n", ans);
    }
    return 0;
}
```

#

****

You are to determine the value of the leaf node in a given binary tree that is the terminal node of a path of least value from the root of the binary tree to any leaf. The value of a path is the sum of values of nodes along that path.

##

The input file will contain a description of the binary tree given as the inorder and postorder traversal sequences of that tree. Your program will read two line (until end of file) from the input file. The first line will contain the sequence of values associated with an inorder traversal of the tree and the second line will contain the sequence of values associated with a postorder traversal of the tree. All values will be different, greater than zero and less than 10000. You may assume that no binary tree will have more than 10000 nodes or less than 1 node.

##

For each tree description you should output the value of the leaf node of a path of least value. In the case of multiple paths of least value you should pick the one with the least value on the terminal node.

##

3 2 1 4 5 7 6 3 1 2 5 6 7 4 7 8 11 3 5 16 12 18 8 3 11 7 16 18 12 5 255 255

##

1 3 255

#

****

You are to determine the value of the leaf node in a given binary tree that is the terminal node of a path of least value from the root of the binary tree to any leaf. The value of a path is the sum of values of nodes along that path.

##

The input file will contain a description of the binary tree given as the inorder and postorder traversal sequences of that tree. Your program will read two line (until end of file) from the input file. The first line will contain the sequence of values associated with an inorder traversal of the tree and the second line will contain the sequence of values associated with a postorder traversal of the tree. All values will be different, greater than zero and less than 10000. You may assume that no binary tree will have more than 10000 nodes or less than 1 node.

##

For each tree description you should output the value of the leaf node of a path of least value. In the case of multiple paths of least value you should pick the one with the least value on the terminal node.

##

3 2 1 4 5 7 6 3 1 2 5 6 7 4 7 8 11 3 5 16 12 18 8 3 11 7 16 18 12 5 255 255

##

1 3 255