---
title: UVa 839 Not so Mobile(树的递归输入)
author: Deng
tags: 
       - Algorithm

category: 
       - Data structure
       - OJ

date: 2014-09-24 09:17:07
---
题意 判断一个树状天平是否平衡 每个测试样例每行4个数 wl,dl,wr,dr 当wl/*dl=wr/*dr时 视为这个天平平衡 当wl或wr等于0是 下一行将是一个子天平 如果子天平平衡 wl为子天平的wl+wr 否则整个天平不平衡

容易看出 输入是递归的 所以我们可以直接递归 边输入边判断

```js 
#include<cstdio>
using namespace std;

bool solve(int &w)
{
    int wl, dl, wr, dr;
    bool mobl = true, mobr = true;
    scanf("%d %d %d %d", &wl, &dl, &wr, &dr);
    if(!wl) mobl = solve(wl);
    if(!wr) mobr = solve(wr);
    w = wl + wr;
    return mobl && mobr && (wl * dl == wr * dr);
}

int main()
{
    int cas, w;
    scanf("%d", &cas);
    while(cas--)
    {
        printf(solve(w) ? "YES\n" : "NO\n");
        if(cas) printf("\n");
    }
    return 0;
}
```

#

****

Before being an ubiquous communications gadget, a *mobile* was just a structure made of strings and wires suspending colourfull things. This kind of mobile is usually found hanging over cradles of small babies.

![\epsfbox{p839a.eps}](../images/dge.org-external-8-p839a.jpg.png)

The figure illustrates a simple mobile. It is just a wire, suspended by a string, with an object on each side. It can also be seen as a kind of lever with the fulcrum on the point where the string ties the wire. From the lever principle we know that to balance a simple mobile the product of the weight of the objects by their distance to the fulcrum must be equal. That is **W**l×**D**l = **W**r×**D**rwhere **D**l is the left distance, **D**r is the right distance, **W**l is the left weight and **W**r is the right weight.

In a more complex mobile the object may be replaced by a sub-mobile, as shown in the next figure. In this case it is not so straightforward to check if the mobile is balanced so we need you to write a program that, given a description of a mobile as input, checks whether the mobile is in equilibrium or not.

![\epsfbox{p839b.eps}](../images/dge.org-external-8-p839b.jpg.png)

##

The input begins with a single positive integer on a line by itself indicating the number of the cases following, each of them as described below. This line is followed by a blank line, and there is also a blank line between two consecutive inputs.

The input is composed of several lines, each containing 4 integers separated by a single space. The 4 integers represent the distances of each object to the fulcrum and their weights, in the format: **W**l **D**l **W**r **D**r

If **W**l or **W**r is zero then there is a sub-mobile hanging from that end and the following lines define the the sub-mobile. In this case we compute the weight of the sub-mobile as the sum of weights of all its objects, disregarding the weight of the wires and strings. If both **W**l and **W**rare zero then the following lines define two sub-mobiles: first the left then the right one.

##

For each test case, the output must follow the description below. The outputs of two consecutive cases will be separated by a blank line.

Write `YES' if the mobile is in equilibrium, write `NO' otherwise.

##

1 0 2 0 4 0 3 0 1 1 1 1 1 2 4 4 2 1 6 3 2

##

YES
#

****

Before being an ubiquous communications gadget, a *mobile* was just a structure made of strings and wires suspending colourfull things. This kind of mobile is usually found hanging over cradles of small babies.

![\epsfbox{p839a.eps}](../images/dge.org-external-8-p839a.jpg.png)

The figure illustrates a simple mobile. It is just a wire, suspended by a string, with an object on each side. It can also be seen as a kind of lever with the fulcrum on the point where the string ties the wire. From the lever principle we know that to balance a simple mobile the product of the weight of the objects by their distance to the fulcrum must be equal. That is **W**l×**D**l = **W**r×**D**rwhere **D**l is the left distance, **D**r is the right distance, **W**l is the left weight and **W**r is the right weight.

In a more complex mobile the object may be replaced by a sub-mobile, as shown in the next figure. In this case it is not so straightforward to check if the mobile is balanced so we need you to write a program that, given a description of a mobile as input, checks whether the mobile is in equilibrium or not.

![\epsfbox{p839b.eps}](../images/dge.org-external-8-p839b.jpg.png)

##

The input begins with a single positive integer on a line by itself indicating the number of the cases following, each of them as described below. This line is followed by a blank line, and there is also a blank line between two consecutive inputs.

The input is composed of several lines, each containing 4 integers separated by a single space. The 4 integers represent the distances of each object to the fulcrum and their weights, in the format: **W**l **D**l **W**r **D**r

If **W**l or **W**r is zero then there is a sub-mobile hanging from that end and the following lines define the the sub-mobile. In this case we compute the weight of the sub-mobile as the sum of weights of all its objects, disregarding the weight of the wires and strings. If both **W**l and **W**rare zero then the following lines define two sub-mobiles: first the left then the right one.

##

For each test case, the output must follow the description below. The outputs of two consecutive cases will be separated by a blank line.

Write `YES' if the mobile is in equilibrium, write `NO' otherwise.

##

1 0 2 0 4 0 3 0 1 1 1 1 1 2 4 4 2 1 6 3 2

##

YES