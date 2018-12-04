---
title: Leetcode_202
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-20 17:04:00
---
Happy Number

<!-- more -->

***

# Problem
Write an algorithm to determine if a number is "happy".

A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.

Example: 19 is a happy number

<ul style="list-style: none;">
<li>1<sup>2</sup> + 9<sup>2</sup> = 82</li>
<li>8<sup>2</sup> + 2<sup>2</sup> = 68</li>
<li>6<sup>2</sup> + 8<sup>2</sup> = 100</li>
<li>1<sup>2</sup> + 0<sup>2</sup> + 0<sup>2</sup> = 1</li>
</ul>


# Solution
如果出现永远循环的话，终端条件就是出现了平方和以后出现同样的结果。

用一个set的add方法做判断，如果添加已有的元素，那么返回为false

``` java
public boolean isHappy(int n) {
    if (n==1){
        return true;
    }

    Set<Integer> set = new HashSet<Integer>();

    while (set.add(n)){
        n = squreSum(n);
        if (n == 1){
            return true;
        }

    }
    return false;

}

private int squreSum(int n){
    int m = 0;
    while (n > 0) {
        m += Math.pow(n % 10, 2);
        n = n / 10;
    }
    return m;
}

```

# Solution

这个方法在 Solution Discussion 看到的。我觉得非常萌，用的快慢指针。

``` java
int digitSquareSum(int n) {
    int sum = 0, tmp;
    while (n) {
        tmp = n % 10;
        sum += tmp * tmp;
        n /= 10;
    }
    return sum;
}

bool isHappy(int n) {
    int slow, fast;
    slow = fast = n;
    do {
        slow = digitSquareSum(slow);
        fast = digitSquareSum(fast);
        fast = digitSquareSum(fast);
    } while(slow != fast);
    if (slow == 1) return 1;
    else return 0;
}
```


*** 

Over！










































