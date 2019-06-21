---
title: POJ 1094 Sorting It All Out（拓扑排序·判断+实现）
author: Deng
tags: 
       - Algorithm

category: 
       - Graph
       - OJ

date: 2015-08-19 14:59:12
---
题意 由一些不同元素组成的升序序列是可以用若干个小于号将所有的元素按从小到大的顺序 排列起来的序列。例如，排序后的序列为 A, B, C, D，这意味着 A < B、B < C和C < D。在本题中， 给定一组形如 A < B的关系式，你的任务是判定是否存在一个有序序列。 输出到哪一项可以确定顺序或者在这一项最先出现冲突，若所有的小于关系都处理完了都不能确定顺序也没有出现冲突，就输出不能确定

每来一个小于关系就进行一次拓扑排序 直到出现冲突（也就是出现了环）或者已经能确定顺序 当结果已经确定时 后面的小于关系也就没有必要处理了 因此可以用一个flag标记结果是否已经确定

```js 
#include <cstdio>
#include <cstring>
#include <vector>
using namespace std;
const int N = 30;
int n, m, in[N];
char ans[N], q[N];
vector<int> e[N];

int topoSort() //拓扑排序
{
    int d[N], ret = 1;
    memcpy(d, in, sizeof(d));
    int front  = 0, rear = 0, p = 0;
    for(int i = 0; i < n; ++i)
        if(d[i] == 0) q[rear++] = i;
    while(front < rear)
    {
        if(rear - front > 1) ret = 0; //顺序不能确定
        int cur = q[front++];
        ans[p++] = 'A' + cur;
        for(int i = 0; i < e[cur].size(); ++i)
        {
            int j = e[cur][i];
            if((--d[j]) == 0) q[rear++] = j;
        }
    }
    if(p < n) return -1; //有环
    ans[p] = 0;
    return ret;
}

int main()
{
    char a, b;
    while(scanf("%d%d", &n, &m), n || m)
    {
        for(int i = 0; i < n; ++i) e[i].clear();
        memset(in, 0, sizeof(in));
        int flag = 0;
        for(int i = 0; i < m; ++i)
        {
            scanf(" %c<%c", &a, &b);
            if(flag) continue;

            a -= 'A', b -= 'A';
            e[a].push_back(b);
            ++in[b];
            flag = topoSort();
            if(flag == 1)
                printf("Sorted sequence determined after %d relations: %s.\n", i + 1, ans);
            if(flag == -1)
                printf("Inconsistency found after %d relations.\n", i + 1);

        }
        if(!flag) puts("Sorted sequence cannot be determined.");
    }
    return 0;
}
//Last modified :  2015-08-19 13:45 CST
```

Sorting It All Out

Description
An ascending sorted sequence of distinct values is one in which some form of a less-than operator is used to order the elements from smallest to largest. For example, the sorted sequence A, B, C, D implies that A < B, B < C and C < D. in this problem, we will give you a set of relations of the form A < B and ask you to determine whether a sorted order has been specified or not.

Input

Input consists of multiple problem instances. Each instance starts with a line containing two positive integers n and m. the first value indicated the number of objects to sort, where 2 <= n <= 26. The objects to be sorted will be the first n characters of the uppercase alphabet. The second value m indicates the number of relations of the form A < B which will be given in this problem instance. Next will be m lines, each containing one such relation consisting of three characters: an uppercase letter, the character "<" and a second uppercase letter. No letter will be outside the range of the first n letters of the alphabet. Values of n = m = 0 indicate end of input.

Output

For each problem instance, output consists of one line. This line should be one of the following three:
Sorted sequence determined after xxx relations: yyy...y.
Sorted sequence cannot be determined.
Inconsistency found after xxx relations.
where xxx is the number of relations processed at the time either a sorted sequence is determined or an inconsistency is found, whichever comes first, and yyy...y is the sorted, ascending sequence.

Sample Input

4 6 A<B A<C B<C C<D B<D A<B 3 2 A<B B<A 26 1 A<Z 0 0

Sample Output

Sorted sequence determined after 4 relations: ABCD. Inconsistency found after 2 relations. Sorted sequence cannot be determined.