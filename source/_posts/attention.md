---
title: Something about Attention
mathjax: true
tags:
  - Machine Learning
categories:
  - Review
date: 2019-08-13 18:47:30
---

A summarization of Attention mechanism. Supporsing that we have hidden state representations $H \in \mathbb{R}^{n*d}$, where $n$ is the length of sequence, $d$ is the dimension of hidden layer representation of each token.

<!--more-->
<!--Supporsing that we have hidden state representations $H \in \mathbb{R}^{n*d}$, where $n$ is the length of sequence, $d$ is the dimension of hidden layer representation of each token.
-->

## In Seq2Seq

[Bahdanau, D., Cho, K., & Bengio, Y. (2014). Neural machine translation by jointly learning to align and translate. ICLR 2015](https://arxiv.org/abs/1409.0473)

$$
  \begin{aligned}
    c_i &= \sum_{j=1}^{T_x}\alpha_{ij}h_j \\
    \alpha_{ij} &= \frac{\exp{(e_{ij})}} {\sum_{k=1}^{T_x} \exp(e_{ik})} \\
    e_{ij} &= a(s_{i-1},h_j)
  \end{aligned}
$$

[Luong, M. T., Pham, H., & Manning, C. D. (2015). Effective approaches to attention-based neural machine translation. EMNLP 2015](https://aclweb.org/anthology/D15-1166)

$$
\begin{aligned}
  & a_t(s) = align(h_t, h_s) = \frac{\exp(score(h_t, h_s))}{\sum_{s'}\exp(score(h_t, h_{s'}))}\\
  &score(h_t, h_s)=
    \begin{cases}
      {h_t}^Th_s    & \quad \text{dot}\\
      {h_t}^TW_ah_s & \quad \text{general}\\
      W_a[h_t;h_s]  & \quad \text{concat}
    \end{cases}
  \end{aligned}
$$

[More details here.](https://jalammar.github.io/visualizing-neural-machine-translation-mechanics-of-seq2seq-models-with-attention/)

## In Relation Classification

[Zhou, P., Shi, W., Tian, J., Qi, Z., Li, B., Hao, H., & Xu, B. (2016). Attention-based bidirectional long short-term memory networks for relation classification. ACL 2016](https://www.aclweb.org/anthology/P16-2034)

$$
  \begin{aligned}
    M &= \tanh(H^T)\\
    \alpha &= softmax(w^TM)\\
    r &= H\alpha^T
  \end{aligned}
  \quad
  \implies
  \quad
  \begin{aligned}
  A &= \tanh(H) \quad & (A \in \mathbb{R}^{n\times d})\\
  \alpha &= softmax(Aw) \quad & (w \in \mathbb{R}^{d}, \alpha \in \mathbb{R}^{n})\\
  r &= \alpha^TH \quad & (r \in \mathbb{R}^{d})
  \end{aligned}
$$

* input: $H \in \mathbb{R}^{n\times d}$
* parameters to train: $w \in \mathbb{R}^{d}$
* output: sentence representation $r \in \mathbb{R}^d$

## HAN in Text Classification

[Yang, Z., Yang, D., Dyer, C., He, X., Smola, A., & Hovy, E. (2016). Hierarchical attention networks for document classification. NAACL 2016](https://www.aclweb.org/anthology/N16-1174)

$$
  \begin{aligned}
    u_{it} &= \tanh(W_wh_{it} + b_w)\\
    \alpha_{it} &= \frac{\exp({u_{it}}^Tu_w)} {\sum_t \exp({u_{it}}^Tu_w)}\\
    s_i &= \sum_t \alpha_{it}h_{it}
  \end{aligned}
  \quad
  \implies
  \quad
  \begin{aligned}
  A &= \tanh(HW+b) \quad & (A \in \mathbb{R}^{n\times d_w})\\
  \alpha &= softmax(Aw) \quad & (w \in \mathbb{R}^{d_w}, \alpha \in \mathbb{R}^{n})\\
  r &= \alpha^TH \quad & (r \in \mathbb{R}^{d})
  \end{aligned}
$$

* input: $H \in \mathbb{R}^{n\times d}$
* parameters to train: $W \in \mathbb{R}^{d\times d_w}$, $u_w \in \mathbb{R}^{d_w}$
* output: sentence representation $r \in \mathbb{R}^d$

## Self-Attention in Transformer

[Vaswani, A., Shazeer, N., Parmar, N., Uszkoreit, J., Jones, L., Gomez, A. N., ... & Polosukhin, I. (2017). Attention is all you need. NIPS 2017](http://papers.nips.cc/paper/7181-attention-is-all-you-need)

$$
  \begin{aligned}
    Q &= HW_Q \\
    K &= HW_K \\
    V &= HW_V \\
    H'&= softmax(\frac{QK^T}{\sqrt{d_k}}) V
  \end{aligned}
$$

* input: $H \in \mathbb{R}^{n\times d}$
* parameters to train: $W_Q, W_K, W_V \in \mathbb{R}^{d\times d_k}$
* output: next hidden layer representation $H' \in \mathbb{R}^{n\times d_k}$

[More details here.](http://jalammar.github.io/illustrated-transformer/)
