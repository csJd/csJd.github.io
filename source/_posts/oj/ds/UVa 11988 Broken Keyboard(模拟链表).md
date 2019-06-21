---
title: UVa 11988 Broken Keyboard(模拟链表)
author: Deng
tags: 
       - Algorithm

category: 
       - Data structure
       - OJ

date: 2014-09-17 11:14:59
---
题意 有一个键盘坏了 会在你不知道的情况下按下home或者end 给你这个键盘的实际输入 要求输出显示器上的实际显示

输入最大5MB 所以直接数组检索肯定会超时的 用数组模拟链表 就可以很快了

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int N=100005;
char s[N];
int next[N];
int main()
{
    int last,cur;
    while(~scanf("%s",s+1))
    {
        next[0]=last=cur=0;
        int length=strlen(s+1);
        for(int i=1;i<=length;++i)
        {
            if(s[i]=='[') cur=0;
            else if(s[i]==']') cur=last;
            else
            {
                next[i]=next[cur];
                next[cur]=i;
                if(cur==last) last=i;
                cur=i;
            }
        }
        for(int i=next[0];i;i=next[i])
            printf("%c",s[i]);
        printf("\n");
    }
    return 0;
}
```

## Broken Keyboard

You're typing a long text with a broken keyboard. Well it's not so badly broken. The only problem with the keyboard is that sometimes the "home" key or the "end" key gets automatically pressed (internally).

You're not aware of this issue, since you're focusing on the text and did not even turn on the monitor! After you finished typing, you can see a text on the screen (if you turn on the monitor).

In Chinese, we can call it Beiju. Your task is to find the Beiju text.

## Input

There are several test cases. Each test case is a single line containing at least one and at most 100,000 letters, underscores and two special characters '[' and ']'. '[' means the "Home" key is pressed internally, and ']' means the "End" key is pressed internally. The input is terminated by end-of-file (EOF). The size of input file does not exceed 5MB.

## Output

For each case, print the Beiju text on the screen.

## Sample Input

This_is_a_[Beiju]_text [[]][][]Happy_Birthday_to_Tsinghua_University

## Output for the Sample Input

BeijuThis_is_a__text Happy_Birthday_to_Tsinghua_University