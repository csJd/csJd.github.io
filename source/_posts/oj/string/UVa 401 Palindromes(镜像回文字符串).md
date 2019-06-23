---
title: UVa 401 Palindromes(镜像回文字符串)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - String

date: 2014-08-30 09:28:54
---
﻿﻿

题意 给一个字符串 判定其是否为回文串和镜像串 回文串很好判断 镜像串对于每一个字符用数组保存它的镜像字符就行了 没有的就是空格

注意若字符串长度为奇数 中间那个字母必须是对称的才是镜像串

```js 
#include<cstdio>
#include<cctype>
#include<cstring>
const int N = 35;
int l;
char s[N], mc[] = "A   3  HIL JM O   2TUVWXY5", mn[] = "1SE Z  8 ";

bool isRegular()
{
    for (int i = 1; i <= l / 2; ++i)
        if (s[i] != s[l - i + 1]) return false;
    return true;
}

bool isMirrored()
{
    for (int i = 1; i <= (l + 1) / 2 ; ++i)
    {
        if (isalpha (s[i]) && s[l - i + 1] != mc[s[i] - 'A']) return false;
        else if (isdigit (s[i]) && s[l - i + 1] != mn[s[i] - '1']) return false;
    }
    return true;
}

int main()
{
    while (scanf ("%s", s + 1) != EOF)
    {
        l = strlen (s + 1);
        if (isMirrored())
        {
            if (isRegular()) printf ("%s -- is a mirrored palindrome.\n\n", s + 1);
            else printf ("%s -- is a mirrored string.\n\n", s + 1);
        }
        else if (isRegular()) printf ("%s -- is a regular palindrome.\n\n", s + 1);
        else printf ("%s -- is not a palindrome.\n\n", s + 1);
    }
    return 0;
}
```

#

****

A regular palindrome is a string of numbers or letters that is the same forward as backward. For example, the string "ABCDEDCBA" is a palindrome because it is the same when the string is read from left to right as when the string is read from right to left.

A mirrored string is a string for which when each of the elements of the string is changed to its reverse (if it has a reverse) and the string is read backwards the result is the same as the original string. For example, the string "3AIAE" is a mirrored string because "A" and "I" are their own reverses, and "3" and "E" are each others' reverses.

A mirrored palindrome is a string that meets the criteria of a regular palindrome and the criteria of a mirrored string. The string "ATOYOTA" is a mirrored palindrome because if the string is read backwards, the string is the same as the original and because if each of the characters is replaced by its reverse and the result is read backwards, the result is the same as the original string. Of course, "A", "T", "O", and "Y" are all their own reverses.

A list of all valid characters and their reverses is as follows.

Character Reverse Character Reverse Character Reverse A A M M Y Y B  N  Z 5 C  O O 1 1 D  P  2 S E 3 Q  3 E F  R  4 G  S 2 5 Z H H T T 6 I I U U 7 J L V V 8 8 K  W W 9 L J X X

**Note** that O (zero) and 0 (the letter) are considered the same character and therefore **ONLY** the letter "0" is a valid character.

##

Input consists of strings (one per line) each of which will consist of one to twenty valid characters. There will be no invalid characters in any of the strings. Your program should read to the end of file.

##

For each input string, you should print the string starting in column 1 immediately followed by exactly one of the following strings.

STRING CRITERIA " -- is not a palindrome." if the string is not a palindrome and is not a mirrored string " -- is a regular palindrome." if the string is a palindrome and is not a mirrored string " -- is a mirrored string." if the string is not a palindrome and is a mirrored string " -- is a mirrored palindrome." if the string is a palindrome and is a mirrored string

**Note** that the output line is to include the -'s and spacing exactly as shown in the table above and demonstrated in the Sample Output below.

In addition, after each output line, you must print an empty line.

##

NOTAPALINDROME ISAPALINILAPASI 2A3MEAS ATOYOTA

##

NOTAPALINDROME -- is not a palindrome. ISAPALINILAPASI -- is a regular palindrome. 2A3MEAS -- is a mirrored string. ATOYOTA -- is a mirrored palindrome.