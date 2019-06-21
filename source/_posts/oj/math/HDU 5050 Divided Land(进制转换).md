---
title: HDU 5050 Divided Land(进制转换)
author: Deng
tags: 
       - Algorithm

category: 
       - Mathematics
       - OJ

date: 2014-09-28 10:53:57
---
题意 给你两个二进制数m,n 求他们的最大公约数 用二进制表示 0<m,n<2^1000

先把二进制转换为十进制 求出最大公约数 再把结果转换为二进制 数比较大要用到大数

```js 
import java.util.*;
import java.math.*;

public class wl6_9 {

	static BigInteger two = BigInteger.valueOf(2), one = BigInteger.ONE,
			zero = BigInteger.ZERO;

	static BigInteger gcd(BigInteger a, BigInteger b) {
		while (!(a.mod(b).equals(zero))) {
			BigInteger t = a.mod(b);
			a = b;
			b = t;
		}
		return b;
	}

	static void bprint(BigInteger x) {
		if (x.equals(zero))
			return;
		bprint(x.divide(two));
		if (x.mod(two).equals(one))
			System.out.print(1);
		else
			System.out.print(0);
	}

	public static void main(String[] args) {
		Scanner in = new Scanner(System.in);
		int T = in.nextInt();
		String a, b;
		for (int t = 1; t <= T; t++) {
			a = in.next();
			b = in.next();
			int la = a.length(), lb = b.length();

			BigInteger m = zero, n = zero;
			for (int i = 0; i < la; ++i) {
				m = m.multiply(two);
				if (a.charAt(i) == '1')
					m = m.add(one);
			}

			for (int i = 0; i < lb; ++i) {
				n = n.multiply(two);
				if (b.charAt(i) == '1')
					n = n.add(one);
			}
			System.out.printf("Case #%d: ", t);
			bprint(gcd(m, n));
			System.out.println();
		}
		in.close();
	}
}
```

# Divided Land

Problem Description

It’s time to fight the local despots and redistribute the land. There is a rectangular piece of land granted from the government, whose length and width are both in binary form. As the mayor, you must segment the land into multiple squares of equal size for the villagers. What are required is there must be no any waste and each single segmented square land has as large area as possible. The width of the segmented square land is also binary.
Input

The first line of the input is T (1 ≤ T ≤ 100), which stands for the number of test cases you need to solve.
Each case contains two binary number represents the length L and the width W of given land. (0 < L, W ≤ 21000)
Output

For each test case, print a line “Case /#t: ”(without quotes, t means the index of the test case) at the beginning. Then one number means the largest width of land that can be divided from input data. And it will be show in binary. Do not have any useless number or space.
Sample Input

3 10 100 100 110 10010 1100
Sample Output

Case /#1: 10 Case /#2: 10 Case /#3: 110
Source

[2014 ACM/ICPC Asia Regional Shanghai Online](http://acm.hdu.edu.cn/search.php?field=problem&key=2014+ACM%2FICPC+Asia+Regional+Shanghai+Online&source=1&searchmode=source)