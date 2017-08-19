---
title: Django + uWSGI + nginx
tags:
	- Python
	- Django
categories:
	- Techonology
	- Python
date: 2017-01-31 19:20:58
---

要给 django 项目加一个服务器呀

<!-- more -->

***

# WSGI 接口
参考：[来了解一下WSGI这个概念](http://www.nowamagic.net/academy/detail/1330310)

WSGI，全称 Web Server Gateway Interface

WSGI就像是一座桥梁，一边连着web服务器，另一边连着用户的应用。但是呢，这个桥的功能很弱，有时候还需要别的桥来帮忙才能进行处理。WSGI 的作用如图所示：

![WSGI就像是一座桥梁](http://www.nowamagic.net/librarys/images/201309/2013_09_04_01.png "WSGI就像是一座桥梁")

WSGI的作用

WSGI有两方：“服务器”或“网关”一方，以及“应用程序”或“应用框架”一方。服务方调用应用方，提供环境信息，以及一个回调函数（提供给应用程序用来将消息头传递给服务器方），并接收Web内容作为返回值。

所谓的 WSGI中间件同时实现了API的两方，因此可以在WSGI服务和WSGI应用之间起调解作用：从WSGI服务器的角度来说，中间件扮演应用程序，而从应用程序的角度来说，中间件扮演服务器。“中间件”组件可以执行以下功能：
1. 重写环境变量后，根据目标URL，将请求消息路由到不同的应用对象。
2. 允许在一个进程中同时运行多个应用程序或应用框架。
3. 负载均衡和远程处理，通过在网络上转发请求和响应消息。
4. 进行内容后处理，例如应用XSLT样式表。

# 至于安装
已经有很详细的文章了：
[基于nginx和uWSGI在Ubuntu上部署Django
](http://www.jianshu.com/p/e6ff4a28ab5a "基于nginx和uWSGI在Ubuntu上部署Django
")

这个是官方文档：
[Setting up Django and your web server with uWSGI and nginx](http://uwsgi-docs.readthedocs.io/en/latest/tutorials/Django_and_nginx.html "Setting up Django and your web server with uWSGI and nginx")

***
