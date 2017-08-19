---
title: 感受一下REST架构
tags:
  - Rest
categories:
  - Technology
  - Architecture
  - 
date: 2016-06-25 10:41:52
---
REST: URL定位资源，用HTTP动词（GET,POST,DELETE,DETC）描述操作。
<!-- more -->

***
### 引用放在最前面
1. {% link REST架构该怎么生动地理解-高票答案 
https://www.zhihu.com/question/27785028/answer/48096396 REST 架构该怎么生动地理解 %}
2. {% link 理解RESTful架构 http://www.ruanyifeng.com/blog/2011/09/restful.html 理解RESTful架构 %}
3. {% link 理解本真的REST架构风格 http://www.infoq.com/cn/articles/understanding-restful-style 理解本真的REST架构风格 %}
4. {% link RPC与REST区别 http://thinking80s.iteye.com/blog/713450 RPC与REST区别 %}
5. {% link Web Service实践之REST vs RPC http://www.cnblogs.com/mindsbook/archive/2009/11/17/web_service_RESTvsRPC.html Web Service实践之REST vs RPC %}

6. {% link 如何获取（GET）一杯咖啡——星巴克REST案例分析 http://www.infoq.com/cn/articles/webber-rest-workflow/ 如何获取（GET）一杯咖啡——星巴克REST案例分析 %}
7. {% link RESTful Web Services: A Tutorial http://www.drdobbs.com/web-development/restful-web-services-a-tutorial/240169069 RESTful Web Services: A Tutorial %}
8. {% link Building a RESTful Web Service https://spring.io/guides/gs/rest-service/REST Building a RESTful Web Service %}
9. {% link ReST Vs SOA(P) vs RPC http://www.slideshare.net/ozten/rest-vs-soap-yawn ReST Vs SOA(P) %}
10. {% link Restful User Experience http://www.slideshare.net/trilancer/restful-user-experience-1421793 Restful User Experience %}

***

### 写在前面

成长路上，跟对人真的很重要啊，遇到个nice的主管简直人生都光明了。

### 要了解一下发展历程
RPC --> http service --> Restful

#### web service
简单地说, 也就是服务器如何向客户端提供服务.

常用的方法有:

1. RPC 所谓的远程过程调用 (面向方法)
2. SOA 所谓的面向服务的架构(面向消息)
3. REST 所谓的 Representational state transfer (面向资源)

如果说 RPC 是基于方法调用(method),那么 SOA 则是基于 消息, 基于方法调用通常会与特定的程序语言 耦合起来,而后者则与具体的实现语言无关, 所以在一定程度上得到大公司的支持.

#### RPC（Remote Procedure Call）远程调用
这个例子不错,原文在这里哈（{% link RPC与REST区别 http://thinking80s.iteye.com/blog/713450 RPC与REST区别 %}）:

打个比方，在网上购物的购物篮功能中，将选购的物品放入购物篮的操作就会使用到RPC，在客户端所表现的只是需要点击一个按钮，按钮的功能是将选定的物品放入购物篮中。这是在前台，用户可以确实的看到的操作；而在后台，在编辑这个网页的过程中，用户点击按钮的这一步，是由远程调用服务器端的相应的函数实现的。在此例中，想实现这个按钮的功能就要知道调用服务器端的添加物品的函数（也叫接口interface）的名称－－AddinBasket（）；client端发送请求给服务器，要将选定的物品放入购物篮，服务器端接到请求后，由AddinBasket函数对请求进行响应，做出处理，然后把响应结果（如，物品已放入购物篮）返回给client端。Client端接到回复后显示给用户：操作成功，物品已加入购物篮。

因为需要调用服务器端的接口函数就需要了解服务器端究竟提供了什么样的接口，接口的实现方法是什么，函数的参数是什么类型的，这些信息都会写在服务器端的函数文档中，如上面的购物篮功能涉及的函数不光有：AddinBasket()还应该包括，RemovefromBasket(),ClearBasket(),getBasketItem(),purchasBasket()等等这些，这些也就是相对于客户的“从购物篮中删除物品”，“清空购物篮”，“获得购物篮中的物品列表”，“为购物篮内物品付款”的这些具体的操作的响应函数，编程人员在编写页面的代码时需要透彻的理解各个函数的调用方法，以及相互之间的逻辑关系。这里的逻辑关系是指如例中，当购物篮内是空的时，从购物篮中删除物品的按钮应该是不允许操作状态的。这一系列的函数的理解都给编程增添了复杂度，而且服务器端在正式运行中要处理所有的用户请求，而这些请求的功能是很烦琐的，这给服务器端无形中创造了很多的工作量，而REST在这一点上是很精简有效的。  

#### Rest
必须承认的是大部分的REST的实现中使用了RPC的机制，它也有client端和server端，所不同于RPC的是，它的响应函数简单来讲就是get函数和post函数，对于上面使用的购物篮问题中使用REST方法实现的化，只需要两个函数getBasket和PostBasket，getBasket 函数是将服务器端当前的购物篮状态获取下来，client端想对购物篮中的物品做出操作的话，比如添加一本书，或将已经有的物品取出购物篮，直接修改修改购物篮中的物品，最后将一个新的购物篮内物品的状态（status）用postBasket方法发送给服务器。

这就是REST的中心原理，即：下载服务器端的当前状态，修改之后将最终用户所期待的状态发送给服务器，服务器按照客户的期待进行修改。

而不同于RPC的也就是响应函数没有那么多的，复杂的逻辑关系，函数也减少了很多，只是get和post两个。

总结一下，RPC逻辑复杂，对服务器造成很多的工作量，但分工明确，不容易造成失误。REST逻辑简单，对服务器的工作压力也比较小，但在某些特殊情况下不一定完美的解决问题。 

#### 再说一些区别
原文在这里: {% link Web Service实践之REST vs RPC http://www.cnblogs.com/mindsbook/archive/2009/11/17/web_service_RESTvsRPC.htmlWeb Service实践之REST vs RPC %}
如果你想只记住一点,那么就请记住 `RPC是以动词为中心`的, `REST是以名词为中心`的, 此处的 动词指的是一些`方法`, 名词是指`资源`.

你会发现,以动词为中心,意味着,当你要需要加入新功能时,你必须要添加更多的动词, 这时候服务器端需要实现 相应的动词(方法), 客户端需要知道这个新的动词并进行调用.

而以名词为中心, 假使我请求的是 hostname/friends/, 无论这个URI对应的服务怎么变化,客户端是无需 关注和更新的,而这种变化对客户端也是透明的.


### REST架构该怎么生动地理解
#### 简洁地说
0. REST: URL定位资源，用HTTP动词（GET,POST,DELETE,DETC）描述操作。
1. REST描述的是在网络中client和server的一种交互形式；REST本身不实用，实用的是如何设计 RESTful API（REST风格的网络接口）；
2. Server提供的RESTful API中，URL中只使用名词来指定资源，原则上不使用动词。“资源”是REST架构或者说整个网络处理的核心。
3. 用HTTP协议里的动词来实现资源的添加，修改，删除等操作。即通过HTTP动词来实现资源的状态扭转。
    >GET 用来获取资源，
    POST 用来新建资源（也可以用于更新资源），
    PUT 用来更新资源，
    DELETE 用来删除资源。

    ![REST API Design](http://ogy9sznv2.bkt.clouddn.com/rest/rest1.jpg "REST API Design")
    <br/>
4. Server和Client之间传递某资源的一个表现形式，比如用JSON，XML传输文本，或者用JPG，WebP传输图片等。当然还可以压缩HTTP传输时的数据（on-wire data compression）。
5. 用 HTTP Status Code传递Server的状态信息。比如最常用的 200 表示成功，500 表示Server内部错误等。

#### REST名称
REST：REpresentational State Transfer = 直接翻译：表现层状态转移。这个中文直译经常出现在很多博客中。尼玛谁听得懂“表现层状态转移”？这是人话吗？

首先，之所以晦涩是因为前面主语被去掉了，全称是 Resource Representational State Transfer：通俗来讲就是：资源在网络中以某种表现形式进行状态转移。

分解开来：
>Resource：资源，即数据（前面说过网络的核心）。比如 newsfeed，friends等；
Representational：某种表现形式，比如用JSON，XML，JPEG等；
State Transfer：状态变化。通过HTTP动词实现。

#### 为什么要用RESTful结构呢？
近年来移动互联网的发展，各种类型的Client层出不穷，RESTful可以通过一套统一的接口为 Web，iOS和Android提供服务。另外对于广大平台来说，比如Facebook platform，微博开放平台，微信公共平台等，它们不需要有显式的前端，只需要一套提供服务的接口，于是RESTful更是它们最好的选择。在RESTful架构下：

![REST API Design](http://ogy9sznv2.bkt.clouddn.com/rest/rest2.jpg "REST API Design")
<br/>

### 理解RESTful架构-阮一峰老师

Fielding将他对互联网软件的架构原则，定名为REST，即Representational State Transfer的缩写。

如果一个架构符合REST原则，就称它为RESTful架构。

要理解RESTful架构，最好的方法就是去理解Representational State Transfer这个词组到底是什么意思，它的每一个词代表了什么涵义。如果你把这个名称搞懂了，也就不难体会REST是一种什么样的设计。

#### 资源（Resources）
REST的名称"表现层状态转化"中，省略了主语。"表现层"其实指的是"资源"（Resources）的"表现层"。

所谓"资源"，就是网络上的一个实体，或者说是网络上的一个具体信息。它可以是一段文本、一张图片、一首歌曲、一种服务，总之就是一个具体的实在。你可以用一个URI（统一资源定位符）指向它，每种资源对应一个特定的URI。要获取这个资源，访问它的URI就可以，因此URI就成了每一个资源的地址或独一无二的识别符。

所谓"上网"，就是与互联网上一系列的"资源"互动，调用它的URI。

#### 表现层（Representation）
"资源"是一种信息实体，它可以有多种外在表现形式。我们把"资源"具体呈现出来的形式，叫做它的"表现层"（Representation）。

比如，文本可以用txt格式表现，也可以用HTML格式、XML格式、JSON格式表现，甚至可以采用二进制格式；图片可以用JPG格式表现，也可以用PNG格式表现。

URI只代表资源的实体，不代表它的形式。严格地说，有些网址最后的".html"后缀名是不必要的，因为这个后缀名表示格式，属于"表现层"范畴，而URI应该只代表"资源"的位置。它的具体表现形式，应该在HTTP请求的头信息中用Accept和Content-Type字段指定，这两个字段才是对"表现层"的描述。

#### 状态转化（State Transfer）
访问一个网站，就代表了客户端和服务器的一个互动过程。在这个过程中，势必涉及到数据和状态的变化。

互联网通信协议HTTP协议，是一个无状态协议。这意味着，所有的状态都保存在服务器端。因此，如果客户端想要操作服务器，必须通过某种手段，让服务器端发生"状态转化"（State Transfer）。而这种转化是建立在表现层之上的，所以就是"表现层状态转化"。

客户端用到的手段，只能是HTTP协议。具体来说，就是HTTP协议里面，四个表示操作方式的动词：GET、POST、PUT、DELETE。它们分别对应四种基本操作：GET用来获取资源，POST用来新建资源（也可以用于更新资源），PUT用来更新资源，DELETE用来删除资源。

#### 综合上面的解释，我们总结一下什么是RESTful架构：
1. 每一个URI代表一种资源；
2. 客户端和服务器之间，传递这种资源的某种表现层；
3. 客户端通过四个HTTP动词，对服务器端资源进行操作，实现"表现层状态转化"
