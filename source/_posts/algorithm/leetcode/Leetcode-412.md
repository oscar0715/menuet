---
title: Leetcode_412
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2017-03-29 10:51:00
---
Fizz Buzz

<!-- more -->

***

### Problem
Write a program that outputs the string representation of numbers from 1 to n.

But for multiples of three it should output “Fizz” instead of the number and for the multiples of five output “Buzz”. For numbers which are multiples of both three and five output “FizzBuzz”.

Example:

n = 15,

Return:
```
[
    "1",
    "2",
    "Fizz",
    "4",
    "Buzz",
    "Fizz",
    "7",
    "8",
    "Fizz",
    "Buzz",
    "11",
    "Fizz",
    "13",
    "14",
    "FizzBuzz"
]
```

### Solution 

``` java
public class Solution {
    public List<String> fizzBuzz(int n) {
        List<String> res = new ArrayList<>();
        
        int i = 1;
        while(i <= n) {
            if(i%3==0 && i%5==0 ){
                res.add("FizzBuzz");
            } else if(i%3==0) {
                res.add("Fizz");
            } else if(i%5==0) {
                res.add("Buzz");
            } else {
                res.add(i+"");
            }
            i++;
        }
        return res;
    }
}
```

# Solution 2

用单独的计数器数 fizz buzz。这样不用 %

``` java
public class Solution {
    public List<String> fizzBuzz(int n) {
        List<String> res = new ArrayList<>();
        
        int i = 1;
        int fizz = 1;
        int buzz = 1;
        while(i <= n) {
            if(fizz == 3 && buzz==5 ){
                res.add("FizzBuzz");
                fizz = 1;
                buzz = 1;
            } else if(fizz==3) {
                res.add("Fizz");
                fizz = 1;
                buzz++;
            } else if(buzz==5) {
                res.add("Buzz");
                buzz = 1;
                fizz++;
            } else {
                res.add(i+"");
                buzz++;
                fizz++;
            }
            i++;
        }
        return res;
    }
}
```










































