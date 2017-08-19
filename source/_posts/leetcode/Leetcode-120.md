---
title: Leetcode_120
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-26 16:01:00
---
Triangle
<!-- more -->

***

### Problem
Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.

给出一个三角形，求最小路劲
如果上一层取的index 为 k， 那么下一层只能取，k 或者 k + 1

For example, given the following triangle
```
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
```

The minimum path sum from top to bottom is 11 (i.e., 2 + 3 + 5 + 1 = 11).

### Solution Best
自下而上的动态规划
``` java
public class Solution {
    
    public int minimumTotal(List<List<Integer>> triangle) {
        
        if(triangle.size() == 0) {
            return 0;
        }
        
        // 比最长那个list长 1
        int[] tmp = new int[triangle.get(triangle.size()-1).size() + 1];
        
        // 从最下面向上循环
        for(int i = triangle.size() - 1; i >= 0; i--) { 
            for(int j = 0; j < triangle.get(i).size(); j++) {
                // 最后这个长度多出来的这个1，只会在第一次使用，因为是0，所以相加没影响。
                tmp[j] = triangle.get(i).get(j) + Math.min(tmp[j],tmp[j+1]);
            }
        }
        return tmp[0];
    }
}
```

### Solution 2
动态规划，用两个数组

``` java
public class Solution {
    
    public int minimumTotal(List<List<Integer>> triangle) {
        
        if(triangle.size() == 0) {
            return 0;
        }
        
        int[] tmp = new int[triangle.get(triangle.size()-1).size()];
        int[] tmp1 = new int[triangle.get(triangle.size()-1).size()];
        
        tmp[0] = triangle.get(0).get(0);
        
        for(int i = 1; i < triangle.size(); i++) {
            for(int j = 0; j < triangle.get(i).size(); j++) {
                
                int val1 = Integer.MAX_VALUE;
                int val2 = Integer.MAX_VALUE;
                
                if( (j - 1) >= 0) {
                    val1 = tmp[j - 1];
                }
                
                if(j < triangle.get(i-1).size()) {
                    val2 = tmp[j];
                }
                
                tmp1[j] = triangle.get(i).get(j) + Math.min(val1,val2);
                
            }
            tmp = tmp1;
            tmp1 = new int[triangle.get(triangle.size()-1).size()];
            
        }
        int min = tmp[0];
        
        for(int i : tmp) {
            min = Math.min(min, i);
        }
        
        return min;
    }

}
``` 
### Solution 3
DFS

这个超时了Orz

``` java
public class Solution {
    
    public int minimumTotal(List<List<Integer>> triangle) {
        PriorityQueue<Integer> queue = new PriorityQueue<>();
        
        getSum(triangle, 0, 0, 0, queue);
        
        return queue.poll();
    }
    
    public void getSum(List<List<Integer>> triangle, int level, int index, int sum, Queue<Integer> queue) {
        if( level == triangle.size() ) {
            queue.add(sum);
            return;
        }
        int val = triangle.get(level).get(index);
        getSum(triangle, level + 1, index, sum + val, queue);
        getSum(triangle, level + 1, index + 1, sum + val, queue);
    }
}
```

*** 

Over！











































