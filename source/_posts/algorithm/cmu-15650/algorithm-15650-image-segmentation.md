---
title: CMU 15650 - Image Segmentation
mathjax: true 
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-11-5 20:42:45
---
CMU 15650 - Algorithm and Data Structure 

Image Segmentation
<!-- more -->

***

# Problem
给定一张图片，判断图片中的一个像素是前景像素还是背景像素。

因为在不同的应用当中，判断一个像素是前景还是背景像素的标准是不一样的，所以我们要对这个标准进行抽象。
我们设定每一个像素$i$都有两个两个非零值：
1. $a_i$ : 像素 $i$ 是前景像素的可能性
2. $b_i$ : 像素 $i$ 是背景像素的可能性

当然这两个值都是由具体的应用决定的。

判断标准：
1. 当 $a_i > b_i$，那么我们可以判定 $a_i$ 是前景像素
2. 如果一个像素是前景像素，那么它旁边的像素是前景像素的可能性更大

我们用无向图 $G=(V,E)$ 来表示相邻像素的关系：
1. $V$ 是代表每个像素的顶点
2. 如果两个像素相邻，那么两个像素之间就有一条边 $e$ ，$e \in E$
3. 在这个模型中增加一个参数，{% raw %}$p_{ij}${% endraw %}。
  对于每一对相邻的像素 $i$，$j$ ，如果其中一个像素是前景像素，而另一个像素的是背景像素，那么 $p_{ij}$ 就是代表着中情况下的惩罚量。

由此我们可以得到*图像分割问题*:
把图中的所有像素分类至前景像素或背景像素，并使下列值最大
{% raw %}
$$q(A,B) = \sum_{i \in A} a_i + \sum_{j \in B} b_j - \sum_{(i,j) \in E \text{, i, j sep}} p_{ij} $$
{% endraw %}

# Analysis
**Image Segmentation 图像分割**
把图像中的像素，分到两个集合 $A$ , $B$ 中，使 $q(A,B)$ 达到最大。

我们定义 $Q = \sum_i (a_i + b_i)$ ，也就是图中所有像素的所有的 $a_i$，$b_i$ 的和。

那么有： 
{% raw %}
$\sum_{i \in A} a_i + \sum_{j \in B} b_j = Q - (\sum_{i \in A} b_i + \sum_{j \in B} a_j )$
{% endraw %}

所以：
{% raw %}
$q(A,B) = Q - \sum_{i \in A} b_i - \sum_{j \in B} a_j - \sum_{(i,j) \in E \text{, i, j sep}} p_{ij} $
{% endraw %}

换言之，我们把求最大值的问题转换为求最小值的问题：
{% raw %}
$q'(A,B) = \sum_{i \in A} b_i + \sum_{j \in B} a_j + \sum_{(i,j) \in E \text{, i, j sep}} p_{ij} $
{% endraw %}


**Minimum Cut 最小分割：**
我们把一个有向图的顶点分到分到两个集合 $A$, $B$ 中，同时这个有向图有一个出发点 $s \in A$ 和一个终点 $t \in B$ ，使得跨越 $A$ 和 $B$ 的的边的权重最小。

我们可以看到图像分割问题和最小分割问题有相似之处，但是有以下不同：
1. 一个是求最大值，另一个求最小值
2. 图像分割没有出发点（source）和终点（sink）
3. 图像分割问题是无向图，最小分割问题是有向的

那么我们可以把一个图像分割问题转化为最小分割问题
1. 在图像分割问题中，加入$s \in V'$和$t \in V'$，
$s$ 指向每一个像素，也就是有边 $(s,u) \in E'$，$s$ 代表前景。这条边的的 weight 为 $a_i$
每一个像素指向 $t$ ，也就是有边 $(u,t) \in E'$ ， $t$ 代表背景。这条边的的 weight 为 $b_i$
2. 其他的各个像素之间添加两条互相指向的边, weight 为 $p_{ij}$ 

在一个$ cut(A,B)$ 中，从 $s$ 到 $t$ 的流量就由 A，B 来决定：
1. A 是属于前景的像素的集合
2. B 是属于背景的像素的集合

![Minimum Cut Problem](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4295505/bin/nihms546476f1.jpg "Minimum Cut Problem")

我们来分析
{% raw %}
$q'(A,B) = \sum_{i \in A} b_i + \sum_{j \in B} a_j + \sum_{(i,j) \in E \text{, i, j sep}} p_{ij} $
{% endraw %}

1. $\sum_{i \in A} b_i$
属于A的像素，我们就加上$b_i$
2. $\sum_{j \in B} a_j$
属于B的像素，我们就加上$a_j$
3. {% raw %}$\sum_{(i,j) \in E \text{, i, j sep}} p_{ij} ${% endraw %}
如果邻点$i$,$j$属于不同的集合，我们就加上$p_{ij} $

所以我们把一个图像分析问题转化为了求最小的分割的问题，也就是求最大流的问题。

 