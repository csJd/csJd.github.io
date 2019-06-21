---
title: POJ 3623 Best Cow Line, Gold(模拟)
author: Deng
tags: 
       - Algorithm

category: 
       - Other
       - OJ

date: 2014-09-02 09:52:08
---
题意 给你一个字符序列 你每次可以从它的头部或尾部拿出一个字符组成一个新的字符序列 输出这样做能达到的最小的字符序列 每行最多输出80个字符(开始被这个坑了好久)

直接模拟就行 哪边小就选哪边 相等就往内看

```js 
#include<cstdio>
#include<iostream>
#include<string>
using namespace std;
const int N = 30010;

int main()
{
    char s[N][2];
    int n;
    while (~scanf ("%d", &n))
    {
        string ans;
        for (int i = 1; i <= n; ++i)
            scanf ("%s", s[i]);
        int le = 1, ri = n;
        while (le <= ri)
        {
            if (le == ri)
            {
                ans += s[le][0];
                break;
            }
            int tl = le, tr = ri;
            while (s[tl][0] == s[tr][0] && le <= ri) ++tl, --tr;
            if (s[tl][0] > s[tr][0])
                ans += s[ri][0], --ri;
            else
                ans += s[le][0], ++le;
        }

        int l = ans.length();
        if (l > 80)
        {
            for (int i = 1; i <= l; ++i)
                if (i != 1 && i % 80 == 1) printf ("\n%c", ans[i - 1]);
                else printf ("%c", ans[i - 1]);
            printf ("\n");
        }
        else cout << ans << endl;
    }
    return 0;
}
```

Best Cow Line, Gold

Description
FJ is about to take his *N* (1 ≤ *N* ≤ 30,000) cows to the annual"Farmer of the Year" competition. In this contest every farmer arranges his cows in a line and herds them past the judges.

The contest organizers adopted a new registration scheme this year: simply register the initial letter of every cow in the order they will appear (i.e., If FJ takes Bessie, Sylvia, and Dora in that order he just registers BSD). After the registration phase ends, every group is judged in increasing lexicographic order according to the string of the initials of the cows' names.

FJ is very busy this year and has to hurry back to his farm, so he wants to be judged as early as possible. He decides to rearrange his cows, who have already lined up, before registering them.

FJ marks a location for a new line of the competing cows. He then proceeds to marshal the cows from the old line to the new one by repeatedly sending either the first or last cow in the (remainder of the) original line to the end of the new line. When he's finished, FJ takes his cows for registration in this new order.

Given the initial order of his cows, determine the least lexicographic string of initials he can make this way.

Input

/* Line 1: A single integer: *N*
/* Lines 2..*N*+1: Line *i*+1 contains a single initial ('A'..'Z') of the cow in the *i*th position in the original line

Output

The least lexicographic string he can make. Every line (except perhaps the last one) contains the initials of 80 cows ('A'..'Z') in the new line.

Sample Input

6 A C D B C B

Sample Output

ABCBCD