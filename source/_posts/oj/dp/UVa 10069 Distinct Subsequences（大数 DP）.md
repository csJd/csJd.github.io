---
title: UVa 10069 Distinct Subsequences（大数 DP）
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - DP

date: 2014-08-25 09:48:31
---
﻿﻿

题意 求母串中子串出现的次数（长度不超过1后面100个0 显然要用大数了）
令a为子串 b为母串 d[i][j]表示子串前i个字母在母串前j个字母中出现的次数当a[i]==b[j]&&d[i-1][j-1]!=0时 d[i][j]=d[i-1][j-1]+d[i][j-1]
(a[i]==b[j]时 子串前i个字母在母串前j个字母中出现的次数 等于 子串前i-1个字母在母串前j-1个字母中出现的次数 加上 子串前i个字母在母串前j-1个字母中出现的次数
a[i]!=b[j]时 子串前i个字母在母串前j个字母中出现的次数 等于 子串前i个字母在母串前j-1个字母中出现的次数)
懒得写大数模版就用java交的 ;

```js 
import java.util.*;
import java.math.*;

public class Main {
	public static void main(String args[]) {
		BigInteger d[][] = new BigInteger[105][10005];
		Scanner in = new Scanner(System.in);
		int t = in.nextInt();
		while ((t--) != 0) {
			String b = in.next();
			String a = in.next();
			int la = a.length();
			int lb = b.length();

			for (int i = 0; i < la; ++i)
				for (int j = 0; j < lb; ++j)
					d[i][j] = BigInteger.ZERO;

			if (a.charAt(0) == b.charAt(0))
				d[0][0] = BigInteger.ONE;
			for (int j = 1; j < lb; ++j) {
				if (a.charAt(0) == b.charAt(j))
					d[0][j] = d[0][j - 1].add(BigInteger.ONE);
				else
					d[0][j] = d[0][j - 1];
			}

			for (int i = 1; i < la; ++i)
				for (int j = 1; j < lb; ++j) {
					if (a.charAt(i) == b.charAt(j)
							&& d[i - 1][j - 1] != BigInteger.ZERO) {
						d[i][j] = d[i][j - 1].add(d[i - 1][j - 1]);
					} else
						d[i][j] = d[i][j - 1];
				}

			System.out.println(d[la - 1][lb - 1]);

		}
		in.close();
	}
}
```

还有没加大数模版的C++代码

```js 
#include<cstdio>
#include<cstring>
using namespace std;
char b[10005], a[105];
int d[105][10005], la, lb, t;
void dp()
{
    memset(d, 0, sizeof(d));
    for(int j = 1; j <= lb; ++j)
    {
        if(a[1] == b[j]) d[1][j] = d[1][j - 1] + 1;
        else d[1][j] = d[1][j - 1];
    }
    for(int i = 2; i <= la; ++i)
        for(int j = 1; j <= lb; ++j)
        {
            if(a[i] == b[j] && d[i - 1][j - 1])
            {
                d[i][j] = d[i][j - 1] + d[i - 1][j - 1];
            }
            else d[i][j] = d[i][j - 1];
        }
}
int main()
{
    scanf("%s", &t);
    while(t--)
    {
        scanf("%s%s", b + 1, a + 1);
        la = strlen(a + 1);
        lb = strlen(b + 1);
        dp();
        printf("%d\n", d[la][lb]);

    }
    return 0;
}
```

## Distinct Subsequences

A subsequence of a given sequence is just the given sequence with some elements (possibly none) left out. Formally, given a sequence ***X* = *x*1*x*2…*xm***, another sequence ***Z* = *z*1*z*2…*zk*** is a subsequence of ***X*** if there exists a strictly increasing sequence **<*i*1,*i*2, …, *ik*>** of indices of ***X*** such that for all *j* = 1, 2, …, *k*, we have ***xij* = *zj***. For example, ***Z* = *bcdb*** is a subsequence of ***X* =*abcbdab*** with corresponding index sequence **< 2, 3, 5, 7 >**.

In this problem your job is to write a program that counts the number of occurrences of ***Z*** in ***X*** as a subsequence such that each has a distinct index sequence.

****

**Input**

The first line of the input contains an integer ***N*** indicating the number of test cases to follow.

The first line of each test case contains a string ***X***, composed entirely of lowercase alphabetic characters and having length no greater than 10,000. The second line contains another string ***Z*** having length no greater than 100 and also composed of only lowercase alphabetic characters. Be assured that neither ***Z*** nor any prefix or suffix of ***Z*** will have more than 10100 distinct occurrences in ***X*** as a subsequence.

****

**Output**

For each test case in the input output the number of distinct occurrences of ***Z*** in ***X*** as a subsequence. Output for each input set must be on a separate line.****

****

**Sample Input**

2
babgbag
bag
rabbbit
rabbit

****

**Sample Output**

5
3