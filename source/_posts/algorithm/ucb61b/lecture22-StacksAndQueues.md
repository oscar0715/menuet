---
title: Lecture22 Stacks And Queues
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-07-13 12:52:30
---
Lecture22 Stacks And Queues

还有上节课没讲完的hashcode

<!-- more -->

***
## Hash Table
string --> hashcode -->(compression function) bucket

### Good HashCode
一个好的对字符串的hashcode算法
{% codeblock HashCode Function lang:Java  %}
private static int hashCode(String key){
    int hashcode = 0;
    for (int i = 0; i < key.length(); i++){
        hashVal = (127 * hashVal + key.charAt(i)) % 16908799
    }
}
{% endcodeblock %}

1. 127 * hashVal的作用是：不同位置上的相同字母也会对hashVal有不同的影响
2. 16908799 是一个大的质数，保证有足够多的buckets可以用，也保证hashVal不会overflow
3. 127 和 16908799 最好都是质数
    - 否则当他们有公因子x时
    - 当最后的hashVal = ax+b, 那b只由最后一个字母决定（Bias）
    - 127这个位置的数不能太小，否则也有buckets数量太小的问题


### BadHashcode

1. Sum ASCII values of characters
    - 和总小于500，buckets最多也就500个
    - Anagram太多 "apt", "pat", "tap"
2. First 3 letters of the words
    - 很多单词共有相同的前缀，例如"pre"
    - 有些"xzp"有些前缀压根没有
3. Suppose the prime module to 127
    - (127 * hashVal + key.charAt(i)) % 127 = key.charAt(i)
    - 所以只有最后一个字母有影响

### Black Art
1. Resizing the hash tables
    - if load factor n/N is to larger, we lose O(1) time
    - load factor > c, typically 0.75, resize it 
    - Allocate new Array (at leat twice as large)
    - Walk the old array, rehash entries into new hash table
    - 同理 shrink hash tables when n/N < 0.25 to free memory
    - Resize:  O(n) time

### Transposition Table: Speed Game Trees
- MiniMax Alg checks if grid is in table
    - Yes: return score
    - No: evaluate score (by minimax recursively) & store it 

这一段不知所云

## Stacks
1. push
2. pop
3. top

Single-Linked List
All operations take O(1) time

Parenthesis Matching







