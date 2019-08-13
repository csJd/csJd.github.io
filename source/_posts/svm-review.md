---
title: SVM Review
mathjax: true
tags:
  - Machine Learning
categories:
  - Review
date: 2019-08-11 07:45:01
---

## 支持向量机的基本型

支持向量机（SVM）是一种二类分类模型，其学习的基本想法是求解能够正确划分训练数据集并且间隔最大的划分超平面。

<!--more-->

划分超平面 $(\mathbf{w}, b)$ 可以由线性方程 $\mathbf{w}^T\mathbf{x} + b = 0$ 来描述，令

$$
  y_i(\mathbf{w}^T\mathbf{x}_i + b) \ge 1, \quad i = 1,..., m, y_i \in \{-1, 1\}
  \tag{0}
$$

若超平面 $(\mathbf{w}_t, b_t)$ 能将训练样本正确分类，则总存在缩放变换 $\mathbf{w}^T=\varsigma \mathbf{w}^T_t, b = \varsigma b_t$ 使得上式成立

此时的间隔 $\gamma$ 定义为 $\gamma = \frac{2}{\| \mathbf{w} \|}$，使间隔最大化，即对以下优化问题求解：
$$
  \begin{aligned}
    \underset{\mathbf{w},b}{max} \quad & \frac{2}{\Vert \mathbf{w}\Vert} \\
    s.t. \quad & y_i(\mathbf{w}^T\mathbf{x}_i+b) \ge 1, \quad i = 1, ..., m
  \end{aligned}
  \tag{1}
$$

这等价于：

$$
  \begin{aligned}
    \underset{\mathbf{w},b}{min} \quad & \frac{1}{2}{\Vert \mathbf{w}\Vert}^2 \\
    s.t. \quad & y_i(\mathbf{w}^T\mathbf{x}_i+b) \ge 1, \quad i = 1, ..., m
  \end{aligned}
  \tag{2}
$$

这是支持向量机的基本型，对求解式 (2) 使用 [拉格朗日乘数法](https://zh.wikipedia.org/wiki/%E6%8B%89%E6%A0%BC%E6%9C%97%E6%97%A5%E4%B9%98%E6%95%B0) 有：
$$
  L(\mathbf{w}, b, \alpha) = \frac{1}{2}{\Vert \mathbf{w}\Vert}^2+\sum_{i=1}^m\alpha_i(1-y_i(\mathbf{w}^T\mathbf{x}_i+b))
  \tag{3}
$$

令 $L$ 对 $\mathbf{w}, b$ 的偏导为零得

$$
  \begin{aligned}
    \mathbf{w} &= \sum_{i=1}^m\alpha_iy_i\mathbf{x}_i\\
    0 &= \sum_{i=1}^m\alpha_iy_i
  \end{aligned}
  \tag{4}
$$

代入式（3）将 $\mathbf{w}$ 和 $b$ 消去有

$$
  \begin{aligned}
    \underset{\mathbf{w},b}{min}\ L(\mathbf{w},b,\alpha) &= \frac{1}{2}\sum_{i=1}^{m}\sum_{j=1}^m \alpha_i\alpha_jy_iy_j(\mathbf{x}_i^T\mathbf{x}_j) - \sum_{i=1}^m\alpha_iy_i\left(\left(\sum_{j=1}^m\alpha_jy_j\mathbf{x}_j\right)\mathbf{x}_i + b\right) + \sum_{i = 1}^m\alpha_i\\
    &=-\frac{1}{2}\sum_{i=1}^{m}\sum_{j=1}^m \alpha_i\alpha_jy_iy_j(\mathbf{x}_i^T\mathbf{x}_j) + \sum_{i=1}^m\alpha_i
  \end{aligned}
  \tag{5}
$$

得到式（2）的对偶问题：

$$
  \begin{aligned}
    \underset{\alpha}{max} \quad & -\frac{1}{2}\sum_{i=1}^{m}\sum_{j=1}^m \alpha_i\alpha_jy_iy_j(\mathbf{x}_i^T\mathbf{x}_j) + \sum_{i=1}^m\alpha_i \\
    s.t.\quad
    & \sum_{i=1}^m\alpha_iy_i = 0, \\
    & \alpha_i \ge 0, \quad i = 1, ..., m
  \end{aligned}
  \tag{6}
$$

解出 $\alpha$ 后求出 $\mathbf{w}$ 与 $b$ 即可得到模型 $f(\mathbf{x}) = \mathbf{w}^Tx + b = \sum_{i=1}^m\alpha_iy_i(\mathbf{x}_i^T\mathbf{x}) + b$

可以使用 SMO 算法来进行求解，SMO 不断执行如下两个步骤直到收敛：

* 选取一对需要更新的变量 $\alpha_i$ 和 $\alpha_j$
* 固定 $\alpha_i$ 和 $\alpha_j$ 以外的参数，求解式（3）更新后的 $\alpha_i$ 和 $\alpha_j$

## 核函数

原始的特征空间的也许并不存在一个能正确划分两类样本的超平面，此时存在一个更高维度的空间，将原始空间中的样本映射到高维度空间后是线性可分的，令 $\phi (\mathbf{x})$ 表示将 $\mathbf{x}$ 映射到高维空间后的特征向量。类似上面的基本型，可以得到此情况下的对偶问题：

$$
  \begin{aligned}
    \underset{\alpha}{max} \quad & -\frac{1}{2}\sum_{i=1}^{m}\sum_{j=1}^m \alpha_i\alpha_jy_iy_j\phi(\mathbf{x}_i)^T\phi(\mathbf{x}_j) + \sum_{i=1}^m\alpha_i \\
    s.t.\quad
    & \sum_{i=1}^m\alpha_iy_i = 0, \\
    & \alpha_i \ge 0, \quad i = 1, ..., m
  \end{aligned}
  \tag{7}
$$

直接计算 $\phi(\mathbf{x}_i)\cdot\phi(\mathbf{x}_j)$ 可能会很困难，设想有这样的函数：

$$
  \kappa(\mathbf{x}_i, \mathbf{x}_j) = \langle\phi(\mathbf{x}_i), \phi(\mathbf{x}_j)\rangle =\phi(\mathbf{x}_i)^T\phi(\mathbf{x}_j)
  \tag{8}
$$

即 $\mathbf{x}_i$ 与 $\mathbf{x}_j$ 映射到高维空间后的内积等于在原始特征空间中通过函数 $\kappa(.,.)$ 计算的结果，函数 $\kappa(.,.)$ 称作 [核函数](https://en.wikipedia.org/wiki/Positive-definite_kernel)。

## 软间隔

往往很难确定合适的核函数是的训练样本严格线性可分，软间隔 SVM 允许某些样本不满足约束，在最大化间隔的同时，不满足约束的样本应尽可能少。使用 hinge 损失函数时，可以得到此条件下的优化目标

$$
  \begin{aligned}
    \underset{\mathbf{w},b}{min} \quad & \frac{1}{2}{\|\mathbf{w}\|}^2 + C\sum_{i=1}^mmax(0, 1-y_i(\mathbf{w}^T\mathbf{x}_i + b)),\quad i = 1, ..., m
  \end{aligned}
  \tag{9}
$$

若引入松弛变量 $\xi_i \ge 0$，可将式（9）重写为

$$
  \begin{aligned}
    \underset{\mathbf{w},b,\xi_i}{min} \quad & \frac{1}{2}{\|\mathbf{w}\|}^2 + C\sum_{i=1}^m\xi_i\\
    s.t. \quad & y_i(\mathbf{w}^T\mathbf{x}_i+ b) \ge 1-\xi_i\\
    &\xi_i \ge 0, \quad i = 1, ..., m
  \end{aligned}
  \tag{10}
$$
这就是软间隔支持向量机。

## 与 LR (Logistic Regression) 的区别

* 二者的 Loss function 可以表示为一种类似的形式
  $$
    \begin{aligned}
      LR:\quad &\lambda \|\mathbf{w}\|^2 + \sum_{i=1}^mlog(1 + exp(-y_i\mathbf{w}^T\mathbf{x}_i))\\
      SVM:\quad & \lambda \|\mathbf{w}\|^2 + \sum_{i=1}^mmax\{0, 1-y_i\mathbf{w}^T\mathbf{x}_i\}
    \end{aligned}
  $$
  其中 $y_i \in \{-1, 1\}$，LR 是 Logistic Loss， SVM 是 Hinge Loss

* SVM 只考虑支持向量（使不等式等号成立的点），LR 考虑所有点；SVM 不依赖于数据分布， LR 受数据分布影响
