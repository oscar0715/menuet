---
title: Leetcode_017
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-01 20:52:00
---
Letter Combinations of a Phone Number

<!-- more -->

***

### Problem
Given a digit string, return all possible letter combinations that the number could represent.

### Solution 1

BackTracking

``` java
public class Solution {
    List<String> res = new ArrayList<>();
    
    public List<String> letterCombinations(String digits) {
        if(digits.length() == 0) {
            return res;
        }
    
        String[] map = {"","", "abc","def","ghi","jkl","mno","pqrs","tuv","wxyz"};
        backtrack(map, digits, 0, "");
        
        return res;
    }
    
    public void backtrack(String[] map, String digits, int cur, String string) {
        if(cur == digits.length()) {
            res.add(string);
            return;
        }
        
        int index = Integer.parseInt(digits.charAt(cur)+"");
        String s = map[index];
       
        for(int i = 0; i < s.length(); i++) {
            backtrack(map, digits, cur+1, string+s.charAt(i));
        }
    }
}
```

### Solution 2
暴力破解

{% codeblock lang:java  %}
public List<String> letterCombinations(String digits) {
	String[] chars = {"0", "1", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
	List<String> result = new ArrayList<String>();

	int[] digitsArray = new int[digits.length()];
	for(int i = 0; i < digits.length(); i++) {
		digitsArray[i] = Integer.valueOf(digits.charAt(i)+"");
	}

	for(int i = 0; i < digits.length(); i++) {
		if( i == 0) {
			for (int j = 0; j < chars[ digitsArray[i] ].length(); j ++) {
				result.add(chars[digitsArray[i]].charAt(j) + "");
			}
		} else {
			List<String> list = new ArrayList<String>();
			for (String s : result) {
				for (int j = 0; j < chars[ digitsArray[i] ].length(); j ++) {
					list.add( s + chars[digitsArray[i]].charAt(j) );
				}
			}
			result = list;
		}
	}
	return result;
}
{% endcodeblock %}
