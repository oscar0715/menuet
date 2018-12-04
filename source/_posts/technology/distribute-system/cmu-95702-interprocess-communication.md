---
title: CMU 95702 - Interprocess Communication   
tags:
  - Distribution-System
categories:
  - Technology
  - Distribution-System
date: 2016-10-17 15:00:45
---
CMU 95702 - Distributed System

Interprocess Communication  
<!-- more -->

***

# Middleware layers
Top - Down Structure
![](http://images.slideplayer.com/26/8686098/slides/slide_3.jpg)

中间两部分属于 Middleware Layer
本文讲的是第三部分

>Middleware provides a higher level programming abstraction for the development of distributed systems. (Coulouris textbook)

# The API for the Internet protocols
## characteristic of interprocess communication

### 同步异步
Two message communication operations
1. send
2. receive

在消息的接收方都有一个队列 (queue)，每个send操作都会把一个message加入这个queue中，在receive操作之后则将这个message从queue中删除
1. Synchronous communication
这种情况下，消息的发送方和接收方对于每一条消息是同步的，send操作和receive操作都是阻塞操作 (blocking operation) 。当send操作发出，发送方的这个thread就会被阻塞 (block) ，直到接收方完成receive的操作
2. asynchronous communication
在非同步情况下，发送方的这个thread在接收方的接收队列中加入这个消息以后就可以继续进行，无需等到receive操作完成。

### 消息的目的地
（Internet address, local port）

### Reliability / Ordering

## Sockets
UDP和TCP都是建立在socket之上的。进程间的通信也是由不同进程间的socket通信来完成的。

## UDP datagram communication
通过UDP发送的消息不需要acknowledgement，所以消息有可能没有抵达目的地。
消息的发送方要发送消息时，需要建立一个socket，这个socket绑定了接收方的地址 (Internet address)和端口 (local port)

**消息的大小：**
IP协议允许packet的大小可达到$2^{16}$bytes。不同的应用application也有不同的要求。
如果消息超过了最大的消息大小，那么消息需要被拆成很多个chunk

**阻塞blocking：**
通常send操作为非阻塞，receive操作为阻塞

**超时timeout：**
因为receive是阻塞的，不能一直让它等着呀，所以要设置一个超时时间。

**失败failure model：**
通信的可靠性有两个要点：integrity 和 validity。
Integrity要求接收到的消息是完整的，非重复的。

UDP的数据包datagram的失败有两种情况
1. Omission failure：校验和checksum没通过
2. Ordering：接收到的时候和发送的时候顺序不一样了

PPT上的Orz:
Failure Model of UDP Request Reply Protocol
A UDP style doOperation may timeout while waiting. (接收端超时) Then:
- return to caller passing an error message. 
接收方没收到 request 造成的超时，返回一个 error 消息
- but perhaps the request was received and the response was lost, so, we might write the client to try and try until convinced that the receiver is down. 
如果接收方是收到了 request 的，之时返回发送方的 response 丢失了，那发送方就一直尝试发送，知道收到 response 为止
- In the case where we retransmit messages the server may receive duplicates
如果发送方一直发送一个消息，那么可能会有接收方重复收消息的情况，我们需要对这种情况进行处理

Failure Model for Handling Duplicates
Suppose the server receives a duplicate messages. 
The protocol may be designed so that either:
1. it re-computes the reply
重新执行操作，并返回一个新的reply
2. it returns a duplicate reply from its history of previous replies.
An acknowledgement from the client may be used to clear the history
接收方有一个历史记录来保存对每一个请求的 reply，如果收到重复请求的时候，就去历史记录中找出执行过的 reply 返回给发送方。

**UDP的应用**
DNS / Voice over IP
UDP的特点是不需要有overhead来储存那些保证消息一定要送达的东西。（我觉得就是因为它的send是非 blocking的，可以看成非连接通信）

**JAVA 中UDP数据报的接口：**
Datagram Packet
包含消息的byte数组 + 消息的长度 + Internet address + Port number


## TCP Stream Communication
TCP stream通信采用了acknowledgement scheme.

**什么是 acknowledgement scheme呢**
举个例子
1. 消息的发送方在发出一个IP packet之后，会对这个packet留下一个记录Record。
2. 如果发送方在timeout规定的时间内，没有收到接收方的接受确认 acknowledgement ，那么发送方会重新发送消息

**Flow control流控制**
TCP 会控制进程 (stream) 中读入或写入消息的速度。 如果读得太快的了，那么进程就会被暂时阻塞

**Message duplicate and ordering**
每个packet都有一个identifier。通过这个identifier，接收方就能侦察到重复的包，然后拒接收，也可以对包进行排序。

**Message destination**
TCP与UDP不同，TCP通信是一个有连接的，要建立一个connection。
1. 要在接收方和发送方之间建立一个连接connection，需要处理一系列的通信进程。
2. 一旦这个连接connection建立了，那么接收方和发送方就不用管Internet address和local port，只要向这个连接的stream中读和写消息就好了（因为这个连接的两头就是发送方和接收方的）

要建立一个连接
1. client要发出一个request
2. server接收到以后，则发送一个accept给client
3. 这个过程相对于UDP来说是一个很麻烦的overhead

在Socket Model中
1. 在server端有一个和server port绑定的listening socket，用来接收client端的request
2. 然后client端向server端发送一个请求，server端accept以后，就会建立一个新的socket与client端进行通信
3. 与此同时，那个listening socket一直存在来接收来自别的client的request请求

在server和client端通信的时候需要注意的点：
1. match of data items: 
server 端和 client 端的数据要统一， client 端发送的是int，server 端不能以 double 来接收
2. blocking
TCP 流通信，同样也是在 server 端有一个 queue 来存储从 client 传过来的消息。如果进程无法从 queue 中取到想要的数据，进程会 block 住，直到这个数据被取到了。这个时候如果 server 端的队列满了的话，client 也会被 block 住，不能再发送新的消息。
3. Threads
用多线程来实现 server 与多个 client 来沟通。

**Failure Model**
1. Integrity
TCP stream 用校验和 (checksum) 来侦查损坏的数据包；用 sequence number 来侦查重复的包
2. Validity
timeouts/retransmission，超时和重新发送的机制

**TCP 的应用**
HTTP/FTP/Telnet/SMTP
  
# External Data Representation
不是所有的机器存储数据的格式都是一样的，所以面临怎么处理数据格式的问题

可以按字面意思理解，就是数据传到程序意外的地方，怎么来展现

## Passing values Problem 
Passing values over a network may be problematic.

If the two sides differ in the way they represent data.
Differ in :
1. Big-endian, little-endian byte ordering 
2. Floating point representation 
3. Character encodings (ASCII, UTF-8, Unicode, EBCDIC)

Solution:
1. Have both sides agree on an external representation (大家都说英语这个例子不错 / html) 就是发送的时候用一个通用的形式，接收方接收到以后，看情况转变为自己local需要的形式
2. Transmit in the sender’s format along with an indication of the format used. The receiver converts to its form. （CORBA）
发送方用自己的格式发送，并对这个格式进行说明，接收方自行把这个格式转变为自己需要的格式。

## Marshalling

1. External data representation 
An agreed **standard** for the representation of **data structures** and primitive values
2. Marshalling 
The process of taking a collection of data items and **assembling** them into a **form** suitable for transmission in a message
3. Unmarshalling 
The process of **disassembling** them on arrival into an equivalent representation at the destination

>The marshalling and unmarshalling are usually carried out by the middleware layer

## Interoperability concern

### Big/Little Endian
小端法，大端法（诶，513会讲到的，还是要找时间学一下的）

Example:

`3` in bytes: `00000000000000000000000000000011`

Little-Endian approach:
Write 00000011, Then 00000000, Then 00000000, Then 00000000

Big-Endian approach:
Write 00000000, Then 00000000, Then 00000000, Then 00000011

### Binary vs. Unicode
`int j = 3;`

1. Binary Presentation: `00000000000000000000000000000011`
2. Unicode: `0000000000110011`


# Approaches to external data representation
1. CORBA’s CDR (Common Data Representation)
Binary data may be used by **different programming languages**.
Both sides have the **IDL** beforehand. (Interface description language)
2. Java and .Net Remoting Object Serialization
    - platform specific and binary
    - Java’sserialization
        Use Java serialization to marshal and
        un-marshal to a network or to storage. No IDL used.
3. XML
XML is a textual format, verbose and slow when compared to binary but interoperable. JSON is like XML but more compact.

## CORBA in a Nutshell
CORBA Common Data Representation (CDR) for constructed types：
1. sequence
2. string
3. array
4. struct
5. enumerated
6. union

- Can be used by a variety of programming languages.
- The data is represented in binary form.
- Values are transmitted in sender’s byte ordering which is specified in each message.
- May be used for arguments or return values in RMI

## Java Serialization
`public class Person implements Serializable {}`
- Serialization refers to the activity of flattening an object or even a connected set of objects
- May be used to store an object to disk
可以用来进行数据持久化，就是存在硬盘上
- May be used to transmit an object as an argument or return value in Java RMI
- The serialized object holds Class information as well as object instance data
保存类相关的信息，和一个类实例信息
- There is enough class information passed to allow Java to load the appropriate class at runtime.
- It may not know before hand what type of object to expect

## Web Service use of XML
1. Need an agreement on what character encoding to use, 
    e.g., an HTTP header might say Content-Type: text/xml; charset:ISO-8859-1;
2. Binary data (e.g. pictures and encrypted elements) may be represented
in Base64 notation.
3. An XSDL document may be used to describes the structure and type of the data.

## Web Service use of JSON
{% codeblock lang:JSON %}
{ 
    "person" : { "name" : "Smith"
               "place":"London"
               "year":1934
               }
}
{% endcodeblock %}

1. UTF-8 is the standard encoding.

# Pass Pointers
Object will be meaningless in another machine

## Representation of a Remote Object Reference
当client 要调用server端的远程对象时，需要一个reference去指定要调用哪个对象，这个就是remote object reference
1. Internet address 
2. port number
3. time
4. object number
5. interface of remote object





