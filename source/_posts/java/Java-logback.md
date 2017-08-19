---
title: Java LogBack 学习
tags:
  - Java
  - LogBack
categories:
  - Techonology
  - Java
date: 2017-04-21 00:28:17
---
Java LogBack
<!-- more -->

***

# Logback
## logback的结构
LogBack被分为3个组件，logback-core, logback-classic 和 logback-access.
1. 其中logback-core提供了LogBack的核心功能，是另外两个组件的基础。
2. logback-classic则实现了Slf4j的API，所以当想配合Slf4j使用时，需要将logback-classic加入classpath.
3. logback-access是为了集成Servlet环境而准备的，可提供HTTP-access的日志接口；

## Logger, Appenders and Layouts

logging API 比 println好在哪儿呢？它可以选择性的打印一部分 log （按照等级），而 System.out.println 不能

Logger 类在 logback-classic 里。Appender 和 Layout 两个接口在 logback-core 中。

Logger context 会把所有的 logger 组成一个树状结构。
>For example, the logger named "com.foo" is a parent of the logger named "com.foo.Bar". Similarly, "java" is a parent of "java.util" and an ancestor of "java.util.Vector". This naming scheme should be familiar to most developers.






