---
title: Leetcode_299
tags:
	- Leetcode
categories:
	- Technology
	- Leetcode
date: 2016-09-02 22:17:00
---
Bulls and Cows

<!-- more -->

***

### Problem
猜数字，数字和位置猜中了就是bull，只猜中有这个数字，没有猜中位置那就是cow

	For example:

	Secret number:  "1807"
	Friend's guess: "7810"
Hint: 1 bull and 3 cows. (The bull is 8, the cows are 0, 1 and 7.)
Write a function to return a hint according to the secret number and friend's guess, use A to indicate the bulls and B to indicate the cows. In the above example, your function should return "1A3B".

Please note that both secret number and friend's guess may contain duplicate digits, for example:

	Secret number:  "1123"
	Friend's guess: "0111"
In this case, the 1st 1 in friend's guess is a bull, the 2nd or 3rd 1 is a cow, and your function should return "1A1B".




### Solution
two pass solution 
1. 第一轮，求bull，顺便添加到map
2. 第二轮，求数字对应上的总数total（先不管位置）
3. cow = total - bull

``` java
public class Solution {
    public String getHint(String secret, String guess) {
        
        // 字符哈希表
        int[] hash = new int[256];
        
        int bull = 0;
        for(int i = 0; i < secret.length(); i++){
        	// 数bulls
            if(secret.charAt(i) == guess.charAt(i) ) {
                bull++;
            } 
            // 计数
            hash[secret.charAt(i)]++;
        }
        
        int cow = 0;
        for(int i = 0; i < guess.length(); i++){
        	// 只要有，那么cow就++
        	// 不过实际上，这里cow 把 bull 也算进去了。
        	// 所以 return 的时候要减掉
            if(hash[guess.charAt(i)]-- >= 1) {
                cow++;   
            }
        }
        return bull + "A" + (cow-bull) + "B";
    }
}
```


*** 

Over！










































