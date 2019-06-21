---
title: HDU 2577 How to Type(模拟)
author: Deng
tags: 
       - Algorithm

category: 
       - Other
       - OJ

date: 2014-09-02 09:51:11
---
题目链接 http://acm.hdu.edu.cn/showproblem.php?pid=2577

题意 给你一个由大写字母和小写字母组成的字符串 模拟键盘输入的最少按键次数

直接模拟每个字符的输入 flag表示capslock的状态 1表示打开 0为关闭 开始是和输入完毕都是关闭的关闭的 用plu记录shift和capslock的按键次数

当接下来输入的字母有连续n个跟capslock状态不同时 分析可只 只有n=1时适合用shift键

如flag=1 n=1 输入a时 shift+a=2 而capslock+a+capslock=3

n>=2 如输入ab是 shift+a+shift+b=4 capslock+a+b+capslock=4

所以每次输入的字母如capslock状态不同时 就看有几个连续状态不同的字母 然后再选择用什么键

注意有一个例外当最后只有一个字母小写 capslock打开时 也应该用capslock 因为最后要换位小写状态

```js 
#include<cstdio>
#include<cctype>
#include<cstring>
using namespace std;
const int N = 105;
int main()
{
    char s[N]; int cas, t;
    scanf ("%d", &cas);
    while (cas--)
    {
        scanf ("%s", s + 1);
        int l = strlen (s + 1), plu = 0, flag = 0;

        for (int i = 1; i <= l; ++i)
        {
            t = 0;
            if (flag) while (islower (s[i])) ++t, ++i;
            else      while (isupper (s[i])) ++t, ++i;

            if (t > 1||(i == l + 1 && islower (s[l]) && t == 1))
                flag = !flag, ++plu, --i;
            else if (t == 1)
                ++plu, --i;
        }

        if (flag) ++plu;
        printf ("%d\n", l + plu);
    }
    return 0;
}
```

# How to Type

Problem Description

Pirates have finished developing the typing software. He called Cathy to test his typing software. She is good at thinking. After testing for several days, she finds that if she types a string by some ways, she will type the key at least. But she has a bad habit that if the caps lock is on, she must turn off it, after she finishes typing. Now she wants to know the smallest times of typing the key to finish typing a string.
Input

The first line is an integer t (t<=100), which is the number of test case in the input file. For each test case, there is only one string which consists of lowercase letter and upper case letter. The length of the string is at most 100.
Output

For each test case, you must output the smallest times of typing the key to finish typing this string.
Sample Input

3 Pirates HDUacm HDUACM
Sample Output

8 8 8

*Hint* The string “Pirates”, can type this way, Shift, p, i, r, a, t, e, s, the answer is 8. The string “HDUacm”, can type this way, Caps lock, h, d, u, Caps lock, a, c, m, the answer is 8 The string "HDUACM", can type this way Caps lock h, d, u, a, c, m, Caps lock, the answer is 8