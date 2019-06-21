---
title: UVa 620 Cellular Structure(DP)
author: Deng
tags: 
       - Algorithm

category: 
       - DP
       - OJ

date: 2014-08-23 13:47:17
---
题意 有A,B两种细胞 A细胞可由空生成 非空细胞链有两种增长方式 设O为原非空细胞链 则O可增长为OAB或BOA给你一个细胞链 若其是合法分裂的 要求判断最后一次分裂是哪种方式 无法得到的输出MUTANT

若给定的S是以AB结尾的 判断去掉AB的部分是否合法即可 若S是B开始A结束的 判断去掉首尾是否合法即可；

```js 
#include <cstdio>  
#include <cstring>  
char s[10000];  
bool dp(int i, int j)  
{  
    if (i == j)  
        return s[i] == 'A' ? 1 : 0;  
    else if (s[j - 1] == 'A' && s[j] == 'B')  
        return dp(i, j - 2);  
    else if (s[i] == 'B' && s[j] == 'A')  
        return dp(i + 1, j - 1);  
    return 0;  
}  
int main()  
{  
    int t;  
    scanf("%d", &t);  
    while (t--)  
    {  
        scanf("%s", s + 1);  
        int l = strlen(s + 1);  
        if (!strcmp(s, "A"))  
            printf("SIMPLE\n");  
        else if (s[l - 1] == 'A' && s[l] == 'B' && dp(1, l - 2))  
            printf("FULLY-GROWN\n");  
        else if (s[1] == 'B' && s[l] == 'A' && dp(2, l - 1))  
            printf("MUTAGENIC\n");  
        else  
            printf("MUTANT\n");  
    }  
    return 0;  
}
```

#

****

A chain of connected cells of two types A and B composes a cellular structure of some microorganisms of species APUDOTDLS.

If no mutation had happened during growth of an organism, its cellular chain would take one of the following forms:

simple stage O = A fully-grown stage O = OAB mutagenic stage O = BOA

Sample notation O = OA means that if we added to chain of a healthy organism a cell A from the right hand side, we would end up also with a chain of a healthy organism. It would grow by one cell A.

A laboratory researches a cluster of these organisms. Your task is to write a program which could find out a current stage of growth and health of an organism, given its cellular chain sequence.

##

A integer*n*being a number of cellular chains to test, and then*n*consecutive lines containing chains of tested organisms.

##

For each tested chain give (in separate lines) proper answers:

SIMPLE for simple stage FULLY-GROWN for fully-grown stage MUTAGENIC for mutagenic stage MUTANT any other (in case of mutated organisms)

If an organism were in two stages of growth at the same time the first option from the list above should be given as an answer.

##

4 A AAB BAAB BAABA

##

SIMPLE FULLY-GROWN MUTANT MUTAGENIC