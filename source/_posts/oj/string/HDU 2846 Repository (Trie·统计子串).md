---
title: HDU 2846 Repository (Trie·统计子串)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - String

date: 2015-07-28 09:06:04
---
题意 给你p个商品名称 然后输入q个字符串查询 对每个查询输出含有查询串为子串的商品个数

Trie能很快的求出字典中以某个串为前缀的串的个数 但现在要查的是以某个串为子串的串的个数 可以发现

一个串的任何子串肯定是这个串某个后缀的前缀如"ri"是“Trie" 的子串 是后缀 "rie" 的前缀

那么我们在向Trie中插入时可以把这个串的所有后缀都插入 插入时要注意来自同一个串的后缀的相同前缀只能统计一次 如 "abab" 这个串 "ab" 既是后缀 "abab" 的前缀 也是后缀 "ab" 的前缀 但是只能统计一次 这用一个id数组标记就行了

这样最后Trie中以查询串为前缀的串的个数就是原始串中以查询串为子串的串的的个数了

```js 
#include <cstdio>
#include <cstring>
using namespace std;
const int N = 1e6 + 5;
int trie[N][26], cnt[N], id[N], L;

void insertTrie(char s[], int iid)
{
    int r = 0, i = 0, j;
    while(s[i])
    {
        j = s[i++] - 'a';
        if(!trie[r][j])
            trie[r][j] = L++;
        r = trie[r][j];
        if(id[r] != iid) ++cnt[r];
        id[r] = iid;//标记当前前缀id 保证来自相同串的只自增一次
    }
}

int searchTrie(char s[])
{
    int r = 0, i = 0, j;
    while(s[i])
    {
        j = s[i++] - 'a';
        if(!trie[r][j])
            return 0;
        r = trie[r][j];
    }
    return cnt[r];
}

int main()
{
    L = 1;//初始化Trie
    char s[30];
    int p, q;
    scanf("%d", &p);
    for(int i = 1; i <= p; ++i)
    {
        scanf("%s", s); //插入s的所有后缀
        for(int j = 0; s[j]; ++j)
            insertTrie(s + j, i);
    }

    scanf("%d", &q);
    while(q--)
    {
        scanf("%s", s);
        printf("%d\n", searchTrie(s));
    }

    return 0;
}
//Last modified :   2015-07-28 08:33
```

# Repository

Problem Description

When you go shopping, you can search in repository for avalible merchandises by the computers and internet. First you give the search system a name about something, then the system responds with the results. Now you are given a lot merchandise names in repository and some queries, and required to simulate the process.
Input

There is only one case. First there is an integer P (1<=P<=10000)representing the number of the merchanidse names in the repository. The next P lines each contain a string (it's length isn't beyond 20,and all the letters are lowercase).Then there is an integer Q(1<=Q<=100000) representing the number of the queries. The next Q lines each contains a string(the same limitation as foregoing descriptions) as the searching condition.
Output

For each query, you just output the number of the merchandises, whose names contain the search string as their substrings.
Sample Input

20 ad ae af ag ah ai aj ak al ads add ade adf adg adh adi adj adk adl aes 5 b a d ad s
Sample Output

0 20 11 11 2