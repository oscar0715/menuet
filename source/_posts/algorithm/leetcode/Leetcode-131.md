title: Leetcode_131
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-26 21:32:00
---
Palindrome Partitioning
<!-- more -->

***

# Problem
Given a string s, partition s such that every substring of the partition is a palindrome.

Return all possible palindrome partitioning of s.

For example, given s = "aab",
Return

```
[
  ["aa","b"],
  ["a","a","b"]
]
```


# Solution 
回溯
BackTracking

对于 String S 
1. 第一层是left = 0， right 递增到 length（） -1
2. 然后再一层一层回溯下去……


``` java
public class Solution {
    
    List<List<String>> res = new ArrayList<>();
    ArrayList<String> list = new ArrayList<>();
    
    public List<List<String>> partition(String s) {
        bfs(s, 0);
        return res;
    }
    
    public void bfs(String s, int l) {
        if(list.size() > 0 && l >= s.length()) {
            ArrayList<String> r = (ArrayList<String>) list.clone();
            res.add(r);
        }
        
        for(int i = l; i < s.length(); i++) {
            if(isPalindrome(s, l, i)) {
                if(l == i) {
                    list.add(s.charAt(i)+"");
                } else {
                    list.add(s.substring(l, i+1));
                }
                bfs(s, i+1);
                list.remove(list.size()-1);
            }
        }
    }
    
    public boolean isPalindrome(String s, int head, int tail) {
        
        if(head == tail) {
            return true;
        }
        
        while(head < tail) {
            if(s.charAt(head) != s.charAt(tail)) {
                return false;
            } 
            head++;
            tail--;
        }
        return true;
    }
}
```

*** 

Over！










































