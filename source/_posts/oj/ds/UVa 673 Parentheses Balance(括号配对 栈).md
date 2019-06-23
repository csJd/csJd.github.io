---
title: UVa 673 Parentheses Balance(括号配对 栈)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Data structure

date: 2015-01-23 16:54:03
---
题意 判断输入的括号序列是否是配对的

栈的基础应用 栈顶元素与输入的字符匹配就出栈咯 注意括号序列可以为空

STL栈

```js 
#include <bits/stdc++.h>
using namespace std;
int main()
{
    int cas;
    char c;
    cin >> cas;
    getchar();
    while(cas--)
    {
        stack<char> s;
        while((c = getchar()) != '\n')
        {
            if(s.empty()) s.push(c);
            else  if((c == ')' && s.top() == '(')
                     || (c == ']' && s.top() == '['))
                s.pop();
            else s.push(c);
        }
        puts(s.empty() ? "Yes" : "No");
    }
    return 0;
}
```
数组模拟栈

```js 
#include<cstdio>
using namespace std;
int main()
{
    int cas, top;
    char s[200], c;
    scanf("%d", &cas);
    getchar();
    while(cas--)
    {
        top = 0;
        while((c = getchar()) != '\n')
        {
            if(top == 0) s[top++] = c;
            else if(s[top - 1] == '(' && c == ')'
                    || s[top - 1] == '[' && c == ']')
                --top;
            else s[top++] = c;
        }
        puts(top ? "No" : "Yes");
    }
    return 0;
}
```

#

****

You are given a string consisting of parentheses () and []. A string of this type is said to be*correct*:
(a) if it is the empty string (b) if A and B are correct, AB is correct, (c) if A is correct, (A ) and [A ] is correct.

Write a program that takes a sequence of strings of this type and check their correctness. Your program can assume that the maximum string length is 128.

##

The file contains a positive integer n and a sequence of n strings of parentheses () and [] , one string a line.

##

A sequence of Yes or No on the output file.

##

3 ([]) (([()]))) ([()[]()])()

##

Yes No Yes