---
title: UVa 213 Message Decoding(World Finals1991，字符串)
author: Deng
tags: 
       - Algorithm

category: 
       - String
       - OJ

date: 2014-09-02 09:50:17
---
#

****

Some message encoding schemes require that an encoded message be sent in two parts. The first part, called the header, contains the characters of the message. The second part contains a pattern that represents the message. You must write a program that can decode messages under such a scheme.

The heart of the encoding scheme for your program is a sequence of ``key" strings of 0's and 1's as follows:

![displaymath26](../images/dge.org-external-2-213img1.gif.png)

The first key in the sequence is of length 1, the next 3 are of length 2, the next 7 of length 3, the next 15 of length 4, etc. If two adjacent keys have the same length, the second can be obtained from the first by adding 1 (base 2). Notice that there are no keys in the sequence that consist only of 1's.

The keys are mapped to the characters in the header in order. That is, the first key (0) is mapped to the first character in the header, the second key (00) to the second character in the header, the *k*th key is mapped to the *k*th character in the header. For example, suppose the header is:

AB/#TANCnrtXc

Then 0 is mapped to A, 00 to B, 01 to /#, 10 to T, 000 to A, ..., 110 to X, and 0000 to c.

The encoded message contains only 0's and 1's and possibly carriage returns, which are to be ignored. The message is divided into segments. The first 3 digits of a segment give the binary representation of the length of the keys in the segment. For example, if the first 3 digits are 010, then the remainder of the segment consists of keys of length 2 (00, 01, or 10). The end of the segment is a string of 1's which is the same length as the length of the keys in the segment. So a segment of keys of length 2 is terminated by 11. The entire encoded message is terminated by 000 (which would signify a segment in which the keys have length 0). The message is decoded by translating the keys in the segments one-at-a-time into the header characters to which they have been mapped.

## Input

The input file contains several data sets. Each data set consists of a header, which is on a single line by itself, and a message, which may extend over several lines. The length of the header is limited only by the fact that key strings have a maximum length of 7 (111 in binary). If there are multiple copies of a character in a header, then several keys will map to that character. The encoded message contains only 0's and 1's, and it is a legitimate encoding according to the described scheme. That is, the message segments begin with the 3-digit length sequence and end with the appropriate sequence of 1's. The keys in any given segment are all of the same length, and they all correspond to characters in the header. The message is terminated by 000.

Carriage returns may appear anywhere within the message part. They are *not* to be considered as part of the message.

## Output

For each data set, your program must write its decoded message on a separate line. There should not be blank lines between messages.

## Sample input

TNM AEIOU 0010101100011 1010001001110110011 11000 $/#/*/*\ 0100000101101100011100101000

## Sample output

TAN ME /#/#/*\$

题意 编写一个解码程序 对数字串进行解码

输入第一行是一个解码key key从左到右每个字符分别对应0,00,01,10,000,001,011,100,101,110,0000,0001,...,1101,1110,00000,.......

长度为len的字符编码有2^n-1个 而且恰好以二进制方式从0到2^n-2递增 而且字符编码的最大长度为7 可以有2^7-1=127个字符

我们只需开一个key[len][val]数组 里面存的是长度为len的第val+1个字符编码 然后解码时对应输出每个字符就行

如样例2 解码key为"S/#/*/*\"

key[1][0]对应编码'0'存的字符为'$' key[2][0]对应编码'00'存的字符为'/#'

key[2][1]对应编码'01'存的字符为'/*' key[2][2]对应编码'10'存的字符为'/*'

key[3][0]对应编码'000'存的字符为'\'

需要解码的文本由多个小节组成 每个小节的前三个数字代表小节中每个编码的长度（用二进制表示，例如010表示2） 然后是该长度的不定个字符编码 以全1结束 如长度为2时 '11'就表示结束 长度为000时表示需要编码的文本结束；

此题是算法竞赛入门经典第二版中的例题 第83页 里面有详细的讲解；

```js 
#include<cstdio>
#include<cstring>
#include<cctype>
using namespace std;
const int N = 1000;
char s[N], key[8][1 << 8], c;
int val, len, l;

void read()
{
    len = 1; l = strlen (s);
    for (int i = val = 0; i < l; ++i)
        if (val < ( (1 << len) - 1)) 
            key[len][val++] = s[i];
        else
        {
            val = 0;
            key[++len][val++] = s[i];
        }
}

void print ()
{
    for (int i = val = 0; i < len; ++i)
    {
        do scanf ("%c", &c); while (!isdigit (c));
        val = val * 2 + (c - '0');
    }
    if (val >= ( (1 << len) - 1))  return;
    else
    {
        printf ("%c", key[len][val]);
        print();
    }
}

int main()
{
    int l1, l2, l3;
    while (gets (s) != NULL)
    {
        if (!s[0]) continue;
        memset (key, 0, sizeof (key));
        read();
        while (scanf ("%1d%1d%1d", &l1, &l2, &l3), len = 4 * l1 + 2 * l2 + l3)
            print ();
        printf ("\n");
    }
    return 0;
}
```