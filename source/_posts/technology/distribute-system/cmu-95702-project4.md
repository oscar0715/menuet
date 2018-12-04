---
title: CMU 95702 - Project 4
tags:
  - Distribution-System
categories:
  - Technology
  - Distribution-System
date: 2016-11-8 18:49:45
---
CMU 95702 - Distributed System

Project 4
感觉几个点还挺有用的，记录一下

<!-- more -->

***
# 引用Quote的API
http://forismatic.com/en/api/

还有一个API的集合的网站
https://github.com/toddmotto/public-apis

# Java 中Json的解析
这个链接写的很清楚了，不造轮子了：
Reference:
https://www.mkyong.com/java/json-simple-example-read-and-write-json/

# Java Http
用 Apache HttpClient来发送HTTP Get/Post 的请求：
https://www.mkyong.com/java/apache-httpclient-examples/
Apache写的很好了，也不要自己用什么Socket，Httpservlet造轮子

# 把的Task1服务布置到heroku 
heroku.com

1. 注册好账号
2. 安装 Heroku toolbelt （command-line tools. Mac用brew装就行啦
3. Install the heroku-deploy command line plugin by running:
heroku plugins:install https://github.com/heroku/heroku-deploy
4. Create a new Heroku application by running:
  `heroku create` 然后得到一个名字 比如 fathomless-beach-44891
5. 找到你的要布置的网络服务的war包。
6. `heroku deploy:war --war Project4Task1-1.0-SNAPSHOT.war --app fathomless-beach-44891`
7. 访问https://fathomless-beach-44891.herokuapp.com/quote/?key=1



***

Over！