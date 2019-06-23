---
title: UVa 136 Ugly Numbers(优先队列)
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Data structure

date: 2015-01-20 20:52:48
---
题意 质因数只可能有2,3,5的数称为丑数 输出第1500个丑数

STL优队列应用 1是丑数 丑数的2,3,5倍都是丑数 用优先队列模拟就行了

```js 
#include <cstdio>
#include <cstring>
#include <set>
#include <queue>
using namespace std;
typedef long long ll;

//priority_queue<ll, vector<ll>, greater<ll> > q;
struct cmp{
    bool operator () (ll a, ll b)    { return a > b; }
};
priority_queue<ll, vector<ll>, cmp> q;


set<ll> cnt;
int ugly[3] = {2, 3, 5};
int main()
{
    ll a, b;
    q.push(1);
    cnt.insert(1);
    for(int i = 1;; ++i)
    {
        a = q.top();
        q.pop();
        if(i == 1500) break;
        for(int j = 0; j < 3; ++j)
        {
            b = a * ugly[j];
            if(!cnt.count(b))
                cnt.insert(b), q.push(b);
        }
    }
    printf("The 1500'th ugly number is %lld.\n", a);
    return 0;
}
```

#

****

Ugly numbers are numbers whose only prime factors are 2, 3 or 5. The sequence

1, 2, 3, 4, 5, 6, 8, 9, 10, 12, 15, ...

shows the first 11 ugly numbers. By convention, 1 is included.

Write a program to find and print the 1500'th ugly number.

## Input and Output

There is no input to this program. Output should consist of a single line as shown below, with <number> replaced by the number computed.

## Sample output

The 1500'th ugly number is <number>.