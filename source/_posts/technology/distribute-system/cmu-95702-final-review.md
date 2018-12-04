---
title: CMU 95702 - Final Reveiew
tags:
  - Distribution-System
categories:
  - Technology
  - Distribution-System
date: 2016-12-8 17:21:45
---
CMU 95702 - Distributed System

Final Reveiew
期末复习！
<!-- more -->

***

# Chapter 1 Introduction 

## Characterization of distributed systems.
1. Components are located on networked computers and execute concurrently.
组成部件分布在网络上不同的电脑上，并且同时运行
2. Components communicate and coordinate only by passing messages
各个部件是通过放消息来沟通和协调的
3. Time differs on each system.
网络中不同的电脑和不同的系统上，时间是不一样的
4. Many challenges associated with distributed systems are not new
问题多多啊

## Four styles of application integration.
•  Shared files
•  Shared database
•  Procedure Call or Method Invocation
•  Messaging through a third party

## Synchronous and asynchronous communication.

In asynchronous systems, the client makes a call and continues with other business. Perhaps it provides a means for a response. 
非同步情况下，客户端发出一个请求以后，不管有没有回应可以现在别的事情

In synchronous systems, the client calls, blocks and waits for the response.
同步情况下，客户端发出一个请求以后，就等着不干别的事情，block住了

## Invocation, Indirect communication
Remote invocation：就是 RPC RMI 这种

Indirect communication (less tightly coupled and involving a third party)
Communicating to a group be sending a message to a group identifier
- Publish-subscribe: (distributed event based systems) routes messages to interested parties. One-to-many style of communication. 一对多发消息
- Message queues: (channels) for point-to-point messaging.  点对点发消息
- Tuple spaces allows for the placement and withdrawal of structured sequences of data. 

## Time and space coupling
时间耦合就是双方同时参与
Coupled in time (both parties exist during interaction) 

空间耦合就是知道和谁交互
Coupled in space (parties likely know who they are interacting with) 

## Layered architecture

分层架构：
•  applications and services 应用和服务
•  middleware
•  operating system

再下面是 hardware 硬件

## Tiered architecture
分列架构：
•   usually applied to the applications and services layer 通常在应用层分列
•   presentation logic, business logic, and data logic 三个 tier
•   Main driver: To promote separation of concerns.

## Software engineering principle: Separation of concerns

Transparency 

## Architectural patterns: Proxy pattern, brokerage pattern
The proxy pattern：
client 调用远端对象在本地的接口，接口背后的细节是对 client 透明的。

The brokerage pattern：
service provider, service requestor and service broker 三方构成了 brokerage pattern.

## Challenges in constructing distributed systems
1. Heterogeneity 
2. Security
3. Scalability
4. Failure handling
5. Concurrency
6. Openness

<br>
***
<br>

# Chapter 2 System Models
## Architectural Models
An architectural model of a distributed system is concerned with the placement of its parts and the relationships between them.

## Fundamentals Models
A model contains only the essential ingredients needed to understand and reason about some aspects of a system’s behavior.

## Agreement in Pepperland pg. 65
//  NULL

## Failure detection in Peperland pg. 69 
//  NULL

## JEE Servlets

Java Enterprise Edition (JEE) 是用 Java 来构建分布式系统的平台

HttpServlet 是用来处理 HTTP 请求的类

Web Container 容器
• Communication support
• Instantiating classes, passing arguments 就这个知道点，用来新建类
• Multithreading (new thread for each request)
• Security

Servlet 的生命周期: 
![Servlet-Life-Cycle](http://jitendrazaa.com/blog/wp-content/uploads/2011/02/Servlet-Life-Cycle.jpg "Servlet-Life-Cycle.")

↑上图描述了 Servlet 的生命周期
Web Container 负责发起 load Servlet class, init(), service(), destroy()

在调用 init() 方法的时候，可能要做的事情是
• Initialize a database
• Open file(s)
• Register with other objects 
• Connect (sockets) to otherservices

调用 service() 也就是可能用到以下 HTTP Method
• doGet
• doPost
• doPut
• doDelete 
• doHead

destroy(): 
• Close a database or file 
• Clean up relationships to other objects
• Close (sockets)

一个 Servlet 实例对象处理多线程的情况（总之 Container 很忙）: 
![One servlet object, multiple threads](https://www.safaribooksonline.com/library/view/head-first-servlets/9780596516680/httpatomoreillycomsourceoreillyimages1378412.png.jpg "One servlet object, multiple threads")


## Java Server Pages (JSP)
1. Provides a higher-level abstraction to servlets
	- JSP gets compiled into servlets
2. JSP is HTML, with Java code embedded in it. 可以在 HTML 里写 Java，通常用来接收从 Controller 或者 View 传过来的值
3. 

## Model View Controller 

![JEE MVC](http://ogy9sznv2.bkt.clouddn.com/final-review/01.png "JEE MVC")

从 Container 到 Controller， 然后进入 MVC 交互，再从 View 离开，回到 Container

## WWW

## HTTP Protocol

### HTTP 
Hypertext Transfer Protocol (HTTP):
1. Http 是一个应用层协议. 作用在在浏览器和网站交互的时候

2. Request / Response Pattern
	- Client: 客户端和服务端简历连接，并发送一个请求（request），然后等待（wait）
	- Server: 服务端处理收到的请求，并在之前建立好的连接上返回一个回应(response)

3. Coupled in time 时间耦合

4. Coupled in space 空间耦合
	- Both client and server must know the address of each other
	- 客户端和服务端互相知道地址，然后进行点对点沟通

5. Can also be used for Request / Acknowledge

### HTTP Request 
``` xml
// General Format
<method> <resource identifier> <HTTP Version>  
[<Header>: <value>] 
...
[<Header>: <value>] 
a blank line
[entity body]

// Example
GET /course/95-702/ HTTP/1.1 
Host: www.andrew.cmu.edu
User-Agent: Joe typing 
Accept: text/html
This line intentionally left blank

```

1. Method 方法
	– GET, PUT, DELETE, HEAD, POST
2. Resource identifier 指定了目标资源的名字
	- 如果是 Get 方法，Resource identifier 后面加 name=value 的键值对来传递变量， 用‘&’作间隔.
	- 如果是 POST 方法， 键值对则包含在 [entity body] 里面
3. HTTP version identifies the protocol used by the client.

### HTTP Response  
``` xml
// General Format
<HTTP Version> <Status>  
[<Header>: <value>] 
...
[<Header>: <value>] 
a blank line
[response body]

// Example
HTTP/1.1 200 OK
Date: Mon, 13 Jan 2014 15:43:08 GMT
Server: Apache/1.3.39 (Unix) mod_throttle/3.1.2 ... 
Set-Cookie: webstats-cmu=cmu128.2.87.50.8400; ... 
Last-Modified: Sun, 12 Jan 2014 21:46:30 GMT 
Accept-Ranges : bytes
Content-Length: 9014
Content-Type: text/html
This line intentionally left blank
<HTML>
<HEAD>
<META http-equiv="Content-Type" content="text/html; charset=UTF-8"> ...

```

Status indicates the result of the request. 
常见的状态码:
• 301 Moved Permanently 
• 400 Bad Request
• 401 Unauthorized
• 404 Not found
• 500 Internal Server Error (server program throws an uncaught exception.)

## Safe operations, Idempotent operations

Safe: 不改变服务器上资源的状态 
• GET: 应该只用来获取数据
• Head: Requests headers only, but not content

Idempotence: 多次执行的结果都是一样的
• GET
• PUT: PUT 是对整个资源操作，添加或者替换; 要做 PUT 操作的时候，必须知道资源的 ID
• DELETE
•  HEAD

另外， POST 比较特殊，既不是 Safe ，也不是 Idempotent 
• 做 POST 操作的时候你可能不知道资源的 ID.
• 用来 更改某个资源 / 提交 Query
• 比如，你点击 Purchase 两次，可能就完成了两次购买（原因应该就在于不知道 ID ）


## URI's
Uniform Resource Identifiers (URI)
• UR Locators (URL): http://cmu.edu/heinz
*scheme://domain:port/path?query_string#fragment_id*
• UR Names (URN) : urn:isbn:0132143011
*urn:namespace_identifier:namespace_specific_string*

## HTML
Hypertext Markup Language (HTML)

<br>
***
<br>

# Cloudlet article "Mobile Computing the Next Decade"

<br>
***
<br>
# Chapter 3 Networking and Internetworking


## Protocol layers

先上图：

!["Protocol layers"](https://people.cs.pitt.edu/~xianeizhang/notes/net_img/layer.png "Protocol layers")

上图中的网络层课件上写的是 Internet Layer / 数据链路层课件上是 Data Link Layer


## Common Application Protocols

列举一些重要的应用层协议:

![Application Protocols](http://ogy9sznv2.bkt.clouddn.com/final-review/02.png "Application Protocols")


## addressing 

### MAC
Media Access Control address

1. 每一台机器都有一个 Mac 地址，是由生产商设置的一个固定的地址
2. 笔记本电脑有：
	• Ethernet: 60:fb:42:ff:fe:f8:b5:08 以太网 Mac 地址
	• Wifi: 60:fb:42:f8:b5:08 Wifi Mac 地址
	• Bluetooth: 60:fb:42:72:08:4c 蓝牙 Mac 地址

MAC addresses are used to pass messages around **a single physical network* segment*
Mac 地址是用来在一个物理网络内部传输消息的

### Internet Protocol
IP 地址 

IP addresses are used to pass messages around **between networks**
IP 地址是用来在不同的网络中传递消息的

IP 地址哪儿来的呢？
• Static: from a network administrator
• Dynamic: Dynamic Host Configuration Protocol （DHCP）

An internetwork is an interconnected collection of networks.
整个互联网是一系列小网络构成的

The Internet Protocol (IP) is the key tool used today to build scalable, heterogeneous internetworks
IP 协议就是建立一个可拓展的异质的互联网的关键所在

### IP Addressing 

Network prefix:
1. 每一个 IP 数据报都有目标主机的 IP 地址
2. IP 地址的 network prefix 唯一指定了互联网（larger Internet）中的一个小网络（single network）
3. 同一个网络中的主机和路由器的 network prefix 都是相通的，他们之间相互沟通可以用 Mac 地址来沟通
4. 所以 IP 地址的 network prefix 就是用来确定一个小网络的
5. 互联网中的每一个小网络都至少有一个路由器，也至少连接到一个其他的网络；这个路由器可以和其他网络中的主机或者路由器交换数据报

### Subnet Addressing 
Subnetting allows an organization to break their address space into smaller networks.

子网就是在一个组织的小网络内继续分成更小的部分

Creating a subnet by dividing the host identifier:
![Creating a subnet by dividing the host identifier](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e5/Subnetting_operation.svg/300px-Subnetting_operation.svg.png "")


### ports and port assignment

Ports：
• Port is a transport-layer software construct
• Port is a 16 bit integer

端口的分配 assignment：
• 0 - 1023 Well Known Ports
Should not be used without IANA registration – Loosely, system-type-services ports
• 1024 - 49151 Registered Ports
Also should not be used without IANA registration – Loosely, application-level ports
• 49152 - 65535 Dynamic and/or Private Ports 
Anything

## DHCP, ARP protocols

### DHCP
DHCP 就是动态分配 IP 地址的过程

先上图，DHCP的流程：
![DHCP 流程](https://docs.oracle.com/cd/E19455-01/806-5529/images/dhcp-diag.epsi.gif "DHCP 流程")

DHCP offer，offer里提供了哪些东西:
• IP Address being offerred to the client
• Subnet mask for the local network
• Gateway IP (default router out of this network) 
• DNS servers
• Lease duration

### Address Resolution Protocal ARP 

ARP 是用来获取另一台机器的 MAC 地址的协议

1. Host1 想联系 Host2
2. H1 检查一下自己有没有 Host2 的 IP/Mac 地址
	- 如果有的话就用Mac地址联系了
3. 如果没有的话 H1 就广播 H2 的 IP 地址，问一句这是谁的 IP 
	- 广播的 MAC 用的是 FF:FF:FF:FF:FF:FF 全是 1
4. 然后 H2 收到了这个 IP， 发现是自己的 IP，于是就给 H1 发送自己的 Mac 地址
5. H1 收到以后，把这个 Mac 地址添加到自己的缓存里面

## Datagrams
// null

## routing
路由表：
Routers maintain routing tables, indicating the best paths to other networks.

定时发送：
At least every 30 seconds, each router will send its routing table to each of its neighbors.

接收后更新：
When receiving a neighbor’s table, a router will update its own tables.

## RIP algorithm 
Routing Information Protocol (RIP)

如果从线路 n 接收到隔壁的路由表以后，就要更新自己的路由表，遵循以下原则：
1. 如果到目标 host 的这条路经过自己，那么不更新，因为我更清楚
2. 如果是要更新的，那么把从 cost 加 1 就好了。 因为这个表是隔壁发过来的，从隔壁走是 cost， 从我这里走就是 cost + 1。 把线路更新成 n 。这样这个隔壁过来的表就更新好了，接下来，把表里的东西往自己的路由表中添加。
4. 如果这是一个新的目标主机，那么这个新的目标主机添加到自己的路由表中
5. 如果不是一个新的目标主机，那么只有比现在 cost 小的路径才更新本地的路由表

说起来很麻烦，其实直接看表反而是很清楚的

## TCP and UDP Comparasion

![Comparasion](http://rtcmagazine.com/files/images/4061/rtc1211p53_large.jpg "Comparasion")

<br>
***
<br>

# Chapter 11 Security and Cryptography
## RSA

## Cryptographic protocols

## Cryptographic notation

## Symmetric key crypto

## Asymmetric key crypto

### Definition 

### Two kinds of Usage 

## Four Scenario
Scenario 1 (like TEA and WWII enigma)
Scenario 2 (Like Needham-Schroeder, Like Kerberos)
Scenario 3 (Authentication
MACs)
Scenario 4 (Like SSL)
http://localhost:4000/distribute-system/cmu-95702-cryptography/#Scenario-1-Like-WWII-amp-TEA

## Programming Java SSL sockets

## Digital signatures
这个不就在 senarios 里面的么
DS 的两个老师都是逻辑不清楚的

<br>
***
<br>

# Chapter 9 Web Services

## Web service design patterns

## Binary schemes and performance

## Coupling in space and time
1. Coupled in time: they must both be ready to interact at a certain moment.
2. Coupled in space: the clients must know the location of the services handling requests. 
3. If a client must provide an ordered list of
typed parameters this is more tightly coupled than one that does not.

## Separation of concerns and examples
1. Separate systems into distinct sections
1. increases modularity
1. promoted by encapsulation and Information hiding
1. Application tiers separates concerns
	e.g., a web site may be based on MVC design
1. Layered architectures separate concerns
	e.g., the layers of HTTP/TCP/IP/Ethernet

## Jon Postel's Law
Be liberal in what you accept.
Be conservative in what you send.

## Productivity and Adaptability
// null

## Web service Design 
Web service API styles: RPC, Message, Resource
Style considerations
REST architectural principles
Linked services pattern
Client server interaction styles

上面的这些知识点都在这里
[Web service Design ](menuet.xyz/distribute-system/cmu-95702-service-design/)

## Odata
// null

<br>
***
<br>

# Chapter 4 Interprocess Communications
Middleware
External Data Representations
CORBA CDR
Java serialization
XML and JSON
UDP based request response protocol
Fault tolerance measures for the UDP request response
RPC Exchange protocols (R, RR, RRA)

上面的这些知识点都在这里
[Interprocess Communication](menuet.xyz/distribute-system/cmu-95702-interprocess-communication/)


<br>
***
<br>

# Article covering The End-To-End Argument

下面链接转到这篇文章的总结：
[The End-To-End Argument](http://menuet.xyz/distribute-system/cmu-95702-end-to-end-Arguments )

<br>
***
<br>

# Android and Mobile Devices

## The rise of mobile platforms
// null

## Building a native application
// null

## Android architecture
// null

## Flow of control
Java -> Any POJO -> main() -> None
Web Application -> Extend HttpServlet -> doGet(), init(), doPost() -> web.xml
Android Application ->  Extend Activity -> onCreate(), onPause(), -> AndriodManifest.xml

## Android UI definition
1. An Android app’s UI is defined 
	– in xml files
	– within the res directory
2. Two editing modes:
	– Design (WYSIWYG) 
	– Text (xml)
3. The complete res directory is compiled from xml into Java classes that define the R object
4. Therefore R. refers to UI elements defined in the res directory
	– If R is undefined, it means the res xml has an error.

## Network latency and AsyncTask

UI can pass off requests that will take a long time to Helper
UI 就是 Android 应用的主界面，如果用户的某个请求需要花很长的时间，那么 UI 就把这个请求交给一个 helper 去完成。这个 helper 可以理解成在后台运行的线程，用户是看不见的，用户只能看见 UI.

When Helper is finished, it can pass the results back to UI
当这个 Helper 完成了这个请求需要执行的任务以后再把结果传到 UI. 这个过程中 UI 可以做别的事情。

AsyncTask is a class that makes doing work in a helper thread easy.
- Create a class that extends AsyncTask 新建一个继承了 AsyncTask 的类
– Implement doInBackground and onPostExecute 这个类有这两个方法
– Instantiate the class, and call its execute method  创建这个类的实例，然后执行

<br>
***
<br>

# Chapter 5 Remote Method Invocation

## Interface Definition Language (IDL)
1. Definition: An interface definition language (IDL) provides a notation for defining interfaces in which each of the parameters of a method may be described as for input or output in addition to having its type specified.
接口描述语言，定义了接口的每个方法的入参或者输出的格式
2. These may be used to allow objects written in different languages to invoke one another. Think Code Generation
定义了 IDL 以后，对象可以用不同的语言来写，但是还是能够互相调用


## Goals of Java RMI
1. Distributed Java
2. Almost the same syntax and semantics used by non-distributed applications
RMI 的最终目标就是使分布式系统下的语意和本地语意相近
3. Promote separation of concerns – the implementation is separate from the interface
4. The transport layer is TCP/IP

## EJB remote and local interfaces
1. Enterprise Java Beans (EJB’s) provide a remote and local interface.
2. EJB’s are a component based middleware technology. 
EJB 是一项中间层技术
3. EJB’s live in a container. The container provides a managed server side hosting environment ensuring that non-functional properties are achieved. 
EJB 是在容器中的，容器提供了一种可管理的服务端环境，可以保证一些非功能性的性质
4. Middleware supporting the container pattern is called an application server.

下面链接转到这篇文章的总结可以看剩下的知识点：
[Remote Invocation](http://menuet.xyz/distribute-system/cmu-95702-remote-invocation)

Generic RMI component interactions
Registries
Java RMI code examples

<br>
***
<br>

# Chapter 12 Distributed File Systems
	NFS, AFS

## NFS
1. Goal: Be unsurprising and look like a UNIX FS. 包装成 UNIX 系统
2. Goal: Implement full POSIX API. The Portable Operating System Interface is an IEEE family of standards that describe how Unix like Operating Systems should behave.
3. Goal: Your files are available from any machine. 
4. Goal: Distribute the files and we will not have to implement new
protocols. 
5. NFS was originally based on UDP and was stateless. TCP added later.
6. NFS defines a virtual file system. The NFS client pretends to be a real file system but is making RPC calls instead. 
7. NFS uses RPC over TCP or UDP. External requests are translated into RPC calls on the server. The virtual file system module provides access transparency.

## AFS 
1. Unlike NFS, the most important design goal is scalability.
2. To achieve scalability, whole files are cached in client nodes. Why does this help with scalability? We reduce client server interactions.
3. A client cache would typically hold several hundreds of files most recently used on that computer. The cache is permanent, surviving reboots.
4. When the client opens a file, the cache is examined and used if the file is available there.

1. If the client code tries to open a file the client cache is tried first. If not there, a server is located and the server is called for the file.
想要打开一个文件，先到缓存中找，找不到再去远程的服务器
2. The copy is stored on the client side and is opened. 
然后服务器会把文件存到 client 端，在 client 端打开它
3. Subsequent reads and writes hit the copy on the client.
所有的读写都发生在本地
4. When the client closes the file - if the files has changed it is sent back to the server. The client side copy is retained for possible more use.
在 client 关闭文件的时候，如果文件被修改了，那就把改动发回到服务器端，本地的文件继续被保存下来。
5. Consider UNIX commands and libraries copied to the client. Consider files only used by a single user. These last two cases represent the vast majority of cases. 其实很多文件都是单个用户用的
6.  Gain: Your files are available from any workstation.
7.  Principle: Make the common case fast. See Amdahl’s Law. Measurements show only 0.4% percent of changed files were updated by more than one user during one week. 

<br>
***
<br>

# Chapter 21 Google Case Study

## GFS

### Requirements of Google File System (GFS)
Goal:
1. Run reliably with component failures. 
2. Solve problems that Google needs solved – not a massive number of files but massively large files are common. Write once, append, read many times.
Google 文件的特点通常是只写一次，会有一些 append，大多数是读的
3. Access is dominated by long sequential streaming reads and sequential appends. No need for caching on the client. 不需要存在客户端
4. Throughput more important than latency. 高吞吐的重要性大于低延迟
5. Think of very large files each holding a very large number of HTML documents scanned from the web. These need read and analyzed. 
6. This is not your everyday use of a distributed file system (NFS and AFS). Not POSIX.


File System:
1. Each file is mapped to a set of fixed size chunks.
2. Each chunk is 64Mb in size.
3. Each cluster has a single master and multiple (usually hundreds) of chunk servers. 单 Master 和 多 Slave
4. Each chunk is replicated on three different chunk servers. 每个 chunk 保存三份
5. The master knows the locations of chunk replicas. Master 维护 chunk 的地址
6. The chunk servers know what replicas they have and are polled by the master on startup. 

### Read Operation
Suppose a client wants to perform a sequential read, processing a very large file from a particular byte offset.
1. The client can compute the chunk index from the byte offset.
客户端计算出文件的 chunk index
2. Client calls master with file name and chunk index.
客户端向 master 提交文件名和 chunk index
3. Master returns chunk identifier and the locations of replicas.
Master 返回给客户端 chunk identifier 和 副本的位置
4. Client makes call on a chunk server for the chunk and it is processed sequentially with no caching. It may ask for and receive several chunks.
Client 再根据 Master 提供的信息去 chunk server 上去找需要的文件

### Write Operation
Suppose a client wants to perform sequential writes to the end of a file.
1. The client can compute the chunk index from the byte offset. This is the chunk holding End Of File.
2. Client calls master with file name and chunk index.
3. Master returns chunk identifier and the locations of replicas. One is designated as the primary.
前两步都是一样的，这里 master return给 client 多了一个指定的 primary
4. The client sends all data to all replicas. The primary coordinates with replicas to update files consistently across replicas.
改动以后，由 primary 负责更新并保持三个副本的 consistency

## MapReduce
综述：
1. Provide a clean abstraction on top of parallelization and fault tolerance.
2. Easy to program. The parallelization and fault tolerance is automatic.
3. Google uses over 12,000 different MapReduce programs over a wide variety of problems. These are often pipelined.

实现：
1. Programmer implements two interfaces: one for mappers and one for reducers.
2. Map takes records from source in the form of key value pairs.
3. Map produces one or more intermediate values along with an output key from the input.
4. When Map is complete, all of the intermediate values for a given output key are combined into a list. The combiners run on the mapper machines.
5. Reduce combines the intermediate values into one or more final values for the same output key (usually one final value per key)
6. The master tries to place the mapper on the same machines as the data or nearby.
 

<br>
***
<br>

# Chapter 6, 9 Indirect Communication and Naming
##  Indirect Messaging
1.  Decoupled in space
	- Sender does not need to know the identify of the receiver(s) and visa-versa
2. Decoupled in time
	-  A component need not even be running
	-  The messaging system can store messages until they are successfully delivered
	-  Reliable delivery is insured
## Java's JMS API
Java Message Service (JMS)
1. An API for performing indirect messaging. 实现间接消息传送
1. It is an abstraction API like JNDI and JDBC. 抽象API
1. Interacts with some Message Oriented Middleware (MOM) 面向消息的中间层
1. JMS is a client-facing interface, meant to abstract way the particulars of any MOM. 
1. In theory, you should have portability of systems written with JMS such that they can work with any MOM.

Compatiblity:
1. JMS systems can interact with non-JMS systems. Such as another system that uses the native messaging system's API
1. The JMS connector is responsible for matching message formats.
JMS connector 负责沟通不同 format 的消息
1. Two vendor options
	- Translate JMS into native mode 
	翻译 JMS 到本地模式
	Can interoperate with non-JMS clients 
	这样 JMS 可以和 非 JMS 的 client 交互
	–  Encapsulate JMS within native mode
	Only JMS clients can communicate
	把本地的包装成 JMS，那么只能和 JMS 交互了

## Point-to-point
Point to point (In JMS called Queues) 
	–  Decoupled
	–  Guaranteed delivery to one and only one message handler
 	–  Might be multiple such handlers that could take a request

## Publish subscribe
Publish Subscribe (In JMS called Topics) –  Decoupled
1.  Similar to a bulletin board or newsgroup 
	- Potentially one to many
1.  One publishes to a topic and zero to many subscribe to it
1.  Guarantees may be configured.
	- If no consumer currently cares about this event it expires 
	- Or messages can be held until consumers are available


## JMS Queues and topics
1. Queues and Topics are administratively managed resources
	- They are not part of the client's software 
	- They are created and destroyed by the MOM, not by the client
	- Queue 和 Topic 是由 administrator 管理的资源，不是客户端的一部分
1. You log into the MOM administrative interface and create or destroy them.
1. Or you define them in a deployment descriptor or an annotation.

## JMS message types
1. Stream
	–  Sequential stream of Java primitive data types.
1. Map
	–  Set of name-value pairs
	• Names are String objects
	• Values are Java primitives (including String)
	–  Can be accessed sequentially or randomly by name.
1. Text
	–  Message is a String object
	• Plain-text message 
	XML messages
1. Object
	–  Serialized Java object
	• Simple if in Java-only environment 
2. Bytes
	–  Stream of un-interpreted bytes.
	–  To encode a message body to match an existing message format


## Message driven beans
// null

<br>
***
<br>

# Chapter 14 Time and Global State
## Internal and external synchronization
1. Internal Synchronization:
	- 两个系统相互之间校对时间
	- Peer systems synchronize among themselves
	- Bound
	• If the synchronization scheme has a bound D (i.e. two clocks will have max D skew)
	• Then all clocks will agree within D.

2. External Synchronization
	系统都和一个外部的更权威的时间校对
	- Systems all synchronize with a more authoritative external clock
	- I.e. UTC (Coordinated Universal Time)
	- Bound:
	• Each clock is accurate to UTC within D

## The network time protocol (NTP)
// null

## Global state terminology 
(histories, happens-before relation, cuts, linearization)

## Lamport clocks
Two rules for assigning logical times to events:
1. Events on one process happen in order 在同一个进程上的事件都是有顺序的
	• Each happens-before the next 先后顺序
	• Increment the logical clock by 1 for each event 后发生的时间的逻辑时间比之前一个多1
2. The passing of messages can be used to indicate happens-before between processes 
	• The sending of the message happens-before the receiving of the message. Send 肯定发生在 Receive 之前
	• Take the larger of the logical clock (+1) value from the process or the message

Total & Causal Ordering
1. Lamport Clocks can be used to coordinate the ordering of distributed events so that each process executes events in the same order (e.g. updating a database)



## Chandy and Lamport Snapshot Algorithm 
1. Assumptions:
	– Reliable channels
2. A channel is a network path from one process to another
	– Every message sent is reliably received 
	•  Once (not more than once)
	•  In order (FIFO)
3. There is a channel (path) between every 2 processes
4. Any process may initiate a snapshot
5. Processes continue as normal while snapshot is taking place.

<br>
***
<br>

# Chapter 16, 17 Local and Distributed Transactions


下面链接转到Transactions的总结:
[Transactions](http://menuet.xyz/distribute-system/cmu-95702-transaction/)

 	ACID Transactions
	Concurrency control
	Recoverable objects
	Locks and deadlock
	Conflicting operations
	Eventual consistency
	Serial equivalence
	Synchronized keyword
	wait and notify
	2PL
	2PC
	CAP Theorem

<br>
***
<br>

# Chapter 19 Mobile and ubiquitous computing

Association
Sensing and Context Awareness
Location sensing
Adaptation
Device awareness / browser detection
Feature detection
Graceful degradation
Progressive enhancement
Responsive web design
Mobile first
Mobile deployment strategies

# Lab 

## Cloudlets  
1. The interaction between a cloudlet and a phone is of what type？
	request/acknowlege/callback

## Quiz 1 on Coulouris Chapter 1
1. In Figure 1.2, the purpose of the Reuters Adapter and the FIX Adapter are to:
	Convert messages from different formats into a common internal format.
2.  Blade servers are:
	minimal computational elements containing for example processing and main memory storage capabilities.
3.  Distributed systems may fail partially. Some failures that have been detected can be **hidden** or **made less severe**. This hiding of failures is called:
	Masking.
4. The three main standard technological components of the world wide web are HTML, URL's and HTTP.

## Quiz 2 on Coulouris Chapter 2  
1. A synchronous distributed system is one where we know
a.  that the time it takes to execute each step in a process has a known upper and lower bound.
b.  that each message sent over a channel is received within a known bounded time.
c.  that each process has a local clock whose drift rate from real time has a known bound.
2.  Interprocess communication refers to a relatively low-level approach that often involves message passing over sockets.
3.  Components and objects are similar but a component is typically deployed to a container and makes all of its dependencies clear.
4. Indirect communication
a.  may include tightly coupled communication with a third party.
b.  may include low level socket programming.
c.  may allow two parties to communicate through a third party.
d. may allow two parties to communicate in a space and time uncoupled way.
5. A tuple space allows one process to place tuples in an area accessible to another process and these processes may be distributed.

## Chapter 3 - Networking & Internetworking  
1. TCP transit protocol 	
http / telnet / ftp / http
2.  If a router has 3 network interfaces, then how many unique subnet masks will it use? 
Each network interface would have a subnet mask, but each would not necessarily be unique.  The router could have 1, 2, or 3, unique masks.
3. If a router has 3 network interfaces, then how many unique network addresses does it have? Three

## Quiz 4 On Crypto  
1. Bitcoin is an example of a cryptographic protocol.
2. Which of the following is true about Java's keystore and truststore？ 
The keystore may be used to hold private keys.
3. Suppose we want to use SSL (TLS) for secure communication. Suppose too that we want to authentic the client with a user name and password. Which one of the following makes the most sense?
Establish a keystore on the server and a truststore on the client.
4. Kerberos is designed to provide authentication between clients and servers in networks that form a single management domain (intranets).
5. Needham-Schroeder protocol 也就是 Kerberos

1. In Project 2, we need for Spy Commander Beggs to communicate with spies in the field. We want to use symmetric key cryptography because it is fast. How do we plan on solving the key exchange problem?  The spy commander provided the keys to his spies before they left Hamburg Hall.
2. The following constructor signature is found in the class TEA.java.
	public TEA(byte[] x)
In this constructor, x is used to reference a sixteen byte key.
3. 

## Quiz 6 on Chapter 4  
1. Java provides the following two types of sockets for interprocess communications:  TCP and UDP sockets.
2. This type of socket might be described as a fire and forget: UDP
3. We examined four ways to represent external data. These four ways were: XML, JSON, CDR, and Java Serialization

## Quiz 7 on Chapter 5  
1. Java RMI runs over TCP/IP
2. In the generic RMI module that we studied, 
there was one proxy on the client for each remote object.
A skeleton on the server associated with the class of the remote object.
3. In Java RMI, after creating a remote object on the server, the server will 
typically, call the bind method on the rmiregistry, passing the object's name and a remote reference. 

## Quiz 10 on Distributed File Systems and Map Reduce  

## Quiz on Web Service Design Styles  
1. One pattern that we reviewed said that we might only publish the addresses of a few root web services. Include the addresses of related services in each response. Let clients parse responses to discover subsequent service URIs. This pattern was called the Linked Services Pattern.
2. Protocol Buffers is a scheme for serializing data into a binary byte stream. There is no type information included in the stream. An IDL is needed to understand the message. Who created protocol buffers? Google
3. Protocol buffers does not include type information in a message, that is, the message is not self describing. Nor does the message contain any pointers to the schema for the message. Why is this done? speed
4. A tolerant reader 
a. is able to process messages that may contain some variety.
b. is very fussy about field size and field type.
5. An asynchronous response handler is less tightly coupled than simple request/response
6. JAX-RS是JAVA EE6 引入的一个新技术。 JAX-RS即Java API for RESTful Web Services，是一个Java 编程语言的应用程序接口，支持按照表述性状态转移（REST）架构风格创建Web服务。JAX-RS使用了Java SE5引入的Java标注来简化Web服务的客户端和服务端的开发和部署。
JAX-WS(Java API for XML Web Services)规范是一组XML web services的JAVA API，JAX-WS允许开发者可以选择RPC-oriented或者message-oriented 来实现自己的web services。
7. RPC API's may have 0 or more parameters. Message API's have 1 parameter
8. Which of the following is true about OData?
b. OData uses URL's to identify resources that are operated upon by HTTP operations.
c. OData may use name value pairs to pass arguments within the URL. 
d. OData takes URL design seriously.
e. OData is based on REST.
9. The "Linked Services Pattern" is 
a. central to the REST architectural style.
b. used by the Odata collection document. 
 Correctc. 
d. the pattern that we use when browsing the web.
e. central to HATEOS.
10. n OData, there is an important document called the "service document". It contains a list of collections.

## Transactions quiz for flipped class  
1. synchronized 的时候, 后来的 thread will be suspended but not rejected.
2. With respect to transactions, which of the following best characterizes 'serializable'? two transactions T1and T2 are running, we want it to appear as if T1 was followed by T2 or T2 was followed  by T1. This is true even though the two transactions may have run concurrently.
3. A transaction has an "all or nothing" characteristic. Which of the following is often used to recover from transactions that only execute partially?  a strategy where only a copy of a resource has been changed. 
4. An interleaving of the operations of transactions in which the combined effect is the same as if the transactions had been performed one at a time in some order is called a serially equivalent interleaving.
5. Consider the following class:
``` Java
public class Account {

     public void makeChangeToValue() throws RemoteException { int value = 0; value = value + 1; }

     public void makeChangeToValue2() throws RemoteException { int value = 0; value = value + 2; }

}
```
If two threads are running (one in makeChangeToValue()  and another in makeChangeToValue2()) then they will each have their own copy of value. That is, they will not interfere with each other. 
6. This approach is an alternative to locking. optimistic concurrency control.

***

Over！