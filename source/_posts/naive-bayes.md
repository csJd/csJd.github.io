---
title: Naive Bayes Review
mathjax: true
tags:
  - Machine Learning
categories:
  - Review
date: 2019-09-16 08:32:01
---

笔试题中遇到了 [朴素贝叶斯算法](https://en.wikipedia.org/wiki/Naive_Bayes_classifier) 的实现，这里对朴素贝叶斯算法做个简单回顾。

<!--more-->

## 朴素贝叶斯算法

朴素贝叶斯算法是一个相对比较简单的机器学习算法，是基于贝叶斯定理和特征独立假设的分类方法。

对于给定的训练集（设共 $m$ 条训练数据，$n$ 种特征，$t$ 种类别 $0, 1, ..., t-1$)

可以先得到各个类别的先验概率分布：

$$
P(Y=k)= |\{y_i = k \}|/m, \quad i=0, 1, ..., m-1 \tag{0}
$$

在特征条件独立性假设下， 有条件概率分布：

$$
P(X=x|Y=k)=\prod_{j=0}^{n-1}P(X^j=x^j|Y=k) \tag{1}
$$

根据贝叶斯定理可以得到后验概率分布：
$$
P(Y=k|X=x)=\frac{P(Y=k)\prod_jP(X^j=x^j|Y=k)}{\sum_kP(Y=k)\prod_jP(X^j=x^j|Y=k)}
\tag{2}
$$
使得后验概率最大的类别即分类器需要输出的类别，对于输入 $x$，式 $(2)$ 的分母对于所有类别都是相同的，所以朴素贝叶斯分类器可表示为：
$$
y=f(x) =\underset{k}{argmax}P(Y=k)\prod_jP(X^j=x^j|Y=k)
\tag{3}
$$

## 笔试题

> 朴素贝叶斯分类器假设在给定样本 label 的情况下，样本的不同特征之间相互独立。
> 现用朴素贝叶斯分类器进行垃圾邮件识别，数据包含 4 个特征。
> 现在将所有的特征进行转换后，得到下表(请在程序以硬编码方式读入);
> 转换规则如下: (注: [m,n]表示m,n之间的闭区间，[m,+]表示大于m的开区间)
>
> 标题长度(feature1) : 1: [0,3], 2: [3,6]，3: [6,+] </br>
> 正文长度(feature2) : 1: [0,10], 2: [10,20], 3: [20,+] </br>
> 附件含有可执行程序(feature3) : 1:是，0:否 </br>
> 正文含特殊字符(feature4) : 1:是，0:否 </br>
>
> 请在程序中读入上述训练数据，实现朴素贝叶斯分类器，语言不限，但不能使用第三方库，
> 不需要考虑平滑方法，然后对给定的测试数据(特征已转换)进行预测，输出结果;
>
> 输入描述： </br>
> 输入数据如下，第一行一个数字 M，表示共有 M 行训练数据，
> 第 2~M+1 行，每行 5 个数字，分别以空格隔开，前四个数字分别代表四个特征，第
> 5 个数字代表这一个样本 label 值。
> 第 M+2 行是一个数字 N，表示共有 N 行测试样本，随后的 N 行每行 4 个数字，分别代表四个特征的值。
>
> 输出描述：</br>
> 使用贝叶斯模型对测试样本进行预测，所有结果按顺序输出到一行，以空格分隔;

对应该笔试题的 Python 代码：

```python
"""
输入示例：
14
1 1 1 0 1
1 1 1 1 1
2 1 1 0 0
3 2 1 0 0
3 3 0 0 0
3 3 0 1 1
2 3 0 1 0
1 2 1 0 1
1 3 0 0 0
3 2 0 0 0
1 2 0 0 0
2 2 1 1 0
2 1 0 0 0
3 2 1 1 1
5
1 1 0 0
1 1 1 0
1 2 1 0
2 1 0 1
2 2 1 1

输出示例：
0 1 1 0 0
"""

def main():
    m = int(input())

    n_values = [3, 3, 2, 2]
    n_features = 4
    n_classes = 2

    X = list()
    cnt = [0] * n_classes
    for i in range(m):
        X.append(list(map(int, input().split())))
        X[-1][0] -= 1  # x[0], x[1] are from 1 here
        X[-1][1] -= 1
        cnt[X[-1][-1]] += 1  # X[-1][-1] is y
    p_classes = [X / m for X in cnt]  # P(y=class[i])

    # p_values[j][k][v] save the proba of P(Xj=v|y=k)
    p_values = []
    for j in range(n_features):
        # save value counts in each class of the j-th feature
        value_counts = [[0] * n_values[j] for y in range(n_classes)]
        for i in range(m):
            y = X[i][-1]
            value_counts[y][X[i][j]] += 1

        # p_values[j][k][v] save the proba of P(Xj=v|y=k)
        p_values.append(
            [[value_counts[k][v]/cnt[k] if cnt[k] else 0
              for v in range(n_values[j])]
             for k in range(n_classes)]
        )

    ans = list()
    n = int(input())
    for line in range(n):
        xi = list(map(int, input().split()))
        xi[0] -= 1
        xi[1] -= 1
        y_pred, maxp = 0, 0
        for k in range(n_classes):
            p = 1
            # to calclate P(x=xi|y=k)
            for j in range(n_features):
                v = xi[j]
                p *= p_values[j][k][v]
            p *= p_classes[k]
            if p > maxp:
                y_pred, maxp = k, p
        ans.append(y_pred)

    print(' '.join(map(str, ans)))


if __name__ == "__main__":
    main()
```


