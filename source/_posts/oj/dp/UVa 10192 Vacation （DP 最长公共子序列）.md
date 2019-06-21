---
title: UVa 10192 Vacation （DP 最长公共子序列）
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2014-08-22 10:03:31
---
﻿﻿

题意：假期你要出去旅游 你爸爸和你妈妈对你去的城市有不同的意见 为了不让他们中的任何一个不开心 你得选出同时符合两人的方案 也就是最长公共子序列 这个大白上有很详细的讲解 直接用增量法就些 注意输入数据有空格 不能用scanf读入

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
#define M 110
using namespace std;
char a[M],b[M];
int d[M][M],n,m;
void LCS()
{
    memset(d,0,sizeof(d));
    n=strlen(a+1);
    m=strlen(b+1);
    for(int i=1; i<=n; ++i)
        for(int j=1; j<=m; ++j)
            if(a[i]==b[j]) d[i][j]=d[i-1][j-1]+1;
            else d[i][j]=max(d[i-1][j],d[i][j-1]);
}
int main()
{
    int t=0;
    while(gets(a+1)!=NULL,a[1]!='#')
    {
        ++t;
        gets(b+1);
        LCS();
        printf("Case #%d: you can visit at most %d cities.\n",t,d[n][m]);
    }
}
```

由于本题没要求打印方案 故还可用滚动数组以达到空间上的优化

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
#define M 110
using namespace std;
char a[M],b[M];
int d[M],n,m,r,temp;
void LCS()
{
    memset(d,0,sizeof(d));
    n=strlen(a+1);
    m=strlen(b+1);
    for(int i=1; i<=n; ++i)
    {
        r=0;
        for(int j=1; j<=m; ++j)
        {
            if(a[i]==b[j]) temp=r+1;
            else temp=max(d[j],d[j-1]);
            r=d[j];
            d[j]=temp;
        }
    }
}
int main()
{
    int t=0;
    while(gets(a+1)!=NULL,a[1]!='#')
    {
        ++t;
        gets(b+1);
        LCS();
        printf("Case #%d: you can visit at most %d cities.\n",t,d[m]);
    }
}
```

#

[Problem E: Vacation](http://vjudge.net/)

## [The Problem](http://vjudge.net/)

You are planning to take some rest and to go out on vacation, but you really don't know which cities you should visit. So, you ask your parents for help. Your mother says "My son, you MUST visit Paris, Madrid, Lisboa and London. But it's only fun in this order." Then your father says: "Son, if you're planning to travel, go first to Paris, then to Lisboa, then to London and then, at last, go to Madrid. I know what I'm talking about."

Now you're a bit confused, as you didn't expected this situation. You're afraid that you'll hurt your mother if you follow your father's suggestion. But you're also afraid to hurt your father if you follow you mother's suggestion. But it can get worse, because you can hurt both of them if you simply ignore their suggestions!

Thus, you decide that you'll try to follow their suggestions in the better way that you can. So, you realize that the "Paris-Lisboa-London" order is the one which better satisfies both your mother and your father. Afterwards you can say that you could not visit Madrid, even though you would've liked it very much.

If your father have suggested the "London-Paris-Lisboa-Madrid" order, then you would have two orders, "Paris-Lisboa" and "Paris-Madrid", that would better satisfy both of your parent's suggestions. In this case, you could only visit 2 cities.

You want to avoid problems like this one in the future. And what if their travel suggestions were bigger? Probably you would not find the better way very easy. So, you decided to write a program to help you in this task. You'll represent each city by one character, using uppercase letters, lowercase letters, digits and the space. Thus, you can have at most 63 different cities to visit. But it's possible that you'll visit some city more than once.

If you represent Paris with "a", Madrid with "b", Lisboa with "c" and London with "d", then your mother's suggestion would be "abcd" and you father's suggestion would be "acdb" (or "dacb", in the second example).

The program will read two travel sequences and it must answer how many cities you can travel to such that you'll satisfy both of your parents and it's maximum.

## [The Input](http://vjudge.net/)

The input will consist on an arbitrary number of city sequence pairs. The end of input occurs when the first sequence starts with an "/#"character (without the quotes). Your program should not process this case. Each travel sequence will be on a line alone and will be formed by legal characters (as defined above). All travel sequences will appear in a single line and will have at most 100 cities.

## [The Output](http://vjudge.net/)

For each sequence pair, you must print the following message in a line alone:
Case /#d: you can visit at most K cities. Where d stands for the test case number (starting from 1) and K is the maximum number of cities you can visit such that you'll satisfy both you father's suggestion and you mother's suggestion.

## [Sample Input](http://vjudge.net/)

abcd acdb abcd dacb /#

## [Sample Output](http://vjudge.net/)

Case /#1: you can visit at most 3 cities. Case /#2: you can visit at most 2 cities.

© 2001 Universidade do Brasil (UFRJ). Internal Contest Warmup 2001.