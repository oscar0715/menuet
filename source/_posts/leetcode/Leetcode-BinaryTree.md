---
title: Leetcode_BinaryTree
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-05 16:13:00
---
二叉树基础总结
<!-- more -->

***

### 二叉树结构
``` java
//  Definition for a binary tree node.
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;

    TreeNode(int x) {
        val = x;
    }
}
```

### 二叉树的递归遍历
``` java
//前序遍历 parent --> left --> right
public void recursePreOrder(TreeNode root) {
    if (root ==null) {
        return;
    }

    System.out.println(root.val);
    
    recursePreOrder(root.left);
    recursePreOrder(root.right);
}
```

``` java
// 中序遍历 left --> parent --> right
public void recurseInOrder(TreeNode root) {
    if (root ==null) {
        return;
    }
    recurseInOrder(root.left);

    // visit
    System.out.println(root.val);
    
    recurseInOrder(root.right);
}
```

``` java
// 后序遍历 left --> right --> parent 
public void recursePostOrder(TreeNode root) {
    if (root ==null) {
        return;
    }
    recursePostOrder(root.left);
    recursePostOrder(root.right);

    // visit
    System.out.println(root.val);
}
```

### 二叉树的非递归遍历
``` java
// 前序遍历 parent --> left --> right
public void iterativePreOrder(TreeNode root) {
    if (root == null) {
        return;
    }
    Stack<TreeNode> stack = new Stack<TreeNode>();
    stack.push(root);
    while (!stack.isEmpty()) {
        TreeNode node = stack.pop();
        // 访问父节点
        System.out.println(node.val);
        // 先压入右子树，后出
        if (node.right != null) {
            stack.push(node.right);
        }
        // 后压入左子树，先出
        if (node.left != null) {
            stack.push(node.left);
        }
    }
}
```

``` java
// 中序遍历 left --> parent --> right
public void iterativeInorder(TreeNode p) {
    Stack<TreeNode> stack = new Stack<TreeNode>();
    while (p != null || !stack.empty()) {
        
        if (p.left != null) {
            // 如果节点的左子树非空，先将节点压栈
            stack.push(p);
            // 将左子树作为当前节点。
            p = p.left;
        } else {
            // 如果左子树为空，那么可以输出当前节点
            System.out.println("[p] = " + p.val);
            
            p = p.right;
            
            // 将右子树作为当前节点
            // 如果当前节点为空，进入内循环
            while (p == null && !stack.empty()) {
                // 如果当前结点为空，意味着，这一个分支都走完了
                // 从栈里弹出最上方的节点
                p = stack.pop();
                // 输出节点，因为这个节点是栈里弹出来的，所以该节点的左子树一定都走完了
                System.out.println("[p] = " + p.val);
                p = p.right;
            }
            // 如果当前节点不为空，那么进入下一轮外循环
        }
    }
}
```

这个比较简单！
``` java
// 中序遍历 left --> parent --> right
public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> list = new ArrayList<Integer>();
    Stack<TreeNode> stack = new Stack<TreeNode>();
    while (root != null || !stack.isEmpty()) {
        if (root != null) {
            stack.push(root);
            root = root.left;
        } else {
            root = stack.pop();
            list.add(root.val);
            root = root.right;
        }
    }
    return list;
}
```

``` java
/** 非递归实现后序遍历 */  
protected static void iterativePostorder(Node p) {  
    Node q = p;  
    Stack<Node> stack = new Stack<Node>();  
    while (p != null) {  
        // 左子树入栈  
        while (p.left != null)  
            stack.push(p);  
            p = p.left
            r
        // 当前节点无右子或右子已经输出  
        while (p != null && (p.right == null || p.right == q)) {  
            visit(p);  
            q = p;// 记录上一个已输出节点  
            if (stack.empty())  
                return;  
            p = stack.pop();  
        }  
        // 处理右子  
        stack.push(p);  
        p = p.right;  
    }  
}  
  
/** 非递归实现后序遍历 双栈法 */  
protected static void iterativePostorder2(Node p) {  
    Stack<Node> lstack = new Stack<Node>();  
    Stack<Node> rstack = new Stack<Node>();  
    Node node = p, right;  
    do {  
        while (node != null) {  
            right = node.right;  
            lstack.push(node);  
            rstack.push(right);  
            node = node.left;  
        }  
        node = lstack.pop();  
        right = rstack.pop();  
        if (right == null) {  
            visit(node);  
        } else {  
            lstack.push(node);  
            rstack.push(null);  
        }  
        node = right;  
    } while (lstack.size() > 0 || rstack.size() > 0);  
}  
  
/** 非递归实现后序遍历 单栈法*/  
protected static void iterativePostorder3(Node p) {  
    Stack<Node> stack = new Stack<Node>();  
    Node node = p, prev = p;  
    while (node != null || stack.size() > 0) {  
        while (node != null) {  
            stack.push(node);  
            node = node.left;  
        }  
        if (stack.size() > 0) {  
            Node temp = stack.peek().right;  
            if (temp == null || temp == prev) {  
                node = stack.pop();  
                visit(node);  
                prev = node;  
                node = null;  
            } else {  
                node = temp;  
            }  
        }  
  
    }  
}  
  
/** 非递归实现后序遍历4 双栈法*/  
protected static void iterativePostorder4(Node p) {  
    Stack<Node> stack = new Stack<Node>();  
    Stack<Node> temp = new Stack<Node>();  
    Node node = p;  
    while (node != null || stack.size() > 0) {  
        while (node != null) {  
            temp.push(node);  
            stack.push(node);  
            node = node.right;  
        }  
        if (stack.size() > 0) {  
            node = stack.pop();  
            node = node.left;  
        }  
    }  
    while (temp.size() > 0) {  
        node = temp.pop();  
        visit(node);  
    }  
}  
```

``` java
// 层次遍历
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> list = new ArrayList<List<Integer>>();
    List<Integer> inResult = new ArrayList<Integer>();

    if (root == null) {
        return list;
    }

    Queue<TreeNode> queue = new LinkedList<TreeNode>();
    queue.add(root);

    // 在层次遍历的基础上，每次判断 queque.isempty() 后，加一个queue.size
    // 并用 size 做内层循环条件，这样内层循环，只循环一层的内容
    while (!queue.isEmpty()) {
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            TreeNode p = queue.poll();
            inResult.add(p.val);
            if (p.left != null) {
                queue.add(p.left);
            }
            if (p.right != null) {
                queue.add(p.right);
            }

        }

        list.add(inResult);
        inResult = new ArrayList<Integer>();
        // 错误代码， 此处不能用clear(),inResult是个指针
        // inResult.clear();
    }
    return list;

}
```



*** 

参考：
1. {% link 更简单的非递归遍历二叉树的方法 http://zisong.me/post/suan-fa/geng-jian-dan-de-bian-li-er-cha-shu-de-fang-fa 更简单的非递归遍历二叉树的方法 %}
2. {% link JAVA 二叉树的递归和非递归遍历 http://robinsoncrusoe.iteye.com/blog/808526 JAVA 二叉树的递归和非递归遍历 %}

Over！










































