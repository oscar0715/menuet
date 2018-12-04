---
title: Leetcode_146
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-07-23 13:39:00
---
LRU Cache

<!-- more -->

***

### Problem
Design and implement a data structure for Least Recently Used (LRU) cache. It should support the following operations: get and set.

get(key) - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.
set(key, value) - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.

设计题

设计一个缓存结构，如果缓存数量达到capacity，就把最久远没用的key删掉


### Solution 
设计一个class， class中用time来标记时间戳
用PriorityQueue，来对时间戳排序。

我这个方法太慢了。
用linked list比较快
{% codeblock lang:java  %}
public class solution146LRUCache {

    Map<Integer, CacheObject> valueMap = new HashMap<Integer, CacheObject>();

    Comparator<CacheObject> comparator = new Comparator<CacheObject>() {


        public int compare(CacheObject o1, CacheObject o2) {
            return o1.getTime()-o2.getTime();
        }
    };

    Queue<CacheObject> queue = new PriorityQueue<CacheObject>(11, comparator);

    int capacity;
    int size;
    int time;

    public solution146LRUCache(int capacity) {

        this.capacity = capacity;
        this.size = 0;
        this.time = 0;
    }

    public int get(int key) {
        if (valueMap.get(key) != null) {
            this.time++;
            CacheObject object = valueMap.get(key);
            object.setTime(time);
            queue.remove(object);
            queue.add(object);
            return valueMap.get(key).getValue();
        }else {
            return -1;
        }


    }

    public void set(int key, int value) {
        time++;
        if (valueMap.containsKey(key)) {
            CacheObject object = valueMap.get(key);
            object.setTime(time);
            object.setValue(value);
            queue.remove(object);
            queue.add(object);
        } else {
            if (size == capacity) {
                CacheObject poll = queue.poll();
                valueMap.remove(poll.getKey());
            } else {
                size++;
            }
            CacheObject cacheObject = new CacheObject(key, value, this.time);
            valueMap.put(key, cacheObject);
            queue.add(cacheObject);
        }
    }

    @Override
    public String toString() {
        return Arrays.toString(valueMap.values().toArray());
    }

    class CacheObject {

        int key;
        int value;
        int time;

        private CacheObject(int key, int value, int time) {
            this.key = key;
            this.value = value;
            this.time = time;
        }


        public int getKey() {
            return key;
        }

        public int getTime() {
            return time;
        }

        public void setTime(int time) {
            this.time = time;
        }

        public int getValue() {
            return value;
        }

        public void setValue(int value) {
            this.value = value;
        }

        public String toString() {
            return "[ " + key + ", " + value + ", " + time + " ]";
        }
    }
}
{% endcodeblock %}

### Solution 题目看错版
一开始以为是key用到频率最少的删掉
{% codeblock lang:java  %}
public class solution146LRUCacheError {

    Map<Integer, CacheObject> valueMap = new HashMap<Integer, CacheObject>();

    Comparator<CacheObject> comparator = new Comparator<CacheObject>() {
        public int compare(CacheObject o1, CacheObject o2) {

            return o1.frequency-o2.frequency;
        }
    };

    Queue<CacheObject> queue = new PriorityQueue<CacheObject>(11, comparator);

    int capacity;
    int size;

    public solution146LRUCacheError(int capacity) {

        this.capacity = capacity;
        this.size = 0;
    }

    public int get(int key) {
        if (valueMap.get(key) != null) {
            CacheObject object = valueMap.get(key);
            object.addFrequency();
            queue.remove(object);
            queue.add(object);
            return valueMap.get(key).getValue();
        }else {
            return -1;
        }


    }

    public void set(int key, int value) {
        if (valueMap.containsKey(key)) {
            valueMap.get(key).setValue(value);
        } else {
            if (size == capacity) {
                CacheObject poll = queue.poll();
                valueMap.remove(poll.getKey());
            } else {
                size++;
            }
            CacheObject cacheObject = new CacheObject(key, value);
            valueMap.put(key, cacheObject);
            queue.add(cacheObject);
        }
    }

    @Override
    public String toString() {
        return Arrays.toString(valueMap.values().toArray());
    }

    class CacheObject {

        int key;
        int value;
        int frequency;

        private CacheObject(int key, int value) {
            this.key = key;
            this.value = value;
            this.frequency = 0;
        }

        private void addFrequency() {
            frequency++;
        }

        public int getKey() {
            return key;
        }

        public int getFrequency() {
            return this.frequency;
        }

        public int getValue() {
            return value;
        }

        public void setValue(int value) {
            this.value = value;
        }

        public String toString() {
            return "[ " + key + ", " + value + ", " + frequency + " ]";
        }
    }
}
{% endcodeblock %}
*** 

Over！










































