---
title: CMU 95702 - 课程项目总结
tags:
  - Distribution-System
categories:
  - Technology
  - Distribution-System
date: 2016-12-31 16:49:45
---
CMU 95702 - Distributed System

总结一下这个学期分布式系统的课程项目

<!-- more -->

***
# Project 1

## Task 1
写一个简单的 web application 实现以下功能：
1. 用户输入一个字符串，并可以选择 MD5 或者 SHA-1 作为哈希算法
2. 点击提交，得到哈希结果

MD5：MD5即Message-Digest Algorithm 5
SHA1：Secure Hash Algorithm

采用 MVC 模式：
1. Model 中写完成哈希算法的流程
2. View 用 JSP 完成
    index.jsp 页面来提交字符串
    result.jsp 页面来展示哈希结果
3. Controller 中写 Servlet
    在 doGet 方法中接收字符串，并调用 Model 中的处理逻辑

[Github Link](https://github.com/oscar0715/DS-Project1-HashFunction)

## Task 2
一个简单的投票系统

采用 MVC 模式：
1. Model 记录选项和票数
2. View 用 JSP 完成
    - index.jsp 现实所有选项，并可以投票
    - next.jsp 下一次投票
    - result.jsp 页面来展示投票结果
3. Controller 中写 Servlet
    - doGet 读取投票结果
    - doPost 接收投票动作，并调用model中的投票逻辑

[Github Link](https://github.com/oscar0715/DS-Project1-ClassSurvey)

## Task3
查看各个国家国旗的 Web Application

采用 MVC 模式：
1. Model 处理获取国旗的的逻辑
2. View 用 JSP 完成
    - index.jsp 页面中可以选择所有的国家，用户选择一个国家
    - showFlag.jsp 页面可以显示选中国家的国旗和国旗的介绍
3. Controller 中写 Servlet
    - doGet 接收用户的请求

[Github Link](https://github.com/oscar0715/DS-Project1-WorldFlagColletion)

<br> 
*** 
<br>

# Project 2

这个是项目的内容是加密，使用 socket 进行通信。

## Task1
业务
1. client 向 server 发送自己的位置和消息。
2. client 在 server 有自己用户名和密码。 Sever 根据 client 用户名密码是否正确判断 client 是否合法。
3. 加密的部分，使用 TEA 对称算法。 Client 和 Server 共享一个 symmetric key 。在判断用户名密码之前， Client 首先要提交对称加密的密钥，如果密钥不通过那也就不用判断用户名密码了
4. 如果对称密钥和用户名密码的验证都通过以后，Server 端将 Client 端的位置写入 kml 文件。

代码实现：
1. Server 端
    - Server端代码在 TCPSpyCommanderUsingTEAandPasswords.java
    - Connection.java 类拓展了 Thread 类，是 server 端的处理 client 的密钥和消息的业务逻辑。
    - 新建服务端进程 serverSocket 监听 7777 端口，每次监听到一个客户端进程 socket ，就新开一个 Connection 类来处理一个 client 请求
    - Sever 端代码
    Sever端：
``` java
// 声明客户端 socket
Socket clientSocket = null;

try {
    // the server port we are using
    int serverPort = 7777;

    // Create a new server socket
    ServerSocket listenSocket = new ServerSocket(serverPort);

    /*
     * Block waiting for a new connection request from a client.
     * When the request is received, "accept" it, and the rest
     * the tcp protocol handshake will then take place, making 
     * the socket ready for reading and writing.
     */
    while (true) {
        clientSocket = listenSocket.accept();
        // If we get here, then we are now connected to a client.

        // create a thread fot this user
        Connection c = new Connection(clientSocket, //其他参数
            );
    }
// Handle exceptions
} catch (IOException e) {
    System.out.println("IO Exception:" + e.getMessage());

    // If quitting (typically by you sending quit signal) clean up sockets
} finally {
    try {
        if (clientSocket != null) {
            clientSocket.close();
        }
    } catch (IOException e) {
        // ignore exception on close
    }
}
```

    - Connection 类：
``` java
class Connection extends Thread {

    private Socket clientSocket;

    public Connection(Socket aClientSocket)
        this.clientSocket = aClientSocket;

        this.start();
    }
    
    public void run() {
        加密通信
        1. 对称加密，双方都有相同的对称密钥

    2. 非对称加密，用非对称加密交换对称密钥
    }
}
```

2. Client 端
    - Client 端代码在TCPSpyUsingTEAandPasswords.java 
    - 每次新建一个客户端 socket 进程和 server 端交互。
    - client 端代码
``` java
Socket clientSocket = null;
// arguments supply hostname
int serverPort = 7777;

try {
    clientSocket = new Socket("localhost", serverPort);
    // 做该做的事情
} catch (IOException e) {
    System.out.println("IO Exception:" + e.getMessage());
} catch (Exception e) {
    e.printStackTrace();
    System.out.println("Exception:" + e.getMessage());
} finally {
    try {
        if (clientSocket != null) {
            clientSocket.close();
        }
    } catch (IOException e) {
        // ignore exception on close
    }
}
```

3. 辅助类
    - TEA 类实现加密算法
    - CommonUtil 是一个辅助类，将一些业务逻辑抽象出来
    - PasswordHash 是用来生成 client 的 hash 以后的密码的
    - KMLWriter 是用来将 client 的地址写入 kml file.
    - kml file 的路径是 "./src/SecretAgents.kml"

[Github Link](https://github.com/oscar0715/DS-Project2-symmetric-algorithm)

## Task2

Task1 是用对称加密，但是要求 client 和 server 端在沟通之前都拥有对称密钥。Task2 中，server 端先用非对称加密算法 RSA 将对称密钥发送给 client 端，然后 client 和 server 继续用对称加密算法 TEA 进行加密沟通。这解决了分配对称加密算法公钥的麻烦。

RSA 算法
    1. 生成两个随机质数 `p, q`
    2. 计算 `n = p * q`
    3. 计算 n 的欧拉函数 `phi(n) = (p-1) * (q-1)`
    4. 随机选择一个整数`e`，条件是 `1< e < phi(n)`，且 `e 与 phi(n) 互质`  
    5. 计算`e`对于`phi(n)`的模反元素 `d = e.modInverse(phi)`;
    6. 将n和e封装成公钥，n和d封装成私钥

[Github Link](https://github.com/oscar0715/DS-Project2-asymmetric-algorithm)

<br> 
*** 
<br>

# Project 3

## 补一下背景知识
WebServices三种基本元素：
1. WebServices三种基本元素之 SOAP
    - SOAP 就是简单对象访问协议。是一种用于访问 Web 服务的协议。
    - 因为 SOAP 基于XML 和 HTTP ，其通过XML 来实现消息描述，然后再通过 HTTP 实现消息传输。
2. WebServices三种基本元素之 WSDL
    - WSDL 即Web Services Description Language也就是 Web 服务描述语言。
    - 是基于 XML的用于描述 Web 服务以及如何访问 Web 服务的语言。
    - 服务描述与底层的 SOAP 基础结构相结合，足以封装服务请求者的应用程序和服务提供者的 Web服务之间的这个细节。
    - WSDL 描述了 Web服务的三个基本属性：
        （1）服务所提供的操作
        （2）如何访问服务
        （3）服务位于何处（通过 URL 来确定就 OK 了）
3. WebServices三种基本元素之 UDDI
    - UDDI 即 Universal Description，Discovery and Integration，也就是通用的描述，发现以及整合。
    - UDDI 呢是一种目录服务，企业可以通过 UDDI 来注册和搜索 Web 服务。
    - 简单来说的话，UDDI 就是一个目录，只不过在这个目录中存放的是一些关于 Web 服务的信息而已。并且 UDDI 通过SOAP 
    -  .Net 之上。

## JAX-WS
Java API for XML Web Services (JAX-WS)

JAX-WS 2.0 有两种开发过程：自顶向下和自底向上。自顶向下方式指通过一个 WSDL 文件来创建Web Service，自底向上是从 Java 类出发创建 Web Service。

## Task 1 RPC Style API Design

在 Netbeans 中使用 JAX-WS 来实现 client 端和 server 端的 RPC 通信。在服务端写好方法，然后挂到客户端供客户端调用。

[Github Link](https://github.com/oscar0715/DS-Project3-RPC-API)

在Netbean中, 
1. Server 端， new Web service。输入 Service name 和 package
    - 可以在 Service 的 Design 视图中定义 Service 的参数和返回值
    - 复制 Service 的 WSDL 文件的 url
2. Client 端，new Web service client，
    - 通过 Server 端的 WSDL 的 url 
    - client 端就会出现 web service 的 reference
    - 然后就能在 client 端调用了。

## Task 2 Single Message Style API Designs

在 Netbeans 中使用 JAX-WS 来实现 client 端和 server 端的 single message 通信。

Server 端只有一个方法，传送的是 xml 消息。 Server 端接收到 Client 端消息以后，解析 xml 字符串，根据 xml 的内容来判断 client 想要实现什么业务逻辑。

[Github Link](https://github.com/oscar0715/DS-Project3-single-message-API)

## Task 3  API Design REST Style

通过 Servet 中的 doGet 和 doPost 方法来实现 REST 风格的 API 

[Github Link](https://github.com/oscar0715/DS-Project3-rest-style-API)

<br> 
*** 
<br>

# Project 4 
写 Android 的

# Project 5
Map Reduce，没啥好讲的

# project 6 
对 Snapshot Algorithm 的详解在这里：[SnapShot算法](http://menuet.xyz/distribute-system/cmu-95702-project6/)


***

Over！