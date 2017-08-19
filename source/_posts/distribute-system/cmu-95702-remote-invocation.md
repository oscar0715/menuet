---
title: CMU 95702 - Remote Invocation   
tags:
  - Distribution-System
categories:
  - Technology
  - Distribution-System
date: 2016-10-30 19:04:45
---
CMU 95702 - Distributed System

Remote Invocation  
<!-- more -->

***

# Middleware layers
Top - Down Structure
![](http://images.slideplayer.com/26/8686098/slides/slide_3.jpg)

中间两部分属于 Middleware Layer
本文讲的是第2部分

# Request-Reply Protocol
Request-Reply 是一种同步的通信协议，在收到reply之前，client端的进程是被阻塞的。

## A Request Reply Protocol

Client side:
    // used to invoke remote operation
    `public byte[] doOperation (RemoteObjectRef o, int methodId, byte[] arguments)`


Server side:
    `public byte[] getRequest ();`
    `public void sendReply (byte[] reply, InetAddress clientHost, int clientPort);`



## Request-Reply Message Structure

1. messageType 
int (0=Request, 1= Reply)
2. requestId
int
3. objectReference
RemoteObjectRef
4. methodId
int or Method
5. arguments
array of bytes


## UDP实现 
**Message Identifier**
1. requestId (让这条消息在发送端是唯一的)
2. an indentifier for the sender process, for example, the port and Internet address （让整条消息在整个分布式系统中是唯一的）

**Failure Model of request-reply model**
The same as UDP
Use Timeout 同样运用超时的机制来处理消息发送失败的情况。

**Timeouts**
Send request repeatly

**duplicate request message**
有时候 server 处理 request 的时间超过了 timeout 设定的时间，所以 client 可能会重复发送 request 请求。
server 端通过 identifier 来判断重复的请求。如果这个时候还没有发送 reply ，那么等消息处理完，统一再发送 reply。

**lost reply message*
如果一个请求，server 端已经处理过了，但是reply  消息没有送到 client 端。那么 client 端会通过超时机制再一次发送这个 request。
如果是一个 idempotent 操作，则可以重复执行，因为不会改变结果。

**History**
如果有些操作不是 idempotent，那么需要一个历史记录来记录已经执行过得操作，如果有重复的 request ，则只需把记录里的 reply 再发送一遍。
但是这个 history 的容量也是一个问题

**相关的协议：**
1. The *request* protocol (R)
2. The *request-reply* protocol (RR)
3. The *request-reply-acknowledge* protocol （RRA）
client request - server reply - client acknowledge

## TCP实现
1. TCP stream 允许 request 的参数数量为任意大小
2. 用 TCP 协议来实现通信业更加的可靠，这样就不需要 request-reply 协议来实现重新发送消息以及过滤重复请求。
3. 另外 TCP 的流控制机制也能使参数大小过大时，不需要额外的处理措施
4. 总之用 TCP 可以是 request-reply 协议的机制更加简化（因为 TCP 本身是复杂的，底层复杂一点，上层就简单了）

**HTTP: RR协议的一种实现**
HTTP有固定的一系列操作（GET,PUT, POST等等）

HTTP协议中的两种功能
1. Content negotiation:
client端发送消息时，可以说明可以接手那种格式的数据，这样server端可以选在最适合用户的数据格式
2. Authetication:
如果 client 试图接入密码保护的信息，那么 server 会向 client 端发出一个 challenge。client收到这个challenge以后，输入包含用户名密码的消息，再发送一个 request ，去取得权限。

如果每次在 client 端收到 reply 以后就关闭连接，代价会很高。所以 HTTP 提供了持久连接( persistent connection )，

# Remote Procedure Call

## Design issues for RPC 

### Programming with interface
在模块化编程中
1. 接口定义了模块中可以调用的方法和可以接入的变量，同时隐藏了除此之外的信息。
2. 接口保持不变的同时，可以改变模块的实现，这样不会影响到别的用户使用这个模块

在分布式系统中
1. 程序猿可以不用关心模块的具体的实现，只要关心抽象以后的接口就好了
2. 那么在分布式系统中，程序猿则可以不用关心模块所用的语言和底层的平台（解决了不同平台的问题）
3. 只要和原来的接口兼容，不仅实现可以改变，接口也是可以升级的

需要注意的是：
1. 在一个进程中运行的模块，无法接入到另一个进程中的变量，所以服务接口不能直接地去接入变量，可以用个 getter 和 setter 接口。
2. 参数只能传值，不能再传指针

Interface definition languages:
不同的语言写的操作可以相互调用。

### RPC call semantics
一共有三种fault tolerance的措施：
1. retransmit request 
2. duplicate filtering 
3. re-execute procedure
4. retransmit reply 

根据是否采用这些容错措施可以分类得到以下语意sematics:
1. Maybe sematics
No fault-tolerance measures are applied，在没用容错措施的情况下
    - Omission failure 如果请求或者回复的消息丢失了，（请求没有被执行/请求执行了）
    - Crash failure 如果server端挂掉了（同样宕机可以发生在请求执行前后）
所以只有可以接受请求可能会失败的应用可以选择 Maybe sematics
2. At-least-one
retransmit request + re-execute procedure
通过重复发送请求来保证请求至少被执行了一次，可以避免上述的 Omission failure，但是还是会发生一下的失败：
    - arbitrary failure：多次执行请求，如果不是idempotent，结果可能会不对
    - crash failure
3. At-most-one semantics
retransmit request + duplicate filtering +  retransmit reply
这种情况下client要么收到一个请求执行了一次的回复，要么收到请求没有执行的消息

### Transparency
RPC服务需要像调用本地服务一样，所以需要把 marshalling 和 message-passing 的过程封装起来，超时重新发送请求也是对 client 隐藏的。(middleware 可以完成这样的transparency)

但是 RPC 总是要比本地的服务更加的不稳定，如果 client 端没有收到请求， client 端没有办法判断是断网的原因，还是远端服务挂掉了。

延时也是一个问题。 client 端应该终止那些耗时过长的 request ，并且恢复到发出请求之前的状态。

RPC不支持参数是指针的情况。

## Implementation of RPC 
![client and server stub procedure in RPC](http://images.slideplayer.com/26/8785084/slides/slide_39.jpg "client and server stub procedure in RPC")

在 client 端，stub procedure 就像一个本地的服务一样，但是它的作用不是执行请求，而是把 procedure identifier 和 arguments marshal 成一个 request message，然后通过communication module把请求传到client端。

Server 端的 dispatcher 的作用是通过请求中的 procedure identifier 来选择哪个 stub procedure 来执行这个请求。同样在 server 端 stub procedure 的作用是 unmarshal 。

communication module 负责执行容错措施（retransmit request等等）

## Service Descripter
// 这是PPT上的内容
Service Descripter 用来在 client 生成一个 proxy ，也就是上面的 stub 。
例如在作业中用的， JAX-WS 框架中， WSDL 格式的 descripter 可以用来生成客户端的 proxy。

rpc 的方法有一系列的参数，所以 rpc 的耦合度很高的系统。当服务器端的 service descripter 改变了，那么客户端也需要改变。

# Remote Method Invocation

RMI 和 RPC 的相同点：
1. support programming with interface 支持接口
2. constructed on the top of request-reply protocols 建立在RR协议之上，可选择的语意相同
3. similar level of transparent 

不同之处：
1. RMI 可以充分利用面向对象编程，可以使用 object, class, inheritance
2. 支持参数是一个对象的指针

## Design issue for RMI

### The object model

1. 对象的指针：对象指针可以被赋值，可以被当做参数传动，也可以当做函数的返回值
2. 接口：定义方法的参数，返回值，异常，而不暴露底层的实现
3. 动作 (Action) ：就是调用方法，有三种结果
  - 目标对象的状态改变了
  - 新的对象被创造出来
  - 可以触发更多的方法
4. 异常
5. 垃圾回收

### Distributed Objects
分布式系统中的一种是现实 client-server 架构。这种架构下，对象是在 server 端的， client 端可以通过 Remote Method Invocation 来调用对象的方法。

在Java中一个对象中的变量 (field?)是可以直接调用的，但是在分布式系统中，对象中的变量的只能通过方法来调用。(封装)

server 端的对象可能被复制保存在 client 端的 cache 里，这样可以本地调用这个方法。

### Distributed object model
在分布式系统中，不同的进程中有各自的对象，有些对象可以被远程调用，有些对象只能本地调用。

remote object：
可以被用来远程调用的对象，叫做remote object

remote objectreference：
如果一个对象，可以使用另一个对象的远程对象指针，那么这个对象就可以调用另外这个对象。

remote interface：
每个remote object都有自己的remote interface，这个接口指定了对象的哪些方法可以被远程调用。

### Actions in distributed object system
就是调用远程对象的方法呗

分布式系统中的垃圾回收机制：本地的垃圾回收器，一个模块来与别的进程同步这些信息

异常：任何远程调用都可能会失败

## RMI的实现

![proxy and skeleton in RMI](http://image.slidesharecdn.com/rmi-140910012015-phpapp02/95/distributes-objects-and-rmi-19-638.jpg?cb=1410312096 "proxy and skeleton in RMI")

### Communication Module
1. 两个通信模块负责实现 request-reply 协议
2. 负责在 client 和服务器之间传输 request 和 reply message ，
3. 也负责实现调用语意 semantics ，也就是容错机制等等

### Remote reference module
这个模块是用来把 remote object reference 转换为local reference的。有一张remote object table来记录相应的配对。

这个模块在后面提到的 RMI Software 中实现 marshall/unmarshall 的时候被调用。

### Servant
Servant是被远程调用的对象的实例

### The RMI software
1. Proxy（client端）:
作用就是包装远程调用的过程，使之对 client 端透明。其实就是 RPC 中的 stub procedure ，来实现 marshall arguement/unmarshall result
2. Dispather（server端）:
每一个类都有一个 Dispather 和 一个 skeleton.
同样和 RPC 差不多的是， Dispather 在收到请求以后，通过 operationID 来选择相应的 skeleton 中的方法来执行
3. Skeleton（server端）:
用来 unmarshall request ，并调用 servant 中相应的方法来执行。

上述的proxy，dispatcher，skeleton都是由interface compiler生成的。

### Dynamic Invocation
有时候，remote object 的 remote interface 不可用 ，这个时候 client 端静态的 proxy 就无法调用到远端的对象了。

动态调用提供了一个类似于 doOperation 的方法来调用对象，跳过了 interface 。这时候这个方法需要 client 提供 remote object reference ，方法名，和相应的参数。

同样在 serve r端，也可以通过 Dynamic Skeleton 来解决接口不可用的问题

### Server and Client programs
除了上述所及的组建以外：

在 server 端，需要一个 initialization 模块，这个模块负责新建至少一个 servant ，来对 client 端的 request 进行响应。

在 client 端，每一个 remote object 都会有一个相应的 proxy ，可以通过一个 binder 来确定每个 proxy 的相应对象的 remote object reference . 

注意的是，接口是没有构造器，我们通过调用接口方法来调用远端对象的时候，是无法通过构造器来新建的。Servant 要么在 initialization section 中创建，要么通过 remote invocation 的方式来创建。 factory method 通常用来指那些通过调用方法来创建 servant 的方法。factor object 则是那些具有 factory method 的对象。

### binder
Binders allow an object to be named and registered.

用来取得remote object reference的的方式。

Registry：在第一次调用远程对象之前，需要注册一下。然后形成一个 Binder，绑定了 remote object reference 和它的 name 。

Java uses the rmiregistry
CORBA uses the CORBA Naming Service

### Java RMI Registry
1. On the clientside, the RMI Registry is accessed through the static class Naming. It provides the method lookup() that a client uses to query a registry.
客户端通过访问 Naming 静态类来访问 Registry，调用 Lookup() 方法来查找一个注册信息
2. The registry is not the only source of remote object references. A remote method may return a remote reference.
Registry 不是唯一返回 reference 的方法，有些方法也可以返回一个 reference

- The registry is only used on start up. 
- The server names the remote object and each type of client does a single lookup.

### Threads
客户端用多线程的方式来执行每个remote invocation





