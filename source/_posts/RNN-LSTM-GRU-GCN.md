---
title: RNN/LSTM/GRU/GCN
tags:
  - 算法
categories:
  - 算法
keywords:
  - 算法
description: 多智能体强化学习的算法和思路总结
abbrlink: 17765
date: 2023-12-21 19:51:33
updated: 2023-12-21 19:51:33
top_img: https://s2.loli.net/2024/02/06/U5SXRYACVolptGM.png
comments:
cover: https://s2.loli.net/2024/02/06/3uoiqZOIw6CPygm.png
toc:
toc_number:
toc_style_simple:
copyright:
copyright_author:
copyright_author_href:
copyright_url:
copyright_info:
mathjax:
katex:
aplayer:
highlight_shrink:
aside:
abcjs:
---

> 最近对强化学习算法和预测结合有些想法，阅读了下面两篇文章对之有了大致认识。

> 参考：https://zhuanlan.zhihu.com/p/587276701
> 参考：https://zhuanlan.zhihu.com/p/605750441

## 循环神经网络RNN
传统的神经网络只能单独的取处理一个个的输入，前一个输入和后一个输入是完全没有关系的。但是，某些任务需要能够更好的处理序列的信息，即前面的输入和后面的输入是有关系的。

由此诞生了RNN。

循环神经网络（Recurrent Neural Network, RNN）是一类以序列（sequence）数据为输入，在序列的演进方向进行递归（recursion）且所有节点（循环单元）按链式连接的递归神经网络。

![img_1.png](..%2Fimage%2FRNN-LSTM-GRU-GCN%2Fimg_1.png)

```
x 输入层 向量
U 输入层到隐藏层的 权重矩阵
s 隐藏层 向量
V 隐藏层到输出层的 权重矩阵
o 输出层 向量
W 上一次隐藏层的值作为这一次输入的权重 权重矩阵
```
循环神经网络的隐藏层的值s不仅取决于当前这次的输入x，还取决于上一次隐藏层的值s。

![img_2.png](..%2Fimage%2FRNN-LSTM-GRU-GCN%2Fimg_2.png)


具体的抽象图：
![img.png](..%2Fimage%2FRNN-LSTM-GRU-GCN%2Fimg.png)

## 长短期记忆网络LSTM

长短期记忆（Long short-term memory, LSTM）是一种特殊的RNN，主要是为了解决长序列训练过程中的梯度消失和梯度爆炸问题。简单来说，就是相比普通的RNN，LSTM能够在更长的序列中有更好的表现。

![img_3.png](..%2Fimage%2FRNN-LSTM-GRU-GCN%2Fimg_3.png)
(左：RNN，右：LSTM)



