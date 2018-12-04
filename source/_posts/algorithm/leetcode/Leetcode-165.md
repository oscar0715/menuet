---
title: Leetcode_165
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-04-03 20:59:00
---
Compare Version Numbers

<!-- more -->

***

### Problem
判断两个版本号的大小

"0.0.1" < "0.1", "1.0.0" == "1"


### Solution 
用Queue来取每一层的版本号
相当于自己写了个 split

``` java
public class Solution {
    public int compareVersion(String version1, String version2) {
        Queue<Integer> q1 = getQueue(version1);
        Queue<Integer> q2 = getQueue(version2);
        
        while(!q1.isEmpty() && !q2.isEmpty()) {
            Integer num1 = q1.poll();
            Integer num2 = q2.poll();
            
            if(num1.compareTo(num2) != 0) {
                return num1.compareTo(num2);
            }
        }
        
        // queue 可能非空，但是可能剩下都是 0
        while(!q1.isEmpty()){
            if(q1.poll() !=0 ){
                return  1;
            }
        }
        
        while(!q2.isEmpty()){
            if(q2.poll() !=0 ){
                return  -1;
            }
        } 
        
        return 0;
        
    }
    
    private Queue<Integer> getQueue(String s) {
        Queue<Integer> q = new LinkedList<>();
        int num = 0;
        for(int i = 0; i < s.length(); i++) {
            if(s.charAt(i)=='.'){
                q.add(num);
                num = 0;
            } else {
                num = num * 10 + Character.getNumericValue(s.charAt(i));
            }
        }
        // 不要忘了最后这一次add
        q.add(num);
        
        return q;
    }
}
```

*** 

Over！










































