---
title: CMU 95702 - Service Design
tags:
  - Distribution-System
categories:
  - Technology
  - Distribution-System
date: 2016-10-11 10:21:45
---
CMU 95702 - Distributed System

Service Design
<!-- more -->

***
# Introduction

**System Building Goal:**
1. Reduce Coupling
2. Separation of Concerns
    - Distinct Sections
    - Modularity
    - Encapsulation and Information Hiding
    - Tiers: MVC
    - Layers: HTTP / TCP /IP

# Web Service API Styles
## RPC API

Provider:
A service makes available a Service Descriptor.
Proxy:
The Service Descriptor is used to generate a
Service Connector (proxy) on the client.

Client ---(Procedure Name and Arguments)---> Service

![RPC_API](http://www.servicedesignpatterns.com/Images/RPC_API.jpg)

**Considerations:**
1. procedures may contain a list of typed parameters.
    - tightly coupled (arguments changes, client breaks) 高耦合
    - Descriptor changes --> clients regenerate the connector  
    - Single Message Argument --> less tightly coupled system
2. Default: Request/Response
    - can change to Request/Acknowledge.
    - Request/Acknowledge --> less tightly coupled
    - Request can be queued --> improve scalability
    - Also: Request/ Acknowledge/Poll or Request/Acknowledge/Callback
    - Clients may use an Asynchronous Response Handler if they don’t want to block while waiKng. 

## Message API

- Define messages that are not derived from signatures of remote procedures.
- When the message is received, the server examines its contents to determine the correct procedure to execute.
- The web service serves as a dispatcher and is usually built amer the message is designed.
- No procedure name or parameter list is in the message.
- The service descriptor is encoded by WSDL and XSDL.
- The services descriptor is used to generate the service connector (proxy).

![MessageAPI](http://www.servicedesignpatterns.com/Images/MessageAPI.jpg)  


## Resource API
1. Assign a *URI* to all procedures, instances of domain data and files. 
2. Leverage *HTTP* as a complete application protocol to define standard
service behaviors.

![MessageAPI](http://www.servicedesignpatterns.com/Images/ResourceAPI.jpg)  

**Resource API’s may be RESTful**
1. RepresentaKonal State Transfer (REST)  
<a href="/distribute-system/0-rest/">REST Introduction</a>
2. REST is an architectural style – guidelines, best pracKces.

## RESTful

### REST Architectural Principles
1. The web has *addressable resources*. 
    - Each resource has a URI.
1. The web has a *uniform and constrained interface*.
    - HTTP, for example, has a small number of methods. 
    - Use these to manipulate *resources*.
1. The web is *representaKon oriented*
    - Providing diverse formats.
1. The web may be used to communicate *statelessly*
    - Providing scalability
1. *Hypermedia* is used as the *engine of applicaKon state*.

### Understanding REST
1. REST is not protocol specific.
2. SOAP and WS-* use HTTP strictly as a transport protocol.
3. HTTP may be used as a rich applicaKon protocol.
4. Browsers usually use only a small part of HTTP (GET and POST).
5. HTTP is a synchronous request/response network protocol used for distributed, collaboraKve, document based systems.
6. Various message formats may be used – XML, JSON,..
7. Binary data may be included in the message body.

### Uniform Interface

Notes
- not save : modify resources
- Idempotent: be called many times without different outcomes.

**Operations:**
1. HTTP GET
    + read only operaKon
    + idempotent (once same as many)
    + safe (no important change to server’s
    state)
    + may include parameters in the URI
        http://www.example.com/products/123
2. HTTP PUT
    + store the message body 
    + update (更新值，可以做赋值操作)
    + idempotent 
    + not safe 
3. HTTP POST
    + Not idempotent （可以做加1这种操作）
        (when you don't know the id, may create several copies)
    + Not safe 
    + Each method call may modify the resource in a unique way
    + The request/response may or may not contain addttional information
    + The parameters are found within the request body (not within the URI)
4. HTTP DELETE
    + remove the resource
    + idempotent
    + not safe
    + The request/response may or may not contain addiKonal informaKon


**What does a uniform interface buy?**
1. Familiarity
We do not need a general IDL(interface description language) that describes a variety of method signatures.
We already know the methods and their semantics.
2. Interoperability
WS-* has been a moving target
HTTP is widely supported
3. Scalability
Since GET is idempotent and safe, results may be cached by clients or proxy servers.
Since PUT and DELETE are both idempotent, neither the client or the server need worry about handling duplicate message delivery

### REST Principle
#### Addressability
Each HTTP request uses a URI.

The format of a URI is well defined:
scheme://host:port/path?queryString#fragment

1. The **scheme** need not be HTTP. May be FTP or HTTPS.
2. The **host** field may be a DNS name or a IP address.
3. The **port** may be derived from the scheme. Using HTTP implies port 80. 
4. The **path** is a set of text segments delimited by the “/”.
5. The **queryString** is a list of parameters represented as name=value pairs. 
Each pair is delimited by an “&”.
6. The **fragment** is used to point to a parKcular place in a document.

#### Representation Oriented
**Representations**
- Representations of resources are exchanged.
- GET returns a representation.
- PUT and POST passes representations to the server so that underlying resources may change.
- RepresentaKons may be in many formats: XML, JSON, YAML, etc., ...

**Content-Type:**
- HTTP uses the CONTENT-TYPE header to specify the message format the client or server is sending.
- The value of the CONTENT-TYPE is a MIME typed string. Versioning informaKon may be included.
- Examples: 
text/plain
text/html
applicaKon/vnd+xml;version=1.1
- “vnd” implies a vendor specific MIME type

**ACCEPT：**
- The ACCEPT header is used in content negotiation. This is a wish for a format.
- An AJAX request might include a request for JSON.
- A Java request might include a request for XML.
- Ruby might ask for YAML

#### Communicate Statelessly
- The application may have state but there is no client session data stored on the server.
- If there is any session-specific data it should be held and maintained by the client and transferred to the server with each request as needed.
- The server is easier to scale. No replication of session data concerns.

#### HATEOAS
Hypermedia As The Engine Of ApplicaKon State

HATEOS is a Linked Services Pa;ern

#### Linked Services Pattern
Only publish the addresses of a few root web services. Include the addresses of related services in each response. Let clients parse responses to discover subsequent service URIs.


# Client-Server InteracKon Styles
………………


***

Over！