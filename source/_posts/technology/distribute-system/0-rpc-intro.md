---
title: 学习一下dubbo
tags:
  - Dubbo
categories:
  - Technology
  - Architeture
date: 2016-06-12 14:45:15
---
dubbo
参考: {% link Dubbo用户手册 http://dubbo.io/User+Guide-zh.htm#UserGuide-zh-%E6%B3%A8%E8%A7%A3%E9%85%8D%E7%BD%AE %}
<!-- more -->

***

#### 背景 
网站应用规模扩大 --> 框架的发展 --> 分布式服务架构；流动计算架构 --> 治理系统

<br/>
![dubbo-architecture-roadmap](http://ogy9sznv2.bkt.clouddn.com/0-alibaba-dubbo/dubbo-architecture-roadmap.jpg "dubbo-architecture-roadmap")
<br/>

+ 单一应用框架
    * 网站流量小，所有功能都部署在一个应用中，节点少，成本低
    * 此时，数据访问框架（ORM）是关键-->简化增删改查工作量（我猜测是因为没别的地方可以优化了呀）
    * ORM: Object Relational Mapping
+ 垂直应用架构
    * 访问量增大，将应用拆分互不相干的几个模块，每个模块都是独立自主的系统
    * 此时，Web框架（MVC）是关键-->加速前段页面开发
+ 分布式服务架构
    * 垂直应用数量增加，应用交互不可避免-->抽取核心业务，形成稳定的服务中心
    * 此时分布式服务框架RPC是关键-->提高业务复用以及整合
    * RPC: Remote Procedure Call Protocol
+ 流动计算架构
    * 增加一个调度中心-->基于访问压力实时管理集群容量，提高集群利用率
    * 此时资源调度和治理中（SOA）是关键-->提高机器利用率
    * SOA: Service-Oriented Architecture

#### 需求
dubbo 服务治理
<br/>
![dubbo 服务治理](http://ogy9sznv2.bkt.clouddn.com/0-alibaba-dubbo/dubbo-service-governance.jpg "dubbo 服务治理")

0. 在大规模服务化之前，应用可能只是通过RMI或Hessian等工具，简单的暴露和引用远程服务，通过**配置服务的URL地址**进行调用，通过F5等硬件进行负载均衡。

1. 当服务越来越多时，服务URL配置管理变得非常困难，F5硬件负载均衡器的单点压力也越来越大。
此时需要一个**服务注册中心**，动态的注册和发现服务，使服务的位置透明。
并通过在消费方获取服务提供方地址列表，实现软负载均衡和Failover，降低对F5硬件负载均衡器的依赖，也能减少部分成本。

2. 当进一步发展，服务间依赖关系变得错踪复杂，甚至分不清哪个应用要在哪个应用之前启动，架构师都不能完整的描述应用的架构关系。
这时，需要自动画出应用间的依赖关系图，以帮助架构师理清理关系。

3. 接着，服务的调用量越来越大，服务的容量问题就暴露出来，这个服务需要多少机器支撑？什么时候该加机器？
为了解决这些问题，第一步，要将服务现在每天的调用量，响应时间，都统计出来，作为**容量规划**的参考指标。
其次，要可以**动态调整权重**，在线上，将某台机器的权重一直加大，并在加大的过程中记录响应时间的变化，直到**响应时间到达阀值**，记录此时的访问量，再以此访问量乘以机器数反推总容量。

#### 一个demo

##### 服务提供者
###### 定义服务接口
{% codeblock DemoService.java lang:Java  %}
// 定义服务接口: (该接口需单独打包，在服务提供方和消费方共享)

package com.alibaba.dubbo.demo;
 
public interface DemoService {
 
    String sayHello(String name);
 
}
{% endcodeblock %}

###### 在服务提供方实现接口
{% codeblock DemoServiceImpl.java lang:Java  %}
// 在服务提供方实现接口：(对服务消费方隐藏实现)

package com.alibaba.dubbo.demo.provider;
 
import com.alibaba.dubbo.demo.DemoService;
 
public class DemoServiceImpl implements DemoService {
 
    public String sayHello(String name) {
        return "Hello " + name;
    }
 
}
{% endcodeblock %}

###### 用Spring配置声明暴露服务

{% codeblock provider.xml lang:xml  %}

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
    xsi:schemaLocation="http://www.springframework.org/schema/beans        http://www.springframework.org/schema/beans/spring-beans.xsd        http://code.alibabatech.com/schema/dubbo        http://code.alibabatech.com/schema/dubbo/dubbo.xsd">
 
    <!-- 提供方应用信息，用于计算依赖关系 -->
    <dubbo:application name="hello-world-app"  />
 
    <!-- 使用multicast广播注册中心暴露服务地址 -->
    <dubbo:registry address="multicast://224.5.6.7:1234" />
 
    <!-- 用dubbo协议在20880端口暴露服务 -->
    <dubbo:protocol name="dubbo" port="20880" />
 
    <!-- 声明需要暴露的服务接口 -->
    <dubbo:service interface="com.alibaba.dubbo.demo.DemoService" ref="demoService" />
 
    <!-- 和本地bean一样实现服务 -->
    <bean id="demoService" class="com.alibaba.dubbo.demo.provider.DemoServiceImpl" />
 
</beans>
{% endcodeblock %}

###### 提供者加载Spring配置
{% codeblock provider.java lang:Java  %}

import org.springframework.context.support.ClassPathXmlApplicationContext;
 
public class Provider {
 
    public static void main(String[] args) throws Exception {
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(new String[] {"http://10.20.160.198/wiki/display/dubbo/provider.xml"});
        context.start();
 
        System.in.read(); // 按任意键退出
    }
 
}
{% endcodeblock %}

##### 服务消费者

###### 用Spring配置引用远程服务

{% codeblock consumer.xml lang:xml  %}
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
    xsi:schemaLocation="http://www.springframework.org/schema/beans        http://www.springframework.org/schema/beans/spring-beans.xsd        http://code.alibabatech.com/schema/dubbo        http://code.alibabatech.com/schema/dubbo/dubbo.xsd">
 
    <!-- 消费方应用名，用于计算依赖关系，不是匹配条件，不要与提供方一样 -->
    <dubbo:application name="consumer-of-helloworld-app"  />
 
    <!-- 使用multicast广播注册中心暴露发现服务地址 -->
    <dubbo:registry address="multicast://224.5.6.7:1234" />
 
    <!-- 生成远程服务代理，可以和本地bean一样使用demoService -->
    <dubbo:reference id="demoService" interface="com.alibaba.dubbo.demo.DemoService" />
 
</beans>
{% endcodeblock %}

###### 提供者加载Spring配置
{% codeblock consumer.java lang:Java  %}

import org.springframework.context.support.ClassPathXmlApplicationContext;
import com.alibaba.dubbo.demo.DemoService;
 
public class Consumer {
 
    public static void main(String[] args) throws Exception {
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(new String[] {"http://10.20.160.198/wiki/display/dubbo/consumer.xml"});
        context.start();
 
        DemoService demoService = (DemoService)context.getBean("demoService"); // 获取远程服务代理
        String hello = demoService.sayHello("world"); // 执行远程方法
 
        System.out.println( hello ); // 显示调用结果
    }
 
}
{% endcodeblock %}

***
Over!

#### 参考
{% link Dubbo用户手册 http://dubbo.io/User+Guide-zh.htm#UserGuide-zh-%E6%B3%A8%E8%A7%A3%E9%85%8D%E7%BD%AE %}

