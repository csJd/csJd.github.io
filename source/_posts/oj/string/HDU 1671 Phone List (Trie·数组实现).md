---
title: HDU 1671 Phone List (Trie·数组实现)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - String

date: 2015-07-27 19:56:43
---
题意 给你一组电话号码 判断其中是否有某个电话是另一个电话的前缀

字典树的基础应用 可以先把所有电话存进Trie 标记每个电话的结束字符 然后再查询每个号码 看中途是否有结束标记 有的话就说明有号码是这个号码的前缀了

实际上 插入完成就能知道是否有号码是另一个号码的前缀了 假设A是B的前缀

若A在B之前插入 那么插入B的时候会遇到A的结束标记

弱A在B之后插入 那么A插入完成之后的结点还有非空的孩子结点

这样就只用在每次插入时判断就行了 可以省掉查询这一步

指针和动态内存分配实现字典更容易些 但在有多组样例的时侯不释放内存会导致MLE 释放内存又要多花费不必要的时间 可能导致TLE 所以用数组实现比较好

```js 
#include <cstdio>
#include <cstring>
using namespace std;
const int N = 10005;
char tel[N][12];
int trie[N * 10][10], L;
bool end[N * 10], flag;

void initTrie()  //清空Trie
{
    L = 1;
    memset(trie, 0, sizeof(trie));
    memset(end, 0, sizeof(end));
}

void insertTrie(char s[])  //插入
{
    int r = 0, i = 0, j;
    while(s[i])
    {
        if(end[r]) flag = false;  //遇到其它号码的结束标记
        j = s[i++] - '0';
        if(!trie[r][j])
            trie[r][j] = L++;
        r = trie[r][j];
    }
    for(int i = 0; i < 10; ++i)  //插入完成的节点还有孩子节点
        if(trie[r][i]) flag = false;
    end[r] = true;
}

int main()
{
    int T, n;
    scanf("%d", &T);

    while(T--)
    {
        initTrie();
        scanf("%d", &n);
        flag = true;
        for(int i = 0; i < n; ++i)
        {
            scanf("%s", tel[i]);
            insertTrie(tel[i]);
        }
        puts(flag ? "YES" : "NO");
    }

    return 0;
}
//Last modified :   2015-07-27 19:36
```

# Phone List

Problem Description

Given a list of phone numbers, determine if it is consistent in the sense that no number is the prefix of another. Let’s say the phone catalogue listed these numbers:
1. Emergency 911
2. Alice 97 625 999
3. Bob 91 12 54 26
In this case, it’s not possible to call Bob, because the central would direct your call to the emergency line as soon as you had dialled the first three digits of Bob’s phone number. So this list would not be consistent.
Input

The first line of input gives a single integer, 1 <= t <= 40, the number of test cases. Each test case starts with n, the number of phone numbers, on a separate line, 1 <= n <= 10000. Then follows n lines with one unique phone number on each line. A phone number is a sequence of at most ten digits.
Output

For each test case, output “YES” if the list is consistent, or “NO” otherwise.
Sample Input

2 3 911 97625999 91125426 5 113 12340 123440 12345 98346
Sample Output

NO YES