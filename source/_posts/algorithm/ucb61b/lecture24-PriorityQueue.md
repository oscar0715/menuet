---
title: Lecture24 Priority Queue
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-07-14 08:54:30
---
Lecture24 Priority Queue

**图片引用写在最前面:**
{% link picture source https://segmentfault.com/a/1190000004153707 picture source%}
<!-- more -->

***

### Priority Queue
类似于字典的结构
1. Entries contains a key an d a value
2. total order is defined on the key 
3. operations
    - identify or remove whose key is lowest,but not other entires
    只能找到最小的那个
    - insert at any time

用法：
1. event queue
2. key is time, value is the description of event

如果key相等，queue只会弹出一个

### Binary Heap: Implementation of Priority Queue

1. Binary Heap: `Complete*` binary tree(every row is full, except the lowest)
2. It is filled from left to right
3. `Heap -order Property`: no child has a key less than its parent's key
4. Stored as arrayof entries by `level order`` traversal
5. Mapping of nodes to indices: `level numbering`
`Node i's children is 2i & 2i+1, parent is i/2(只保留整数位)`
6. Each tree node has 2 reference(key, value), or references an "Entry" object

<br/>
![heap](http://ogy8sh1ok.bkt.clouddn.com/ucb61b/lecture24-PriorityQueue/heap1.png "heap")
<br/>

### Three Operations of Priority Queue

#### Entry min()
Just Return entry at root

#### Entry insert(Object k, Object v):

1. x: the new entry (k, v). 
2. place x in bottom level of tree, at first. Free spot `from left`
3. 此时，可能违反 Heap-order Property. Entry `Bubbles up`
4. Repeat
    - compare x's key with its parent's key
        - if x's key is `less`, exchange （相等则不用）

<br/>
![heap Insert](http://ogy8sh1ok.bkt.clouddn.com/ucb61b/lecture24-PriorityQueue/heapInsert.png "heap Insert")
<br/>

#### Entry removeMin() 重点在这里！

1. `remove entry at root`; save for return value;
2. fill the hole(the place of root) with the `last entry` x
3. `Bubble the x down` the heap
    - if x > one or both of its children
    - swap x with its `minmum child`

<br/>{% asset_img .png  %}
![heap Delete](http://ogy8sh1ok.bkt.clouddn.com/ucb61b/lecture24-PriorityQueue/heapDelete.png "heap Delete")
<br/>


### Running time
|                       | Binary Heap | Sorted List   |  Unsorted List  | 
| -----------           |:----------: | :------------:| ---------------:| 
| min()                 | O(1)        | O(1)          |   O(n)          |
| insert(worst-case)    | O(log n)    | O(n)          |   O(1)          |
| insert(best-case)     | O(1)        | O(1)          |   O(1)          |
| RemoveMin(worst-case) | O(log n)    | O(1)          |   O(n)          |
| RemoveMin(best-case)  | O(1)        | O(1)          |   O(n)          |

insert(worst)
1. taks O(1) time to compare x with its parent
2. complete binary  tree: has at most 1 + log(n) levels
3. O(log n) worst-case time;

### Bottom up heap construction

1. make a complete tree out of entries in any order
2. walk backward from `last internal node` to root; reverse order in array
3. when visit a node, bubble down as in removeMin()

中文翻译:
1. 建立一个完全二叉树，顺序随意。
2. 从最后一个非叶节点往root访问，层次遍历。
3. 每次访问一个节点的时候都做bubble down的动作

run time O(n)

<br/>
![heap Build](http://ogy8sh1ok.bkt.clouddn.com/ucb61b/lecture24-PriorityQueue/heapBuild.png "heap Build")
<br/>












