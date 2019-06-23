---
title: UVa 11988 Broken Keyboard(链表的应用)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Data structure

date: 2014-09-02 09:50:48
---
## Broken Keyboard (a.k.a. Beiju Text)

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

题意 电脑键盘的home键和end键坏了 会在你不注意时自动按下

给你一个输入序列 '['代表home键 ']'代表end键 要求输出屏幕上对应的输出

用链表保存每个位置的字符c和下一个位置的编号next 最后一个字符的next为0

并用cur表示光标的移动

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 100005;
char s[N], c;
int cur, last, l;
struct Node{ char c; int next;}  lis[N];
int main()
{
    while (~scanf ("%s", s + 1))
    {
        l = strlen (s + 1);
        lis[0].next = last = cur = 0;
        for (int i = 1; i <= l; ++i)
        {
            c = s[i];
            if (c == '[') cur = 0;
            else if (c == ']') cur = last;
            else
            {
                lis[i].c = c;
                lis[i].next = lis[cur].next;
                lis[cur].next = i;
                if (cur == last) last = i;
                cur = i;
            }
        }
        for (int i = lis[0].next; i != 0; i = lis[i].next)
            printf ("%c", lis[i].c);
        printf ("\n");
    }
    return 0;
}
```