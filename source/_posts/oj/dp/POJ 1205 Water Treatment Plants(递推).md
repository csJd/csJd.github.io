---
title: POJ 1205 Water Treatment Plants(递推)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - DP

date: 2014-09-02 09:51:36
---
题意 建设一条河岸的污水处理系统 河岸有n个城市 每个城市都可以自己处理污水 V 也可以把污水传到相邻的城市处理 >或< 除了你传给我我也传给你这种情况 其它都是合法的 两端的城市不能传到不存在的城市

令d[i]表示有i个城市时的处理方法数 最后一个城市的处理方法有

1.V 自己处理自己的 与前i-1个城市的处理方法无关 有d[i-1]种方法

2.< 给左边的城市去处理 也与前i-1个城市的处理方法无关 把自己的污水给第i-1个城市就行了 有d[i-1]种方法

3.>V 左边有污水传过来 和自己的一起处理 这时第i-1个城市可以向右传了 如果这种情况发生的话 那么第i-1个城市肯定不可能是向左传的 但前i-2个城市的处理方法没有影响 所以第i-1个城市向左传的方法有d[i-2]种 然后第i-1个城市不向左传就有d[i-1]-d[i-2]种方法了

所以有递推公式d[i]=3/*d[i-1]-d[i-2];

增长速度很快要用大数处理 大数就用java了

```js 
import java.util.*;
import java.math.*;

public class Main {
	public static void main(String args[]) {
		Scanner in = new Scanner(System.in);
		BigInteger d[] = new BigInteger[105];
		d[1] = BigInteger.ONE;
		d[2] = BigInteger.valueOf(3);
		for (int i = 3; i <= 100; ++i)
			d[i] = d[i - 1].multiply(d[2]).subtract(d[i - 2]);
		while (in.hasNext())
			System.out.println(d[in.nextInt()]);
		in.close();
	}
<span style="font-family:Microsoft YaHei;">}</span>
```
还有没加大数模版的c++代码

```js 
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 105;
int main()
{
    int d[N] = {0, 1, 3}, n;
    for (int i = 3; i <= 100; ++i)
        d[i] = 3 * d[i - 1] - d[i - 2];
    while (~scanf ("%d", &n))
        printf ("%d\n", d[n]);
    return 0;
}
```

Water Treatment Plants

Description

River polution control is a major challenge that authorities face in order to ensure future clean water supply. Sewage treatment plants are used to clean-up the dirty water comming from cities before being discharged into the river.
As part of a coordinated plan, a pipeline is setup in order to connect cities to the sewage treatment plants distributed along the river. It is more efficient to have treatment plants running at maximum capacity and less-used ones switched off for a period. So, each city has its own treatment plant by the river and also a pipe to its neighbouring city upstream and a pipe to the next city downstream along the riverside. At each city's treatment plant there are three choices:

![](../images/es-1205_1.jpg.png)
The choices above ensure that:
every city must have its water treated somewhere and
at least one city must discharge the cleaned water into the river.
Let's represent a city discharging water into the river as "V" (a downwards flow), passing water onto its neighbours as ">" (to the next city on its right) or else "<" (to the left). When we have several cities along the river bank, we assign a symbol to each (V, < or >) and list the cities symbols in order. For example, two cities, A and B, can
each treat their own sewage and each discharges clean water into the river. So A's action is denoted V as is B's and we write "VV" ;
or else city A can send its sewage along the pipe (to the right) to B for treatment and discharge, denoted ">V";
or else city B can send its sewage to (the left to) A, which treats it with its own dirty water and discharges (V) the cleaned water into the river. So A discharges (V) and B passes water to the left (<), and we denote this situation as "V<".
We could not have "><" since this means A sends its water to B and B sends its own to A, so both are using the same pipe and this is not allowed. Similarly "<<" is not possible since A's "<" means it sends its water to a non-existent city on its left.
So we have just 3 possible set-ups that fit the conditions:
 A B A > B A < B V V V V RIVER~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~RIVER "VV" ">V" "V<"
If we now consider three cities, we can determine 8 possible set-ups.
Your task is to produce a program that given the number of cities NC (or treatment plants) in the river bank, determines the number of possible set-ups, NS, that can be made according to the rules define above.
You need to be careful with your design as the number of cities can be as large as 100.

Input

The input consists of a sequence of values, one per line, where each value represents the number of cities.

Output

Your output should be a sequence of values, one per line, where each value represents the number of possible set-ups for the corresponding number of cities read in the same input line.

Sample Input

2 3 20

Sample Output

3 8 102334155