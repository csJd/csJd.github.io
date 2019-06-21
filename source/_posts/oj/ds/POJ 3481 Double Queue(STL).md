---
title: POJ 3481 Double Queue(STL)
author: Deng
tags: 
       - Algorithm

category: 
       - Data structure
       - OJ

date: 2014-09-02 09:51:41
---
题意 模拟银行的排队系统 有三种操作 1-添加优先级为p 编号为k的人到队列 2-服务当前优先级最大的 3-服务当前优先级最小的 0-退出系统

可以用stl中的map 因为map本身就根据key的值排了序 对应2，3 我们只需要输出最大或最小就行了并从map中删除该键值

```js 
#include<cstdio>
#include<map>
using namespace std;
map<int, int> a;
int main()
{
    map<int, int>::iterator it;
    int n,k,p;
    while (scanf ("%d", &n), n)
    {
        if (n == 1)
        {
            scanf ("%d%d", &k, &p);
            a[p] = k;
        }
        else
        {
            if (a.empty()) printf ("0\n");
            else
            {
                if(n==2) it = a.end(),--it;
                else it=a.begin();
                printf ("%d\n", it->second);
                a.erase (it->first);
            }
        }
    }
    return 0;
}
```

Double Queue

Description
The new founded Balkan Investment Group Bank (BIG-Bank) opened a new office in Bucharest, equipped with a modern computing environment provided by IBM Romania, and using modern information technologies. As usual, each client of the bank is identified by a positive integer *K* and, upon arriving to the bank for some services, he or she receives a positive integer priority *P*. One of the inventions of the young managers of the bank shocked the software engineer of the serving system. They proposed to break the tradition by sometimes calling the serving desk with the **lowest** priority instead of that with the highest priority. Thus, the system will receive the following types of request:
0 The system needs to stop serving 1 *K P* Add client *K* to the waiting list with priority *P* 2 Serve the client with the highest priority and drop him or her from the waiting list 3 Serve the client with the lowest priority and drop him or her from the waiting list

Your task is to help the software engineer of the bank by writing a program to implement the requested serving policy.

Input

Each line of the input contains one of the possible requests; only the last line contains the stop-request (code 0). You may assume that when there is a request to include a new client in the list (code 1), there is no other request in the list of the same client or with the same priority. An identifier *K* is always less than 106, and a priority *P* is less than 107. The client may arrive for being served multiple times, and each time may obtain a different priority.

Output

For each request with code 2 or 3, the program has to print, in a separate line of the standard output, the identifier of the served client. If the request arrives when the waiting list is empty, then the program prints zero (0) to the output.

Sample Input

2 1 20 14 1 30 3 2 1 10 99 3 2 2 0

Sample Output

0 20 30 10 0