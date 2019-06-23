---
title: UVa 1339 Ancient Cipher
author: Deng
tags: 
       - Algorithm

category: 
       - OJ
       - String

date: 2014-09-02 09:50:13
---
Ancient Cipher

Ancient Roman empire had a strong government system with various departments, including a secret service department. Important documents were sent between provinces and the capital in encrypted form to prevent eavesdropping. The most popular ciphers in those times were so calledsubstitution cipherandpermutation cipher. Substitution cipher changes all occurrences of each letter to some other letter. Substitutes for all letters must be different. For some letters substitute letter may coincide with the original letter. For example, applying substitution cipher that changes all letters from `A' to `Y' to the next ones in the alphabet, and changes `Z' to `A', to the message ``VICTORIOUS'' one gets the message ``WJDUPSJPVT''. Permutation cipher applies some permutation to the letters of the message. For example, applying the permutation![$ \langle$](../images/dge.org-external-13-3213img1-.png)2, 1, 5, 4, 3, 7, 6, 10, 9, 8![$ \rangle$](../images/dge.org-external-13-3213img2-.png)to the message ``VICTORIOUS'' one gets the message ``IVOTCIRSUO''. It was quickly noticed that being applied separately, both substitution cipher and permutation cipher were rather weak. But when being combined, they were strong enough for those times. Thus, the most important messages were first encrypted using substitution cipher, and then the result was encrypted using permutation cipher. Encrypting the message ``VICTORIOUS'' with the combination of the ciphers described above one gets the message ``JWPUDJSTVP''. Archeologists have recently found the message engraved on a stone plate. At the first glance it seemed completely meaningless, so it was suggested that the message was encrypted with some substitution and permutation ciphers. They have conjectured the possible text of the original message that was encrypted, and now they want to check their conjecture. They need a computer program to do it, so you have to write one.

##

Input file contains several test cases. Each of them consists of two lines. The first line contains the message engraved on the plate. Before encrypting, all spaces and punctuation marks were removed, so the encrypted message contains only capital letters of the English alphabet. The second line contains the original message that is conjectured to be encrypted in the message on the first line. It also contains only capital letters of the English alphabet. The lengths of both lines of the input file are equal and do not exceed 100.

##

For each test case, print one output line. Output ` YES ' if the message on the first line of the input file could be the result of encrypting the message on the second line, or ` NO ' in the other case.

##

JWPUDJSTVP VICTORIOUS MAMA ROME HAHA HEHE AAA AAA NEERCISTHEBEST SECRETMESSAGES

##

YES NO YES YES NO

题意 给两个字符串a，b 能否把b重排后再把每个字母映射到另一个字母得到a 由于可以重排 出现的顺序就不用考虑了 只要分别把ab中每个字母出现的次数存在数组中 再对数组排序 要是得到的两个数组完全相同b就能转换为a了；

```js 
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int N = 105;
char a[N], b[N];

int main()
{
    while (scanf ("%s%s", a, b) != EOF)
    {
        int ca[26] = {0}, cb[26] = {0}, l = strlen (a), i;
        for (i = 0; i < l; ++i)
        {
            ++ca[a[i] - 'A'];
            ++cb[b[i] - 'A'];
        }
        sort (ca, ca + 26);
        sort (cb, cb + 26);
        for (i = 0; i < 26; ++i)
            if (ca[i] != cb[i]) break;
        printf (i == 26 ? "YES\n" : "NO\n");
    }
    return 0;
}
```