---
title: Lecture27 Graphs
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-07-18 09:06:30
---
Lecture27 Graphs

<!-- more -->

***

## Graphs
A graph G is a set V of vertices and a set E of edges that connect vertices.

G = (V, E)

2 Types
- directed 
- undirected

### Directed Graphs
e = (v, w)  ———— ordered pair, v is the origin and w is the destination.

### undirected Graphs
(v, w) = (w, v)  ————  unordered pair


self-edge (v, v)

### Path
sequence of vertices with each adjcent pair connected by an edge.

### Length of path 
numbers of edges 
<4,5,6,3> ———— length is 3
<2> ———— length is 0

### Strongly connected
There is a path from any vertex to any other vertex 
(Called connected in undirected graph)

### Degree of a vertex 
Numbers of edges incident an vertex.
(self-edges count as one)

digraph
1. Indegree
2. Outdegree

## Graph Representation
<br/>![](http://ogy8sh1ok.bkt.clouddn.com/ucb61b/lecture27-Graphs/graph1.gif)<br/>

### Adjacency matrix:
|v| - by - |v| array of booleans

二维数组

斜对角线对称（i, j） = (j, i)

最大edge的数量
directed = |v|^2
undirected = |v|^2 / 2

planer graphs has O(|v|) edges. 能在二维平面展示，不交叉的图，能有线性的数量的edges

sparse Graph 稀疏的图

如果是String，先Hash String 到 int  // 等等，这里没明白，老师说会collision是什么情况

### Adajcency List
Each vertex has a linkedList of edges out

1. Vertices are consecutive integers
Use Array of Lists (`ArrayList<List<Integer>`)
2. Vertices has name "String" 
Use Hashtable to map Strings to List (`HashMap<List<String>>`)
Key: vertex name
Value: List

If it is a sparce gragh, then Adjacent List 时间空间上都更省


## Graph traversal

防止重复visit
每个点有个一个 Boolean 'visited' field 

### Depth-First serach DFS
{% codeblock lang:java  %}
public void dfs(Vertex u){
    u.visit();
    u.visited;

    // 访问和u相连的每个v点
    for(each vertex v such that in the (u,v) ){
        if(!v.visited){
            // 迭代
            dfs(v);
        }
    }
}
{% endcodeblock %}

Runs in O(|V|+|E|) time using Adjacent List
Runs in O(|V|^2) time using Adjacent Matrix

### Breadth-First search BFS
Queue
{% codeblock lang:java  %}
public class Vertex {
  Vertex parent;
  int depth;

  public void visit(Vertex origin) {
    this.parent = origin;
    if (origin == null) {
      this.depth = 0;
    } else {
      this.depth = origin.depth + 1;
    }
  }
}

public void bfs(Vertex u) {
  u.visit(null);
  u.visited = true;

  q = new Queue();
  q.enqueue(u)

  while (!q.empty()) {
    v = q.dequeue(); // 取出q最前面那个，做该点的bfs
    for ( Each vertex w such that (v, w) is an edge ) {
      if (!w.visited) {
        w.visit(v);
        w.visited = true;
        q.enqueue(w); // 取出w的时候会访问与它相连的点
      }
    }
  }
}
{% endcodeblock %}

when edge (v, w) is traversed to visit w, depth of w is depth of v + 1, and v becomes the "parent" of w.

Find `shortest path `  `from any vertex to start ` vertex by following parent pointers

run time
- Adjacency list in O(|v|+|e|)
- Adjacency list in O(|v|^2)

***












