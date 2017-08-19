---
title: web.xml_in_a_Web_Project
tags:
  - Java
categories:
  - Technology
  - Java
date: 2016-06-14 11:37:43
---
Web项目启动的时候，容器唯一会自动读取web.xml文件。
学习一下web.xml的机制和标签的含义
<!-- more -->

***

## 初始化流程

### web.xml
1. 启动一个WEB项目的时候,容器(如:Tomcat)会去读它的配置文件web.xml.
2. 容器在web.xml中会首先读两个节点: `<listener></listener>` 和 `<context-param></context-param>`

### context-param
>`<context-param>` 用来声明应用范围(整个WEB项目)内的上下文初始化参数。

格式:
``` xml
<context-param>  
	<!-- param-name 设定上下文的参数名称。必须是唯一名称 -->
	<param-name>contextConfigLocation</param-name> 

	<!-- param-value 设定的参数名称的值 -->  
	<param-value>contextConfigLocationValue</param-value>  
</context-param>  
```


读到 `<context-param>`，容器会创建一个`ServletContext`，应用范围内，即整个WEB项目，都能使用这个上下文。

接着，容器将`param-name`和`para-value`转化为键值对，交给`ServletContent`

### Listener
>定义: 监听器`Listener`就是在`application`,`session`,`request`三个对象创建、销毁或者往其中添加修改删除属性时自动执行代码的功能组件。
`Listener`是`Servlet`的监听器，可以监听客户端的请求，服务端的操作等。

在`web.xml`中，`Listener`的配置信息的初始化（`ServletContentListener`初始化）必须比`Filter`和`Servlet`，`Listener`都优先，而销毁比Servlet和Filter都慢。

``` xml
<listener>
	<listener-class>com.listener.class</listener-class>
</listener>
```

- 容器创建 `<listener></listener>` 中的类实例，即创建监听（备注：`listener`定义的类可以是自定义的类但必须需要继承`ServletContextListener`）
- 在监听的类中会有一个`contextInitialized(ServletContextEvent event)`初始化方法。
- 在这个方法中可以通过`event.getServletContext().getInitParameter("contextConfigLocation")` 来得到
`context-param` 设定的值。
在这个类中还必须有一个 `contextDestroyed(ServletContextEvent event)` 销毁方法。用于关闭应用前释放资源，比如说数据库连接的关闭。
得到这个`context-param`的值之后,你就可以做一些操作了。注意，这个时候你的WEB项目还没有完全启动完成。这个动作会比所有的`Servlet`都要早

由上面的初始化过程可知容器对于`web.xml`的加载过程是 `context-param >> listener  >> fileter  >> servlet` 


### context-param 和 init-param区别
`web.xml`里面可以定义两种参数：
1. application 范围内的参数，存放在 `servlet context` 中，在 `web.xml` 中配置如下：
``` xml
<context-param>
	<param-name>context/param</param-name>
	<param-value>avalible during application</param-value>
</context-param>
```
在 `servlet` 里面可以通过 `getServletContext().getInitParameter("context/param")` 得到

2. `servlet` 范围内的参数，只能在 `servlet` 的 `init()`方法中取得，在web.xml中配置如下：

``` xml
<servlet>
	<servlet-name>MainServlet</servlet-name>
	<servlet-class>com.wes.controller.MainServlet</servlet-class>
	<init-param>
	   <param-name>param1</param-name>
	   <param-value>avalible in servlet init()</param-value>
	</init-param>
	<load-on-startup>0</load-on-startup>
</servlet>
```

在servlet中可以通过代码分别取用：
``` java
package com.wes.controller;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
public class MainServlet extends HttpServlet ...{
	public MainServlet() ...{
		super();
	 }
	public void init() throws ServletException ...{
		 System.out.println("下面的两个参数param1是在servlet中存放的");
		 System.out.println(this.getInitParameter("param1"));
		 System.out.println("下面的参数是存放在servletcontext中的");
		System.out.println(getServletContext().getInitParameter("context/param"));
	  }
}
```

###  Spring 中 DispacherServlet， WebApplicationContext， ServletContext 的关系 
再梳理一下 Spring 的启动过程
1. `ServletContext`
首先，对于一个web应用，它部署在 web 容器中， web 容器提供其一个全局的上下文环境，这个上下文就是 `ServletContext` ，其为后面的 spring IoC 容器提供宿主环境；
2. `WebApplicationContext`
其次，在web.xml中会提供有 `contextLoaderListener` 。在 web 容器启动时，会触发容器初始化事件，此时 `contextLoaderListener`会监听到这个事件，其 `contextInitialized` 方法会被调用.
在这个方法中，Spring 会初始化一个启动上下文，这个上下文被称为根上下文，即`WebApplicationContext`.
这是一个接口类，确切的说，其实际的实现类是`XmlWebApplicationContext`。这个就是 Spring 的 IoC 容器，其对应的 Bean 定义的配置由 web.xml 中的 `<context-param>` 标签指定.
在这个IoC容器初始化完毕后，spring以 `WebApplicationContext.ROOTWEBAPPLICATIONCONTEXTATTRIBUTE` 为属性Key，将其存储到 `ServletContext` 中，便于获取；
3. `DispatcherServlet`
再次，`contextLoaderListener`监听器初始化完毕后，开始初始化web.xml中配置的Servlet.
以最常见的`DispatcherServlet`为例，这个 servlet 实际上是一个标准的前端控制器，用以转发、匹配、处理每个 servlet 请求。
`DispatcherServlet`上下文在初始化的时候会建立自己的IoC上下文，用以持有 spring mvc 相关的 bean 。
在建立 `DispatcherServlet` 自己的IoC上下文时，会利用`WebApplicationContext.ROOTWEBAPPLICATIONCONTEXTATTRIBUTE`先从 `ServletContext` 中获取之前的根上下文(即 `WebApplicationContext` )作为自己上下文的 parent 上下文。
有了这个 parent 上下文之后，再初始化自己持有的上下文。
初始化完毕后， spring 以与 servlet 的名字相关(此处不是简单的以 servlet 名为 Key ，而是通过一些转换，具体可自行查看源码)的属性为属性 Key ，也将其存到 `ServletContext` 中，以便后续使用。这样每个 servlet 就持有自己的上下文，即拥有自己独立的 bean 空间，同时各个 servlet 共享相同的 bean ，即根上下文(第2步中初始化的上下文)定义的那些bean。


## 实例
### Spring使用ContextLoaderListener加载ApplicationContext配置信息
``` xml
<context-param>
	<param-name>contextConfigLocation</param-name>
	<param-value>classpath:spring/applicationContext-*.xml</param-value>
	<!-- 采用的是通配符方式，查找WEB-INF/spring目录下xml文件。如有多个xml文件，以“,”分隔。 -->
</context-param>
<listener>
	<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```
`contextConfigLocation` 参数定义了要装入的 Spring 配置文件。
spring 会使用 `contextConfigLocation` 参数加载所有逗号分割的xml。
如果没有这个参数，spring 默认加载 `web-inf/applicationContext.xml` 文件.

Spring 提供 `ServletContextListener` 的一个实现类 `ContextLoaderListener` 。
该类可以作为 listener 使用，它会在创建时自动查找 `WEB-INF/` 下的 `applicationContext.xml` 文件。
因此，如果只有一个配置文件，并且文件名为 `applicationContext.xml` ，则只需在web.xml文件中增加如下代码即可:
``` xml
<listener>
	<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```

***


### 配置一个spring为加载而设置的servlet可以达到同样效果.
Spring 提供了一个特殊的 Servllet 类: `ContextLoaderServlet。`
该 Servlet 在启动时，会自动查找WEB-IN日下的 `applicationContext.xml` 文件。

当然，为了让 `ContextLoaderServlet` 随应用启动而启动，应将此 Servlet 配置成 `load-on-startup` . Servlet `load-on-startup` 的值小一点比较合适，因为要保证 ApplicationContext 优先创建。

如果只有一个配置文件，并且文件名为 `applicationContext.xml` ，则在 web.xml 文件中增加如下代码即可:
``` xml
<servlet>
	<servlet-name>context</servlet-name>
	<servlet-class>org.springframework.web.context.ContextLoaderServlet</servlet-class>
	<load-on-startup>1</load-on-startup>
</servlet>
```

带多个配置文件的web.xml 文件如下:
``` xml
<web-app>
	<!--确定多个配置文件-->
	<context-param>
		<!-- 参数名为contextConfigLocation-->
		<param-name>contextConfigLocation</param-name>
		<!-- 多个配置文件之间以，隔开 -->
		<param-value>/WEB-工NF/daoContext.xml,/WEB-工NF/applicationContext.
		xml</param-value>
	</context-param>

	<!--采用load-on-startup Servlet 创建Applicat工onContext 实例-->
	<servlet>
		<servlet-name>context</servlet-name>
		<servlet-class>org.springframework.web.context.ContextLoader
		Servlet</servlet-class>

		<!-- 表示启动容器时初始化该Servlet；值小一点比较合适，会优先加载 -->
		<load-on-startup>1</load-on-startup>
	</servlet>
</web-app>
```

### 实例解释各个上下文关系

``` xml
<context-param>  
	<param-name>contextConfigLocation</param-name>  
	<param-value>/WEB-INF/applicationContext.xml</param-value>  
</context-param>  

<listener>  
	<listener-class>  
		org.springframework.web.context.ContextLoaderListener  
	</listener-class>  
</listener>  

<servlet>  
	<servlet-name>mvc-dispatcher</servlet-name>  
	<servlet-class>  
		org.springframework.web.servlet.DispatcherServlet  
	</servlet-class>  
	<load-on-startup>1</load-on-startup>  
</servlet> 

<servlet-mapping>  
	<servlet-name>mvc-dispatcher</servlet-name>  
	<url-pattern>/</url-pattern>  
</servlet-mapping>
```



上文已经说过， 以上配置首先会在 `ContextLoaderListener` 中通过 `<context-param>` 中的 `applicationContext.xml` 创建一个 `ApplicationContext` ，再将这个 applicationContext 塞到 `ServletContext` 里面，通过 `ServletContext` 的 `setAttribute` 方法达到此目的，在 `ContextLoaderListener` 的源代码中，我们可以看到这样的代码：
``` java
servletContext.setAttribute(WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE, this.context);
```

以上由 ContextLoaderListener 创建的 `ApplicationContext` 是共享于整个Web应用程序的。
而你可能早已经知道， `DispatcherServlet` 会维持一个自己的 `ApplicationContext` ，默认会读取 `/WEB-INFO/<dispatcherServletName>-servlet.xml` 文件，而我们也可以重新配置：
``` xml
<servlet-name>  
	customConfiguredDispacherServlet  
</servlet-name>  
<servlet-class>  
	org.springframework.web.servlet.DispatcherServlet  
</servlet-class>  
<init-param>  
	<param-name>  
		contextConfigLocation  
	</param-name>  
	<param-value>  
		/WEB-INF/dispacherServletContext.xml  
	</param-value>  
</init-param>  
<load-on-startup>1</load-on-startup>  
```

1. `ContextLoaderListener`中创建`ApplicationContext`主要用于整个Web应用程序需要共享的一些组件，比如DAO，数据库的ConnectionFactory等。
2. 而由`DispatcherServlet`创建的`ApplicationContext`主要用于和该Servlet相关的一些组件，比如Controller、ViewResovler等。对于作用范围而言，在`DispatcherServlet`中可以引用由`ContextLoaderListener`所创建的`ApplicationContext`，而反过来不行。

在Spring的具体实现上，这两个`ApplicationContext`都是通过`ServletContext`的`setAttribute`方法放到`ServletContext`中的。
- 但是，`ContextLoaderListener`会先于`DispatcherServlet`创建`ApplicationContext`
- `DispatcherServlet`在创建`ApplicationContext`时会先找到由`ContextLoaderListener`所创建的`ApplicationContext`，再将后者的`ApplicationContext`作为参数传给`DispatcherServlet`的`ApplicationContext`的`setParent()`方法
- 在Spring源代码中，你可以在FrameServlet.java中找到如下代码：
`wac.setParent(parent);`
其中，wac即为由`DisptcherServle`t创建的`ApplicationContext`，而parent则为有`ContextLoaderListener`创建的`ApplicationContext`。
- 此后，框架又会调用`ServletContext`的`setAttribute()`方法将wac加入到`ServletContext`中。

***

## 参考
1. {% link Java中的Listener 监听器 http://tianweili.github.io/blog/2015/01/27/java-listener/ %}
2. {% link Web.xml配置文件之contextConfigLocation(Spring)监听器 http://my.oschina.net/u/874225/blog/177978 %}
3. {% link  Web.xml配置详解之context-param http://blog.csdn.net/liaoxiaohua1981/article/details/6759206 %}
4. {% link context-param与init-param的区别与作用 http://www.cnblogs.com/hzj-/articles/1689836.html %}
5. {% link Spring中DispacherServlet、WebApplicationContext、ServletContext的关系 http://blog.csdn.net/c289054531/article/details/9196149 %}

