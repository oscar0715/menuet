---
title: Java自动内存管理【Java系列笔记】
tags:
  - Java
categories:
  - Technology
  - Java
date: 2017-04-07 16:40:00
---

Java 自动内存管理
Reference: [JVM自动内存管理：视频](http://www.jikexueyuan.com/course/1793_2.html "JVM自动内存管理：视频")
<!-- more -->

***

# Java 内存区域基础概念
Java 虚拟机运行时数据区
1. 程序计数器
2. Java 堆
3. Java 虚拟机栈
4. 本地方法栈
5. 方法区

![数据区](http://ogy9ohfjc.bkt.clouddn.com/neicunguanli/runtime-shujuqu.png "数据区")

# 程序计数器
Program Counter Register
- 一块较小的内存空间
- 当前线程所执行的`字节码`的行号指示器
- 是线程私有的一块区域
- 如果线程正在执行一个java方法，计数器记录的是正在执行的虚拟机`字节码指令`的内存 地址。
- 如果执行本地Native方法，计数器则为空

# 虚拟机栈
- 线程私有，生命周期与线程相同
- 后进先出栈（LIFO）
- 描述 Java 方法`调用，执行，退出`的内存概念模型
	每个方法执行的时候都会创建一个栈帧，用来创建这个方法的操作数栈，局部变量表，方法出口，动态链接等信息。
	每个方法调用和结束的过程就对应了一个栈帧在虚拟机栈中入栈和出栈的过程
- 可能出现 OutOfMemoryError 和 StackOverflowError

# 本地方法栈 
- 线程私有
- 后进先出栈
- 描述 Native 方法`调用，执行，退出`
- 可能出现 OutOfMemoryError 和 StackOverflowError

# 栈帧
栈帧
- Java 虚拟机栈中存储的内容
- 用于存储`数据`和`部分过程结果`的数据结构 
- 同时也用来处理`动态链接`，`方法返回值`和`异常分派`
- 一个完整的栈帧包括：局部变量表，操作数栈，动态连接信息表，方法返回地址和异常信息


局部变量表
- 是一组变量值的存储空间，长度由编译器决定
- 由若干slot组成
- Slot可以存储一个类型为 boolean，byte，char，short，float，reference 和 returnAddress 的数据，两个slot可以存储一个类型为long或double的数据（64位）
- Reference
	1. 对象实例的引用，查找到对象实例在Java堆中的地址
	2. 可以查找到这个对象类型在Java方法区中的数据类型信息
- returnAddres 不再使用了
- 局部变量表用于方法之间传递参数，以及方法执行过程中存储基础数据类型的值和对象的引用

操作数栈
- 是一个后进先出栈，
- 由若干 Entry 组成，长度由编译器决定
- 在方法执行过程中，栈帧用于存储`计算参数`和`计算结果`；
	在方法调用时，操作数栈也用来准备`调用方法`的`参数`以及接受方法的`返回结果`。


# Java 堆
Java 堆
1. 全局共享
2. 通常是 JVM 中最大的一块区域
3. 作为Java对象的主要存储区域
4. 要求实现自动内存管理，也就是GC 

![堆和栈1](http://ogy9ohfjc.bkt.clouddn.com/neicunguanli/stackandheap.png "堆和栈1")
Reference 直接指向对象的地址，对象的对象头中有一个到对象类型数据的指针，再通过这个指针到方法区找对象的类型数据。这个方法的好处是，更快（更直接嘛），指针内存更少。

![堆和栈2](http://ogy9ohfjc.bkt.clouddn.com/neicunguanli/stackandheap2.png "堆和栈2")
Reference指向一个句柄。这种方法的好处是，对象实例被移动或者回收以后，Reference本身是不用被修改的，存储的值是稳定的，只会改变句柄池中到对象实例数据的指针。

Java堆异常
1. OutOfMemory

# 方法区
方法区特征：
1. 全局共享
2. 存储 Java 类的结构信息
3. 可能出现 OutOfMemoryError

运行时常量池特征：
1. 全局共享
2. 是方法区的一部分 （1.7 移到 Java 堆之中）
3. 作用是存储 Java 类文件常量池中的符号信息
4. 可能出现 OutOfMemoryError

# 直接内存
特征
1. 全局共享
2. 并非 JVMS 定义的标准 Java 运行时内存区域
3. 可能出现 OutOfMemoryError


# 遗补
Java 内存区域模型：
![Java 内存区域模型](http://www.hollischuang.com/wp-content/uploads/2015/04/2354447461.jpg "Java 内存区域模型")


