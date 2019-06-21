---
title: UVa 531 Compromise （DP 最长公共子序列）
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2014-08-22 10:20:33
---
题意 求两段文本的最长公共子单词序列

还是最长公共子序列 只是现在是单词不是一个字母了 用字符串数组保存 还是一样的处理 注意结果的打印和输入的处理

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
#define M 105
using namespace std;
char a[M][32],b[M][32];
int d[M][M],flag[M][M],la,lb;
bool Isfirst;
void lcs()
{
    memset(d,0,sizeof(d));
    memset(flag,0,sizeof(flag));
    for(int i=1; i<=la; ++i)
        for(int j=1; j<=lb; ++j)
        {
            if(!strcmp(a[i],b[j]))
            {
                d[i][j]=d[i-1][j-1]+1;
                flag[i][j]=1;
            }
            else if(d[i-1][j]>d[i][j-1])
            {
                d[i][j]=d[i-1][j];
                flag[i][j]=2;
            }
            else
            {
                d[i][j]=d[i][j-1];
                flag[i][j]=3;
            }
        }
}
void print(int i,int j)
{
    if(i<1||j<1)
    {
        Isfirst=true;
        return;
    }
    if(flag[i][j]==1)
    {
        print(i-1,j-1);
        if(Isfirst)
        {
            printf("%s",a[i]);
            Isfirst=false;
        }
        else printf(" %s",a[i]);
    }
    else if(flag[i][j]==2)
        print(i-1,j);
    else print(i,j-1);
}
int main()
{
    char t[M];
    while(scanf("%s",a[1])!=EOF)
    {
        la=1;
        lb=0;
        bool Isfirst=true;
        while(scanf("%s",t),t[0]!='#') strcpy(a[++la],t);
        while(scanf("%s",t),t[0]!='#') strcpy(b[++lb],t);
        lcs();
        print(la,lb);
        printf("\n");
    }
    return 0;
}
```

#

In a few months the European Currency Union will become a reality. However, to join the club, the Maastricht criteria must be fulfilled, and this is not a trivial task for the countries (maybe except for Luxembourg). To enforce that Germany will fulfill the criteria, our government has so many wonderful options (raise taxes, sell stocks, revalue the gold reserves,...) that it is really hard to choose what to do.

Therefore the German government requires a program for the following task:

Two politicians each enter their proposal of what to do. The computer then outputs the longest common subsequence of words that occurs in both proposals. As you can see, this is a totally fair compromise (after all, a common sequence of words is something what both people have in mind).

Your country needs this program, so your job is to write it for us.

##

The input file will contain several test cases.

Each test case consists of two texts. Each text is given as a sequence of lower-case words, separated by whitespace, but with no punctuation. Words will be less than 30 characters long. Both texts will contain less than 100 words and will be terminated by a line containing a single '/#'.

Input is terminated by end of file.

##

For each test case, print the longest common subsequence of words occuring in the two texts. If there is more than one such sequence, any one is acceptable. Separate the words by one blank. After the last word, output a newline character.

##

die einkommen der landwirte sind fuer die abgeordneten ein buch mit sieben siegeln um dem abzuhelfen muessen dringend alle subventionsgesetze verbessert werden /# die steuern auf vermoegen und einkommen sollten nach meinung der abgeordneten nachdruecklich erhoben werden dazu muessen die kontrollbefugnisse der finanzbehoerden dringend verbessert werden /#

##

die einkommen der abgeordneten muessen dringend verbessert werden

﻿﻿