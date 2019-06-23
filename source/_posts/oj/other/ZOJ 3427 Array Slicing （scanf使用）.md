---
title: ZOJ 3427 Array Slicing （scanf使用）
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - Other

date: 2015-07-24 16:00:46
---
题意 Watashi发明了一种蛋疼(eggache) 语言 你要为这个语言实现一个 array slicing 函数 这个函数的功能是 有一个数组初始为空 每次给你一个区间[ l, r) 和一些数 你要输出数组中下标在[l, r) 之间的数 然后删除这些数 然后把给你的那些数插入到数组的下标为 l 的位置

签到模拟题 一直没看懂题意 看了Watashi的scanf高端用法 弱到连scanf都不会用了 强行到[cpp](http://www.cplusplus.com/reference/cstdio/scanf/?kw=scanf)预习了一下 先记录一下那些并不了解的scanf用法吧

int scanf ( const char /* format, ... );

format 由这些内容组成

* **空白字符:** scanf在读入时会忽略下一个非空白字符之前的所有空白字符 [空白字符](http://www.cplusplus.com/reference/cctype/isspace/)包含 ' ''\t', '\n', '\v', '\f', '\r
/* 表示这个格式说明符所对应的内容读而不存 也不用为其指定参数 (注意区别 printf /* 需要一个整型参数 表示至少输出多少位 不足用空格代替)

width 表示最多读取多少位字符
length 有的话一定是这几个之一 hh , h , l , ll , j , z , t , L 对应同种数据的不同位数类型如 int(d) 和 long long(lld)   重点是 specifier *specifier* 描述 Characters extracted i Integer Any number of digits, optionally preceded by a sign (+ or -).
[Decimal digits](http://www.cplusplus.com/isdigit) assumed by default (0-9), but a 0 prefix introduces octal digits (0-7), and 0x [hexadecimal digits](http://www.cplusplus.com/isxdigit) (0-f).
*Signed* argument. d *or* u
 Decimal integer Any number of [decimal digits](http://www.cplusplus.com/isdigit) (0-9), optionally preceded by a sign (+ or -).
d is for a *signed* argument, and u for an *unsigned*. o Octal integer Any number of octal digits (0-7), optionally preceded by a sign (+ or -).
*Unsigned* argument. x Hexadecimal integer Any number of [hexadecimal digits](http://www.cplusplus.com/isxdigit) (0-9, a-f, A-F), optionally preceded by 0x or 0X, and all optionally preceded by a sign (+ or -).
*Unsigned* argument. f, e, g Floating point number A series of [decimal](http://www.cplusplus.com/isdigit) digits, optionally containing a decimal point, optionally preceeded by a sign (+ or -) and optionally followed by the e or E character and a decimal integer (or some of the other sequences supported by [strtod](http://www.cplusplus.com/strtod)).
Implementations complying with C99 also support hexadecimal floating-point format when preceded by

```js 
0x
```
or

```js 
0X
```
. a c Character The next character. If a *width* other than 1 is specified, the function reads exactly *width*characters and stores them in the successive locations of the array passed as argument. No null character is appended at the end. s String of characters Any number of non-whitespace characters, stopping at the first [whitespace](http://www.cplusplus.com/isspace) character found. A terminating null character is automatically added at the end of the stored sequence. p Pointer address A sequence of characters representing a pointer. The particular format used depends on the system and library implementation, but it is the same as the one used to format %p in [fprintf](http://www.cplusplus.com/fprintf). [*characters*] Scanset Any number of the characters specified between the brackets.
A dash (-) that is not the first character may produce non-portable behavior in some library implementations. [^*characters*] Negated scanset Any number of characters none of them specified as *characters* between the brackets. n Count No input is consumed.
The number of characters read so far from [stdin](http://www.cplusplus.com/stdin) is stored in the pointed location. % % A % followed by another % matches a single %.
除了 n 之外 specifier至少匹配一个输入的字符 否则读入会在当前字符终止 n需要一个指向int的参数 保存在这之前读取了多少位字符  
```js 
int main()
{
    char s[500];
    int n;
    scanf("%s%n", s, &n);
    printf("%s  %d\n", s, n);
    //输入hello
    //输出hello 5
    return 0;
}
```
 printf中也可以使用%n表示这条语句这之前输出了多少位字符  
```js 
int main()
{
    int n;
    printf("hello%n", &n);
    printf(" %d\n", n);
    //输出hello 5
    return 0;
}
```

specifier可以是[...]、[^...]这两种[正则表达式](https://zh.wikipedia.org/wiki/%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F)

[...]表示读取括号中包含的字符 遇到其他字符就结束这个specifier
```js 
int main()
{
    char s[500];
    int n;
    scanf(" %[0-9]%n", s, &n);//%[0-9]只读取数字  遇到非数字结束
    printf("%s %d\n", s, n);
    scanf(" %[ab]%n", s, &n);
    printf("%s %d\n", s, n);
    //输入"   12345abc"  前面有三个空格
    //输出12345 8  ab 2
    return 0;
}
```

[^...]表示读取除括号中字符之外的所有字符 遇到括号中的字符就结束这个specifier

当然这些东西sscanf也适用

感觉预习了scanf之后这个题的输入就很好处理了 直接用list的[splice](http://www.cplusplus.com/reference/list/list/splice/)函数就行了
```js 
#include <bits/stdc++.h>
using namespace std;
char s[5000], *ps;

int main()
{
    list<int> a, b;
    int l, r, n, v;
    list<int>::iterator it;
    while(~scanf(" [ %d : %d ]%[^\n]", &l, &r, s))
    {
        b.clear();
        ps = s;
        while(sscanf(ps, "%*[^-0-9]%d%n", &v, &n) > 0)
        {
            //过滤掉非'-'和数字再读入一个数
            //n记录scanf读了多少个字符
            ps = ps + n;  //读了n个字符所以要前进n位
            b.push_back(v);
        }

        advance(it = a.begin(), l);
        while(l < r)
        {
            printf("%d%s", *it, l < r - 1 ? ", " : "");
            it = a.erase(it);
            l++;
        }
        puts("");
        a.splice(it, b);
    }
    return 0;
}
```

Array Slicing    Time Limit:2 Seconds Memory Limit:65536 KB

Array slicing is an operation that extracts certain elements from an array and packages them as another array. Now you're asked to implements the array slicing operations for a new programming language -- *eggache/** (pronounced "eggache star"). The grammar of array slicing in *eggache/** is:
[ *begin* : *end* ] = *x1*, *x2*, ..., *xk*, ...
where begin ≤ end are indices indicating the range of slice. Redundant whitespaces should be ignored. For each operation, the original slice should be printed first, then these elements will be replaced with the new elements provided. See sample for more details.

#### Input

There is only one case for this problem, which contains about 50 lines of array slicing operations. It's guaranteed that all operations are valid and the absolute values of all integers never exceed 100. **The array is empty ([]) before the first operation.**

#### Output

The output produced by array slicing operations in the *eggache/** programming language.

#### Sample Input

[ 0 : 0] = 1 2 3 4 5 6 7 8 9 [ 1 : 1] = -1 [ 1 : 1] = [ 0 : 8] = 9 8 7 6 5 4 3 2 1 [ 2 : 8] = -2, -3, -5, -7 [ 0 : 9] = 000 [ 0 : 1] = 1, 2, 8 [ 2 : 2] = 4 [ 0 : 4] =

#### Sample Output

1, -1, 2, 3, 4, 5, 6, 7 7, 6, 5, 4, 3, 2 9, 8, -2, -3, -5, -7, 1, 8, 9 0 1, 2, 4, 8