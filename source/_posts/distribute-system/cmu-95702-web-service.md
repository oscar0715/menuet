---
title: CMU 95702 - Web Service
tags:
  - Distribution-System
categories:
  - Technology
  - Distribution-System
date: 2016-11-1 10:21:45
---
CMU 95702 - Distributed System

Web Service

书上第九章的内容，但是考试好像都不涉及，只总结了一部分的内容
<!-- more -->

***
# Introduction

先来放一张总体的框架图：
![web service infrastructure and componets](http://images.slideplayer.com/25/8078622/slides/slide_11.jpg "web service infrastructure and componets")

# Web Service
**Combination if web service**
如果每一种服务都有一个接口，那么把多个接口组合在一起，也就最终把服务组合在了一起。

举个例子，如果一个订票网站，分别开放了：订房，租车，机票的标准的网络服务接口。
那么就可以把它们组合在一起组成一个

**communication pattern**
1. synchronous
2. asynchronous
如果是request-reply模式，那client端可以稍后再接收reply.
例如订票网站，发出订票请求以后，服务端可以时不时地更新一下订票信息，直到所有需要的信息都补充完全了。

Web servie是要在分布式的互联网上的使用的，而互联网上有不同的语言和范式，所以要求web service要完全独立于某个范式。

**loose coupling**
在web service的语意下，低耦合意味着，不同服务间的依赖要最小，这样底层的实现就能更加的灵活了。

要实现低耦合的方法：
1. 接口编程
把接口和接口的实现分开来。
2. 对接口进行简化，并使之能够通用
例如REST approach。这样能够把依赖于操作名的耦合降低。这样的做法的一个结果就是，数据的变得比操作更加重要，
3. 降低耦合之后，web service就有了不同的范式：
    - request-replay: RR 通信使client和server耦合在了一起
    - asynchronous messaging: 相对于同步的情况，降低了耦合
    - indirect communication: 通过第三方的间接通信，在时间和空间上都解耦了

**Representation of messages**
通过SOAP来传输的数据是用XML格式的，比起二进制，XML要占用更多的空间（因为有标签呀），而且还要花更多的时间来解析XML数据。但是XML的可读性更强。

**Service Reference**
URI (Uniform Resource Identifier)
1. URL(Uniform Resource Location): 包含了域名等信息
2. URN(Uniform Resource Name): 和域名独立，需要lookup service找到name相应的location

每个web service都有一个URI，通过URI被用户找到。
最常用的URI就是URL了。

**Transparency**
middlware的作用是：
1. 让程序员不需要操心data representation和marshalling
2. 让程序员调用远程服务就像调用本地服务

但是对于web service来说就没有middleware这件事情。
通常来说，client端和server端直接在SOAP中用XML对消息进行读写。

不过，更加方便的是，SOAP和XML的实现也可以被本地的API封装起来，这样既不用担心必须要的marshalling和unmarshalling。

Proxy/Dynamic invocation 

## SOAP
SOAP定义了一个用XML来展示request和reply内容的scheme。

**SOAP Message：**
先上一张图，SOAP Message的结构
![](http://images.slideplayer.com/12/3709091/slides/slide_16.jpg)

再来一段代码
``` xml
<?xml version="1.0"?>

<soap:Envelope
xmlns:soap="http://www.w3.org/2003/05/soap-envelope/"
soap:encodingStyle="http://www.w3.org/2003/05/soap-encoding">

<soap:Header>
...
</soap:Header>

<soap:Body>
    <!-- SOAP message 可以传文档，也可以用来传request或者reply，都放在body里面 -->
    ...
    <soap:Fault>
    // 如果 request报出错了，在这个元素里面会记录错误描述
    ...
    </soap:Fault>
</soap:Body>

</soap:Envelope>
```


## Comparison of web services with the distributed object model

Web service 需要服务接口来对数据进行获取或者更新，和RMI有相似之处。RMI通过remote object reference来对远程对象进行操作。Web servicez则是利用URI来。

不过亦有不同之处：
RMI中可以创建新的remote object，并返回新建的对象的remote object reference。而Web service则不同，Web service的服务和对象都是可以认为都是固定的，所以也不涉及garbage collection的问题




























***

Over！