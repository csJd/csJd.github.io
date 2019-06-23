---
title: HDU 1247 Hat’s Words (Trie·指针实现)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - String

date: 2015-07-27 16:43:11
---
题意 给你一个字典 输出字典中能表示成两个单词连接的所有单词

最基础的字典树应用 先把所有单词加入字典树中 标记每个结点是否为某个单词的结尾 然后查找每个单词 在树上查询过程中遇到单词结尾时 如果剩下的后缀也是一个单词 那当前查询的单词就可以是两个单词的连接了

```js 
#include <cstdio>
#include <cstring>
using namespace std;
const int N = 50005;
int n;
char ss[N][20];

struct Trie
{
    Trie *chi[26];
    bool isEnd;
    Trie()
    {
        isEnd = false;
        memset(chi, NULL, sizeof(chi));
    }
}*root;

void insertTrie(Trie *r, char s[])
{
    int i = 0, j;
    while(s[i])
    {
        j = s[i++] - 'a';
        if(r->chi[j] == NULL)
            r->chi[j] = new Trie();
        r = r->chi[j];
    }
    r->isEnd = true;
}

//op标记这次查询是查询后缀还是查询整个
bool searchTrie(Trie *r, char s[], int op)
{
    int i = 0, j;
    while(s[i])
    {
        j = s[i++] - 'a';
        if(r->chi[j] == NULL)
            return false;
        r = r->chi[j];
        if(!op && r->isEnd && s[i])//前缀是一个单词
        {
            if(searchTrie(root, s + i, 1))
                return true; //查询后缀是否是一个单词
        }
    }
    return op ? r->isEnd : op;
}

int main()
{
    int m = 0;
    root = new Trie();
    while(~scanf("%s", ss[m]))
        insertTrie(root, ss[m++]);

    for(int i = 0; i < m; ++i)
        if(searchTrie(root, ss[i], 0)) puts(ss[i]);

    return 0;
}
```

# Hat’s Words

Problem Description

A hat’s word is a word in the dictionary that is the concatenation of exactly two other words in the dictionary.
You are to find all the hat’s words in a dictionary.
Input

Standard input consists of a number of lowercase words, one per line, in alphabetical order. There will be no more than 50,000 words.
Only one case.
Output

Your output should contain all the hat’s words, one per line, in alphabetical order.
Sample Input

a ahat hat hatword hziee word
Sample Output

ahat hatword