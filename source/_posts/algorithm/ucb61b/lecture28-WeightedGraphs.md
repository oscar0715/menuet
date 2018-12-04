---
title: Lecture28 Weighted Graphs
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-07-19 09:06:30
---
Lecture28 Weighted Graphs

<!-- more -->

***

## Weighted Graphs
Each `edge` labeled with a numertical `weight`.

Adjacency matrix: array of ints/doubles/whatever

Adjacency list: Each listnode includes a weight field.

Problem: "shortest path problem"

"Minimum spanning tree" 

Each node is an outlet, or soure of electricity.

Edges labeld with length of wire.

Connect all nodes with shortest length of wires.

### Kruskal's Algorithm
最小生成树
克鲁斯卡算法

G = (V, E) undirected graph

Spanning tree: T = (V, F) of G is a graph   (G T 点相同，边不同)
Normally T has |V|-1 edges of T that form a tree.

If G is not connected, then T is a forest, a collect of trees.

If G is weighted, a "minimum spanning tree" (没打完，反正权重和也是最小的)

1. Creat a new Tree T 点都和G一样，但是没有边
2. Make list of all edges in G
3. `Sort` edges by weight, `lowest to highest`. 
4. Iterate through edges in sorted order. For each edge(u, w): 
  - if u & w are not connected by `path` in T, add (u, w) to T  注意这里是path
  - Never adds (u, w) is same path connects u & w, T is guaranteed to be a tree(if G is connected) or a forest.

就是每次都找最小的连，如果已经通了，就不连

Running time 
1. 前两步 O(|V|)
2. 第三步 Sort |E| edges takes O(|E| log |E|) 
3. 第四步 < O(|E| log |E|) 老师说后面会讲到
4. 总体而言 O(|V| + |E| log |E|) = O(|V| + |E| log |V^2|) = O(|V| + |E| log |V|)


***












