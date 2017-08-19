---
title: Lecture32 Sorting III
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-07-26 18:20:30
---
Lecture32 Sorting III

<!-- more -->

<br>

***

<br>

# Selection 
 
Select kth smallest key in list 

Qucik Select

Partition `I` into List `I1, Iv, I2`

Item with same key as v go into any list
(list-based `Iv`; array-basd `I1&I2`)

    if (j < |I1|) {
        Recursivly find item with index j in I1;
        return it;
    } else if (j < |I1| + |Iv|) {
        return v;
    } else {
        Recusively find tiem with index j - |I1| - |Iv| in I2;
        return it;
    }

和 Quick Sort 比起来，每次只要递归一个`I1 I2`其中的一个就可以了，所以会快一些
(Quick sort 里的i 就是 `|I2|`)

O(n) average time if we select pivot properly

# A Lower Bound on comparision-based sorting

`Lower Bound` 最快情况

comparasion-based:
all decisions based on comparing keys (If statements)

A correct sorting algorithm must generate a `different` sequence of true/false answer for each permutation of 1 ... n

# Linear-time sorting
Bucket Sort
works when keys are in small range 
e.g 0 to q-1 ——i.e when q in O(n)

Array of q queues, numbered from O to q-1.

enqueue each item: key i goes to queue i




















































