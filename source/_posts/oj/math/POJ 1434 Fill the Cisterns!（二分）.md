---
title: POJ 1434 Fill the Cisterns!（二分）
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Mathematics

date: 2015-03-23 22:36:47
---
题意 有n个互相连通的长方体水箱 给出每个水箱的位置和长宽高 判断放v体积的水时水面的高度 精确到两位小数

可以把每个水箱的位置 底面积 和高存起来 然用体积v二分高度 就是要注意精度的处理 最好转换成整数运算

```js 
import java.util.*;
public class Main {
	static int N = 50005;
	static long[] b = new long[N];
	static long[] h = new long[N];
	static long[] s = new long[N];
	static int n;

	static long getv(long k) {
		long v = 0;
		for (int i = 0; i < n; ++i) {
			if (b[i] + h[i] <= k) v += s[i] * h[i];
			else if (b[i] < k) v += (k - b[i]) * s[i];
		}
		return v;
	}

	public static void main(String args[]) {
		Scanner in = new Scanner(System.in);
		int cas = in.nextInt();
		while (cas-- > 0) {
			n = in.nextInt();
			long l = 0, r = 0, f = 0, mid = 0, v;
			for (int i = 0; i < n; ++i) {
				b[i] = in.nextLong() * 1000;
				h[i] = in.nextLong() * 1000;
				s[i] = in.nextLong() * in.nextLong();
				f += s[i] * h[i];
				if (b[i] + h[i] > r)  r = b[i] + h[i];
			}
			if ((v = in.nextLong() * 1000) > f)
				System.out.println("OVERFLOW");
			else {
				while (r >= l) {
					mid = (r + l) / 2;
					if (getv(mid) < v) l = mid + 1;
					else r = mid - 1;		
				}
				double ans = l / 1000.0;
				System.out.println(String.format("%.2f", ans));
			}
		}
		in.close();
	}
}
```

Fill the Cisterns!

Description
During the next century certain regions on earth will experience severe water shortages. The old town of Uqbar has already started to prepare itself for the worst. Recently they created a network of pipes connecting the cisterns that distribute water in each neighbourhood, making it easier to fill them at once from a single source of water. But in case of water shortage the cisterns above a certain level will be empty since the water will to the cisterns below.
  ![](../images/es-1434_1.jpg.png)  You have been asked to write a program to compute the level to which cisterns will be lled with a certain volume of water, given the dimensions and position of each cistern. To simplify we will neglect the volume of water in the pipes.
Task
Write a program which for each data set:
reads the description of cisterns and the volume of water,
computes the level to which the cisterns will be filled with the given amount of water,
writes the result.

Input

The first line of the input contains the number of data sets k, 1 <= k <= 30. The data sets follow.
The first line of each data set contains one integer n, the number of cisterns, 1 <= n <= 50 000. Each of the following n lines consists of 4 nonnegative integers, separated by single spaces: b, h, w, d - the base level of the cistern, its height, width and depth in meters, respectively. The integers satisfy 0 <= b <= 10^6 and 1 <= h /* w /* d <= 40 000. The last line of the data set contains an integer V - the volume of water in cubic meters to be injected into the network. Integer V satisfies 1 <= V <= 2 /* 10^9.

Output

The output should consist of exactly d lines, one line for each data set.
Line i, 1 <= i <= d, should contain the level that the water will reach, in meters, rounded up to two fractional digits, or the word 'OVERFLOW', if the volume of water exceeds the total capacity of the cisterns.

Sample Input

3 2 0 1 1 1 2 1 1 1 1 4 11 7 5 1 15 6 2 2 5 8 5 1 19 4 8 1 132 4 11 7 5 1 15 6 2 2 5 8 5 1 19 4 8 1 78

Sample Output

1.00 OVERFLOW 17.00