---
title: Java_日志系统
tags:
  - Java
  - Logger
categories:
  - Techonology
  - Java
date: 2017-04-21 00:27:17
---
Java 日志系统
<!-- more -->

***
# 引用放在最前面
1. {% link Java日志终极指南 http://www.importnew.com/16331.html Java日志终极指南 %}
2. {% link LogBack简易教程 http://www.cnblogs.com/mailingfeng/p/3499436.html LogBack简易教程 %}
3. {% link slf4j+logback配置及详解 http://www.bkjia.com/ASPjc/1047668.html slf4j+logback配置及详解 %}
4. {% link logback 常用配置详解（序） http://aub.iteye.com/blog/1101222 logback 常用配置详解（序） %} : 这篇很详细


# 简介
## 日志框架
在Java中，输出日志需要使用一个或者多个日志框架，这些框架提供了必要的对象、方法和配置来传输消息。

1. Java在`java.util.logging`包中提供了一个默认的框架。
2. 除此之外，还有很多其它第三方框架，包括`Log4j`、`Logback`以及`tinylog`。
3. 还有其它一些开发包，例如`SLF4J`和`Apache Commons Logging`，它们提供了一些抽象层，对你的代码和日志框架进行解耦，从而允许你在不同的日志框架中进行切换。

## 抽象层
诸如`SLF4J`这样的抽象层，会将你的应用程序从日志框架中解耦。

应用程序可以在运行时选择绑定到一个特定的日志框架（例如`java.util.logging`、`Log4j`或者`Logback`），这通过在应用程序的类路径中添加对应的日志框架来实现。

如果在类路径中配置的日志框架不可用，抽象层就会立刻取消调用日志的相应逻辑。抽象层可以让我们更加容易地改变项目现有的日志框架，或者集成那些使用了不同日志框架的项目。

## 配置
尽管所有的Java日志框架都可以通过代码进行配置，但是大部分配置还是通过外部配置文件完成的。这些文件决定了日志消息在何时通过什么方式进行处理，日志框架可以在运行时加载这些文件。

## Java日志组件
Java日志API由以下三个核心组件组成：

1. Loggers：Logger负责捕捉事件并将其发送给合适的Appender。
2. Appenders：也被称为Handlers，负责将日志事件记录到目标位置。在将日志事件输出之前，Appenders使用Layouts来对事件进行格式化处理。
3. Layouts：也被称为Formatters，它负责对日志事件中的数据进行转换和格式化。Layouts决定了数据在一条日志记录中的最终形式。

当Logger记录一个事件时，它将事件转发给适当的Appender。然后Appender使用Layout来对日志记录进行格式化，并将其发送给控制台、文件或者其它目标位置。另外，Filters可以让你进一步指定一个Appender是否可以应用在一条特定的日志记录上。在日志配置中，Filters并不是必需的，但可以让你更灵活地控制日志消息的流动。



*** 
Reference:
1. [Log4j1,Logback以及Log4j2性能测试对比](https://my.oschina.net/OutOfMemory/blog/789267 "Log4j1,Logback以及Log4j2性能测试对比")
2. [Log4j配置文件详解](https://my.oschina.net/xianggao/blog/515216 "Log4j配置文件详解")
3. [logback高级特性使用(三) 异步记录日志](http://blog.csdn.net/shayanxiang/article/details/43194373  "logback高级特性使用(三) 异步记录日志")
4. [Log4j的AsyncAppender能否提升性能？](http://blog.csdn.net/chendc201/article/details/9033357 "Log4j的AsyncAppender能否提升性能？")
5. [logback: AsyncAppender takes more time than Synchronous FileAppender](http://stackoverflow.com/questions/38685471/logback-asyncappender-takes-more-time-than-synchronous-fileappender "logback: AsyncAppender takes more time than Synchronous FileAppender")
6. [Benchmarking Java logging frameworks](https://www.loggly.com/blog/ "benchmarking-java-logging-frameworks/ Benchmarking Java logging frameworks")






















