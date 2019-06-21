---
title: ZOJ 3447 Doraemon's Number Game(Java优先队列·BigInteger)
author: Deng
tags: 
       - Algorithm

category: 
       - Data structure
       - OJ

date: 2015-07-29 20:40:22
---
题意 给你一个数组 你每次可以从中删掉2到k个数 然后把删掉的数的积加入到原数组 直到最后只剩一个数 求这样能得到的最大值和最小值的差

每次选的数值越小 选的数量越少 最后得到的结果肯定越大 因为这样大的数可以乘以最大的倍数 运算的次数也是最多从而使+1的次数最多 这显然是有利于最后结果的增大的

同理 每次选的数越大 选的数越多 最后得到的结果越小

这样最大值就是每次取最小的两个数 最大值就是每次取最大的k个数了 很容易用优先队列实现 结果会很大 用Java的BigInteger实现比较方便

```js 
import java.math.BigInteger;
import java.util.*;

public class Main {
	public static void main(String args[]) {
		Scanner in = new Scanner(System.in);
		int N = 105;

		PriorityQueue<BigInteger> inc = new PriorityQueue<BigInteger>();
		//优先队列默认小的优先 是增序队列
		PriorityQueue<BigInteger> dec = new PriorityQueue<BigInteger>(N,
				new Comparator<BigInteger>() {
					public int compare(BigInteger o1, BigInteger o2) {
						return -o1.compareTo(o2);
					}
				}); //匿名实现Comparator接口  降序大的优先

		int n, k;
		BigInteger a, b, one = BigInteger.ONE;
		List<BigInteger> list = new ArrayList<BigInteger>();

		while (in.hasNext()) {
			n = in.nextInt();
			k = in.nextInt();

			list.clear();
			for (int i = 0; i < n; ++i) {
				a = in.nextBigInteger();
				list.add(a);
			}

			inc.addAll(list);
			while (inc.size() > 1) {
				a = inc.poll();
				b = inc.poll();
				inc.add(a.multiply(b).add(one));
			}

			dec.addAll(list);
			while (dec.size() > 1) {
				a = one;
				for (int i = 0; i < k; ++i)
					if (dec.size() > 0)
						a = a.multiply(dec.poll());
				a = a.add(one);
				dec.add(a);
			}
			System.out.println(inc.poll().subtract(dec.poll()));
		}

		in.close();
	}
}
```

ZOJ Problem Set - 3447
Doraemon's Number Game    Time Limit:2 Seconds Memory Limit:65536 KB

Doraemon and Nobita are playing a number game. First, Doraemon will give Nobita *N* positive numbers. Then Nobita can deal with these numbers for two rounds. Every time Nobita can delete *i* (2 ≤ *i* ≤ *K*) numbers from the number set. Assume that the numbers deleted is a[ 1 ], a[ 2 ] ... a[ i ]. There should be a new number *X* = ( a[ 1 ] /* a[ 2 ] /* ... /* a[ i ] + 1 ) to be inserted back into the number set. The operation will be applied to the number set over and over until there remains only one number in the set. The number is the result of round. Assume two results *A* and *B* are produced after two rounds. Nobita can get |*A* - *B*| scores.

Now Nobita wants to get the highest score. Please help him.

#### Input

Input will contain no more than 10 cases. The first line of each case contains two positive integers *N* and *K* (1 ≤ *N* ≤ 100, 1 ≤ *K* ≤ 50). The following line contains *N* positive integers no larger than 50, indicating the numbers given by Doraemon.

#### Output

For each case, you should output highest score in one line.

#### Sample Input

6 3 1 3 4 10 7 15

#### Sample Output

5563

#### Hint

For most cases, *N* ≤ 20