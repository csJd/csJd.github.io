---
title: CF 505A Mr. Kitayuta's Gift(暴力)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - String

date: 2015-01-19 09:00:59
---
题意 在一个字符串中插入一个字母使其变成一个回文串 可以的话输出这个回文串 否则NA

大水题 插入情况最多就26/*11种 可以直接暴力

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 20;
char s[N], p[N];
int l;
bool ispal()
{
    for(int i = 0; i < (l + 1) / 2; ++i)
        if(p[i] != p[l - i]) return false;
    return true;
}

int main()
{
    int i, j, k;
    scanf("%s", s);
    l = strlen(s);
    for(char c = 'a'; c <= 'z'; ++c)
    {
        for(k = 0; k <= l; ++k)
        {
            i = j = -1;
            while(i < k - 1) p[++j] = s[++i];
            p[++j] = c;
            while(i < l - 1) p[++j] = s[++i];
            if(ispal())
            {
                printf("%s\n", p);
                return 0;
            }
        }
    }
    printf("NA\n");
    return 0;
}
```
A. Mr. Kitayuta's Gift

Mr. Kitayuta has kindly given you a string *s* consisting of lowercase English letters. You are asked to insert exactly one lowercase English letter into *s* to make it a palindrome. A palindrome is a string that reads the same forward and backward. For example, " noon ", " testset " and " a " are all palindromes, while " test " and " kitayuta " are not.

You can choose any lowercase English letter, and insert it to any position of *s*, possibly to the beginning or the end of *s*. You have to insert a letter even if the given string is already a palindrome.

If it is possible to insert one lowercase English letter into *s* so that the resulting string will be a palindrome, print the string after the insertion. Otherwise, print "NA" (without quotes, case-sensitive). In case there is more than one palindrome that can be obtained, you are allowed to print any of them.
Input

The only line of the input contains a string *s* (1 ≤ |*s*| ≤ 10). Each character in *s* is a lowercase English letter.

Output

If it is possible to turn *s* into a palindrome by inserting one lowercase English letter, print the resulting string in a single line. Otherwise, print "NA" (without quotes, case-sensitive). In case there is more than one solution, any of them will be accepted.
Sample test(s)

input revive

output reviver
input ee

output eye
input kitayuta

output NA