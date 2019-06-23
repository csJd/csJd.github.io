---
title: POJ 2182 Lost Cows （树状数组,递推）
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Data structure

date: 2014-08-22 10:43:32
---
﻿﻿ <span style="font-family:Microsoft YaHei;font-size:14px;">题意 N头牛排成一列 但编号顺序是乱的 知道每头牛前面有多少头牛编号在其之前 要求输出此时队列中每头牛的编号</span></span> <span style="font-family:Microsoft YaHei;font-size:14px;">本题有两种做法 一是利用数据结构的树状数组 时间复杂度低但相对较复杂 也可以依次推出每头牛的编号 这里先介绍递推的 以后再介绍数据结构优化的</span></span> <span style="font-family:Microsoft YaHei;font-size:14px;">从后往前递推 知道第i头牛在前i头牛里面的编号 即a[i]+1 然后加上a[i]+1之前已经被确定的编号个数（也就是被i后面的牛所确定的编号） 就是第i头牛的实际编了每次算i的编号时 i+1的编号都是确定的</span> <pre class="cpp" name="code">/#include<cstdio> using namespace std; /#define M 8005 /#define t a[i]+1 //t开始为i在前i头牛中的编号，后逐步推为实际编号； int main() { int sure[M]= {0},a[M],n; scanf("%d",&n); a[1]=0; for(int i=2; i<=n; i++) scanf("%d",&a[i]); for(int i=n; i>0; i--) { for(int j=1; j<=t; j++) if(sure[j]) //编号j已经被确定; a[i]++; sure[t]=1; } for(int i=1; i<=n; i++) printf("%d\n",t); return 0; }

Lost Cows

N (2 <= N <= 8,000) cows have unique brands in the range 1..N. In a spectacular display of poor judgment, they visited the neighborhood 'watering hole' and drank a few too many beers before dinner. When it was time to line up for their evening meal, they did not line up in the required ascending numerical order of their brands.
Regrettably, FJ does not have a way to sort them. Furthermore, he's not very good at observing problems. Instead of writing down each cow's brand, he determined a rather silly statistic: For each cow in line, he knows the number of cows that precede that cow in line that do, in fact, have smaller brands than that cow.
Given this data, tell FJ the exact ordering of the cows.

/* Line 1: A single integer, N
/* Lines 2..N: These N-1 lines describe the number of cows that precede a given cow in line and have brands smaller than that cow. Of course, no cows precede the first cow in line, so she is not listed. Line 2 of the input describes the number of preceding cows whose brands are smaller than the cow in slot /#2; line 3 describes the number of preceding cows whose brands are smaller than the cow in slot /#3; and so on.

/* Lines 1..N: Each of the N lines of output tells the brand of a cow in line. Line /#1 of the output tells the brand of the first cow in line; line 2 tells the brand of the second cow; and so on.

Sample Input
<span style="font-family:Comic Sans MS;font-size:12px;">5 1 2 1 0 </span>
Sample Output
<span style="font-family:Comic Sans MS;font-size:12px;">2 4 5 3 1</span>