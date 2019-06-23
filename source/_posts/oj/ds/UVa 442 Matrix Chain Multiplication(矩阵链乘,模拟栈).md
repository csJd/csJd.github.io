---
title: UVa 442 Matrix Chain Multiplication(矩阵链乘,模拟栈)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Data structure

date: 2014-09-17 09:32:50
---
题意 计算给定矩阵链乘表达式需要计算的次数 当前一个矩阵的列数等于后一个矩阵的行数时 他们才可以相乘 不合法输出error

输入是严格合法的 即使只有两个相乘也会用括号括起来 而且括号里最多有两个 那么就很简单了 遇到字母直接入栈 遇到反括号计算后入栈 然后就得到结果了

```js 
#include<cstdio>
#include<cctype>
#include<cstring>
using namespace std;
const int N = 1000;
int st[N], row[N], col[N], r[N], c[N];

int main()
{
    int n, ans, top;
    scanf("%d", &n);
    char na[3], s[N];
    for(int i = 1; i <= n; ++i)
    {
        scanf("%s", na);
        int j = na[0] - 'A';
        scanf("%d%d", &row[j], &col[j]);
    }

    while(~scanf("%s", &s))
    {
        int i;
        for(i = 0 ; i < 26; ++i)
            c[i] = col[i], r[i] = row[i];
        ans = top = 0;

        for(i = 0; s[i] != '\0'; ++i)
        {
            if(isalpha(s[i]))
            {
                int j = s[i] - 'A';
                st[++top] = j;
            }

            else if(s[i] == ')')
            {
                if(r[st[top]] != c[st[top - 1]])  break;
                else
                {
                    --top;
                    c[st[top]] = c[st[top + 1]];
                    ans += (r[st[top]] * c[st[top]] * r[st[top + 1]]);
                }
            }
        }
        if(s[i] == '\0') printf("%d\n", ans);
        else printf("error\n");
    }
    return 0;
}
```

#

****

Suppose you have to evaluate an expression like A/*B/*C/*D/*E where A,B,C,D and E are matrices. Since matrix multiplication is associative, the order in which multiplications are performed is arbitrary. However, the number of elementary multiplications needed strongly depends on the evaluation order you choose.

For example, let A be a 50/*10 matrix, B a 10/*20 matrix and C a 20/*5 matrix. There are two different strategies to compute A/*B/*C, namely (A/*B)/*C and A/*(B/*C).

The first one takes 15000 elementary multiplications, but the second one only 3500.

Your job is to write a program that determines the number of elementary multiplications needed for a given evaluation strategy.

## Input Specification

Input consists of two parts: a list of matrices and a list of expressions.

The first line of the input file contains one integer *n* ( ![tex2html_wrap_inline28](../images/dge.org-external-4-442img1.gif.png) ), representing the number of matrices in the first part. The next *n* lines each contain one capital letter, specifying the name of the matrix, and two integers, specifying the number of rows and columns of the matrix.

The second part of the input file strictly adheres to the following syntax (given in EBNF):

SecondPart = Line { Line } <EOF> Line = Expression <CR> Expression = Matrix | "(" Expression Expression ")" Matrix = "A" | "B" | "C" | ... | "X" | "Y" | "Z"

## Output Specification

For each expression found in the second part of the input file, print one line containing the word "error" if evaluation of the expression leads to an error due to non-matching matrices. Otherwise print one line containing the number of elementary multiplications needed to evaluate the expression in the way specified by the parentheses.

## Sample Input

9 A 50 10 B 10 20 C 20 5 D 30 35 E 35 15 F 15 5 G 5 10 H 10 20 I 20 25 A B C (AA) (AB) (AC) (A(BC)) ((AB)C) (((((DE)F)G)H)I) (D(E(F(G(HI))))) ((D(EF))((GH)I))

## Sample Output

0 0 0 error 10000