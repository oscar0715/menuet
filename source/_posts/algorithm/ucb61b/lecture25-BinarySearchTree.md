---
title: Lecture25 Binary Search Tree
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-07-14 22:06:30
---
Lecture25 Binary Search Tree

**图片引用写在最前面:**
{% link picture source https://segmentfault.com/a/1190000004407072 picture source%}

<!-- more -->

***

### Ordered dictionary 
dictionary in which keys have a total order, like a heap.

1. Insert Entries
2. Find
3. Remove
4. Quickly find entry with min or max key
5. or entry nearest another entry(key小于30的最大的那个entry)

### Binary search tree invariant 
For any node X, every key in `left subtree` of X is <= X's Key
For any node X, every key in `right subtree` of X is >= X's Key
父节点总比左子树大，右子树小

### Inorder Traversal of binary search tree
Visit in sorted order
基于上面的规则，中序遍历确实是顺序输出。

### Entry find( Object k )
{% codeblock lang:java  %}
public Object find(Object k) {

  BinaryTreeNode node = root;

  while (node != null) {
    int comp = ((Comparable) k).compareTo(node.entry.key());
    if (comp < 0) {
      node = node.left;
    } else if (comp > 0) {
      node = node.right;
    } else {
      return node.entry;
    }
  }
  return null;
}
{% endcodeblock %}

但是这并没有什么, hashmap更快
有趣的是: 
1. when search down a tree for a key k that is `not` in tree.
2. How to find the `smallest key >=k`,  (最后一次查左子树的那个点)
3. or `largest key <= k`;（最后一次查右子树的那个点）


### Entry first() / Entry last()

1. first(): 找最小的那个，简单地说，一路向左，知道有个节点没有左节点
2. last(): 同理一路向右

### Entry insert（ Object k, Object v）
和查找`find(Object k)`相同
当遇到null的时候，把这个entry放在null这个位置

如果有重复呢，老师说，随便，Let’s just go to left！哈哈哈哈

### Entry remove( Object k )
find a node n with key k

#### Return null if k is not in the tree

#### If n has no children
delete it  简单
<br/>![](http://ogy8sh1ok.bkt.clouddn.com/ucb61b/lecture25-BinarySearchTree/delete0.png)
<br/>

#### If n has one child
move child up to n's place 简单 
<br/>![](http://ogy8sh1ok.bkt.clouddn.com/ucb61b/lecture25-BinarySearchTree/delete2.png)
<br/>

#### 重点来了: If n has two children
1. Let x be node in n's right subtree with smallest key (一路向左)
2. 这个时候x是这棵n的右子树里最小的，所以它最多只有一个右子树，反正没有左子树。
3. 所以要把x remove掉很容易的。所以就把x remove掉，放到要删除的n的位置。
4. 如果x有右子树，按照前面的方法接上就好了。
5. 然后就神奇地结束了！因为x是在这整棵树里大于n且最接近n的
<br/>![](http://ogy8sh1ok.bkt.clouddn.com/ucb61b/lecture25-BinarySearchTree/delete3.png)
<br/>


学生还有一种solution就是把n的左子树接到x下面，老师说，可以，但是我不喜欢树太深  

对比一下二叉堆，二叉堆remove min操作的时候是把最后一个节点先放到最上面，再bubble down，有异曲同工之妙

### Running time
insert find remove 
proportion to the depth of the tree O(log n)

一个悲伤地故事是，如果是 1，2，3，4，5，6………………
这就会变成一个链表，然后就变成线性了

<br/>![](http://ogy8sh1ok.bkt.clouddn.com/ucb61b/lecture25-BinarySearchTree/list.png)
<br/>










