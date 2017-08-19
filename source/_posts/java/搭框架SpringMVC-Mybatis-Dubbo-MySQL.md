---
title: 搭框架SpringMVC + Mybatis + MySQL + Dubbo + Maven
tags:
  - Dubbo
  - Mybatis
  - Spring
  - Maven
categories:
  - Technology
  - Java
date: 2016-06-13 17:10:00
---
终于搭起来了一个可以跑的SpringMVC+Mybatis+MySQL+Dubbo框架
回顾一下:
Mybatis: 一个Java持久化框架（persistence framework），它通过XML描述符或注解把对象与存储过程或SQL语句关联起来。
Dubbo: 一个分布式服务框架，致力于提供高性能和透明化的RPC远程服务调用方案，是阿里巴巴SOA服务化治理方案的核心框架

Mybatis的部分参考的原文链接：
{% link Spring+Mybatis+Maven+MySql搭建实例 http://blog.csdn.net/evankaka/article/details/48784641 %}

<!-- more -->

***

### 说明
一个简单的Demo，只是能够让SpringMVC+Mybatis+MySQL+Dubbo跑起来
{% link 两个项目的源码 https://github.com/oscar0715/SpringMVC-Mybatis-Dubbo-Mysql-Maven 源码下载 %}

### 环境
要用Dubbo先要安装zookeeper
Mac上`brew install zookeeper`不能更方便。

还有就是eclipse Mysql这些就不说了

***

### 数据库
创建一个数据库，我的数据库名为`Users`
然后是建表

{% codeblock create table lang:sql %}
CREATE TABLE `t_user` (  
  `USER_ID` int(11) NOT NULL AUTO_INCREMENT,  
  `USER_NAME` char(30) NOT NULL,  
  `USER_PASSWORD` char(10) NOT NULL,  
  `USER_EMAIL` char(30) NOT NULL,  
  PRIMARY KEY (`USER_ID`),  
  KEY `IDX_NAME` (`USER_NAME`)  
) ENGINE=InnoDB AUTO_INCREMENT=11 DEFAULT CHARSET=utf8  
{% endcodeblock %}

然后插入一条数据
{% codeblock insert lang:sql%}
INSERT INTO t_user (USER_ID, USER_NAME, USER_PASSWORD, USER_EMAIL) VALUES (1, 'DemoName', 'DemoPWD', 'Demo@email.com');  
{% endcodeblock %}

数据库部分OK！

***

### 创建服务提供的Project
因为要demo Dubbo的部分，除了为了搭框架的项目，我需要另外建一个小项目来模拟提供服务

在eclipse中建一个Maven Project，我的项目名为service.Provider 这个可以自取。
项目结构如下:

![ServiceProvider](http://ogy9ohfjc.bkt.clouddn.com/SpringMVC-Mybatis-Dubbo-MySQL/ServiceProvider.png "ServiceProvider")__

在Package Demo.Service下提供服务的接口以及实现
在resources文件夹下放Provider.xml

#### POM.xml
{% codeblock POM.xml lang:xml%}
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>service.provider</groupId>
    <artifactId>service.provider</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>service.provider</name>
    <url>http://maven.apache.org</url>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <!-- spring版本号 -->
        <spring-version>4.2.6.RELEASE</spring-version>

        <!-- log4j日志文件管理包版本 -->
        <slf4j.version>1.7.21</slf4j.version>
        <log4j.version>1.2.17</log4j.version>

    </properties>

    <dependencies>
        <!-- 添加Spring依赖 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring-version}</version>
        </dependency>

        <!-- 日志文件管理包 -->
        <!-- log start -->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>${log4j.version}</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>${slf4j.version}</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>${slf4j.version}</version>
        </dependency>
        <!-- log end -->


        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>dubbo</artifactId>
            <version>2.5.3</version>
            <exclusions>
                <exclusion>
                    <artifactId>spring</artifactId>
                    <groupId>org.springframework</groupId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>com.github.sgroschupf</groupId>
            <artifactId>zkclient</artifactId>
            <version>0.1</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
{% endcodeblock %}


#### DemoService接口
{% codeblock DemoService lang:java %}
package demo.service;  
    
public interface DemoService {  
    public String sayHello();  
} 
{% endcodeblock %}

#### DemoServiceImp实现接口
{% codeblock DemoServiceImpl lang:java %}
package demo.service;

public class DemoServiceImpl implements DemoService {

    public String sayHello() {
        System.out.println("hello dubbo!");
        return "hello dubbo!";
    }

}

{% endcodeblock %}

#### Provider.xml
配置dubbo的provider.xml文件，负责将服务提交到本地的zookeeper上

{% codeblock Provider.xml lang:xml %}
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
    xsi:schemaLocation="http://www.springframework.org/schema/beans    
        http://www.springframework.org/schema/beans/spring-beans.xsd    
        http://code.alibabatech.com/schema/dubbo    
        http://code.alibabatech.com/schema/dubbo/dubbo.xsd    
        ">

    <!-- 提供方应用信息，用于计算依赖关系 -->
    <dubbo:application name="hello-world-app" />
    <!-- 使用multicast广播注册中心暴露服务地址 -->
    <dubbo:registry protocol="zookeeper" address="localhost:2181" />
    <!-- 用dubbo协议在20880端口暴露服务 -->
    <dubbo:protocol name="dubbo" port="20880" />

    <!-- 声明需要暴露的服务接口 -->
    <dubbo:service interface="demo.service.DemoService" ref="demoService" />       <!-- 和本地bean一样实现服务 -->
    <!-- 和本地bean一样实现服务 -->
    <bean id="demoService" class="demo.service.DemoServiceImpl" />
</beans>    
{% endcodeblock %}

#### DubboProviderDemo
启动Provider服务的入口, 在demo.service.DemoServiceDemo包内

{% codeblock DubboProviderDemo lang:java %}
package demo.service.DemoServiceDemo;

import java.io.IOException;

import org.springframework.context.support.ClassPathXmlApplicationContext;

public class DubboProviderDemo {

    @SuppressWarnings("resource")
    public static void main(String[] args) throws InterruptedException {

        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(
                new String[] { "/provider.xml" });
        context.start();
        System.out.println("这里是dubbo-provider服务，按任意键退出");
        try {
            System.in.read();
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}

{% endcodeblock %}

#### 启动服务
运行DubboProviderDemo, run as Application
然后在Term中运行`jps`, 结果如下
{% codeblock jps lang:sh %}
$ jps
2708
3241 DubboProviderDemo  // zookeeper中provider的进程
3242 Jps
2125 QuorumPeerMain // zookeeper进程
{% endcodeblock %}

好了！ provider提供服务的这个项目完成了！

***
### 开始建立 搭框架的这个Project
main-project为我们要搭框架的主项目
项目结构如下:
图长了一点儿（忍受一下Orz)
![mainProject](http://ogy9ohfjc.bkt.clouddn.com/SpringMVC-Mybatis-Dubbo-MySQL/mainProject.png "mainProject")__

#### POM文件

{% codeblock pom.xml lang:xml%}
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.njm</groupId>
    <artifactId>main-project</artifactId>
    <name>main-project</name>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>war</packaging>
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <!-- spring版本号 -->
        <spring-version>4.2.6.RELEASE</spring-version>
        <!-- junit版本号 -->
        <junit-version>4.11</junit-version>

        <!-- log4j日志文件管理包版本 -->
        <slf4j.version>1.7.21</slf4j.version>
        <log4j.version>1.2.17</log4j.version>

        <!-- mybatis版本号 -->
        <mybatis-version>3.4.0</mybatis-version>
    </properties>

    <dependencies>
        <!--单元测试依赖 -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>${junit-version}</version>
            <scope>test</scope>
        </dependency>

        <!-- 添加Spring依赖 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${spring-version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${spring-version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>${spring-version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring-version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>${spring-version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>${spring-version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>${spring-version}</version>
        </dependency>

        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>

        <!-- http://mvnrepository.com/artifact/javax.servlet/servlet-api -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>
        </dependency>

        <!-- 日志文件管理包 -->
        <!-- log start -->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>${log4j.version}</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>${slf4j.version}</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>${slf4j.version}</version>
        </dependency>
        <!-- log end -->

        <!--mybatis依赖 -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>${mybatis-version}</version>
        </dependency>

        <!-- mybatis/spring包 -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>1.3.0</version>
        </dependency>

        <!-- mysql驱动包 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.38</version>
        </dependency>

        <!-- dubbo -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>dubbo</artifactId>
            <version>2.5.3</version>
            <exclusions>
                <exclusion>
                    <artifactId>spring</artifactId>
                    <groupId>org.springframework</groupId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>com.github.sgroschupf</groupId>
            <artifactId>zkclient</artifactId>
            <version>0.1</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
    
    
</project>
{% endcodeblock %}

#### Java 文件
##### User映射类
User类的属性对应数据库中的属性
{% codeblock User lang:java %}
package com.njm.domain;

/**
 * User映射类
 * 
 */
public class User {
    private Integer userId;
    private String userName;
    private String userPassword;
    private String userEmail;

    public Integer getUserId() {
        return userId;
    }

    public void setUserId(Integer userId) {
        this.userId = userId;
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public String getUserPassword() {
        return userPassword;
    }

    public void setUserPassword(String userPassword) {
        this.userPassword = userPassword;
    }

    public String getUserEmail() {
        return userEmail;
    }

    public void setUserEmail(String userEmail) {
        this.userEmail = userEmail;
    }

    @Override
    public String toString() {
        return "User [userId=" + userId + ", userName=" + userName
                + ", userPassword=" + userPassword + ", userEmail=" + userEmail
                + "]";
    }
    
}

{% endcodeblock %}

##### User DAO类
DAO(Data Access Object) 数据访问对象是第一个面向对象的数据库接口。
对数据源的访问操作抽象封装在一个公共API中。
用程序设计的语言来说，就是建立一个接口，接口中定义了此应用程序中将会用到的所有事务方法。在这个应用程序中，当需要和数据源进行交互的时候则使用这个接口，并且编写一个单独的类来实现这个接口在逻辑上对应这个特定的数据存储。

在本项目中UserDao定义了对User数据库的查询接口

{% codeblock DubboProviderDemo lang:java %}
package com.njm.dao;

import com.njm.domain.User;

/**
 * 功能概要：User的DAO类
 * 
 */
public interface UserDao {
    /**
     * 
     * @param userId
     * @return
     */
    public User selectUserById(Integer userId);

}
{% endcodeblock %}


##### UserMapper.xml
{% codeblock UserMapper.xml lang:xml %}
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"  
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.njm.dao.UserDao">
<!--设置domain类和数据库中表的字段一一对应，注意数据库字段和domain类中的字段名称一致，此处一定要！-->
    <resultMap id="BaseResultMap" type="com.njm.domain.User">
        <id column="USER_ID" property="userId" jdbcType="INTEGER" />
        <result column="USER_NAME" property="userName" jdbcType="CHAR" />
        <result column="USER_PASSWORD" property="userPassword" jdbcType="CHAR" />
        <result column="USER_EMAIL" property="userEmail" jdbcType="CHAR" />
    </resultMap>
    <!-- 查询单条记录 -->
    <select id="selectUserById" parameterType="int" resultMap="BaseResultMap">
        SELECT * FROM t_user WHERE USER_ID = #{userId}
    </select>
</mapper>
{% endcodeblock %}

##### UserService类
{% codeblock UserService类 lang:java %}
package com.njm.service;

import com.njm.domain.User;

/**
 * 功能概要：UserService接口类
 * 
 */
public interface UserService {
    User selectUserById(Integer userId);
}
{% endcodeblock %}

##### UserServiceImpl，接口的实现
{% codeblock UserServiceImpl lang:java %}
package com.njm.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.njm.dao.UserDao;
import com.njm.domain.User;

/**
 * 功能概要：UserService实现类
 * 
 */
@Service
public class UserServiceImpl implements UserService{
    @Autowired
    private UserDao userDao;

    public User selectUserById(Integer userId) {
        return userDao.selectUserById(userId);
        
    }
}
{% endcodeblock %}

##### DemoService
注意这个类，逻辑上与以上的类分离。

之前的类用来测试Mybatis

而这个接口是在上一个服务的provider中定义的接口，该类在这个项目中用来测试dubbo的customer服务。
{% codeblock DubboProviderDemo lang:java %}
package demo.service;  
    
public interface DemoService {  
    public String sayHello();  
} 
{% endcodeblock %}

##### userController
添加一个Controller实现SpringMVC

{% codeblock userController lang:java %}
package com.njm.controller;

import org.apache.log4j.Logger;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

import com.njm.domain.User;
import com.njm.service.UserService;

import demo.service.DemoService;
  

@Controller  
public class userController {  
      
    @Autowired
    private UserService userService;
    Logger logger = Logger.getLogger(userController.class);
    
    @Autowired
    private DemoService helloService;
 
    // 第一步，最简单的测试SpringMVC是否可用
    @RequestMapping(value="/hello",produces="text/html;charset=UTF-8" )   
    @ResponseBody  
    private String hello(){  
        return "hello MVC";  
    } 

    // 第二步，测试Mybatis
    @RequestMapping(value="/",produces="text/html;charset=UTF-8" )   
    @ResponseBody  
    private String selectUserByIdTest(){  
        User user = userService.selectUserById(1);
        System.out.println("[User email =] "+ user.getUserEmail());
        return user.getUserEmail();  
    }  
    
   
    // 第三步，测试Dubbo
    @RequestMapping(value="/dubbo",produces="text/html;charset=UTF-8" )   
    @ResponseBody  
    private String dubbo(){  
        return helloService.sayHello();  
    } 
    
    
}  
{% endcodeblock %}

#### 资源配置
接下来是资源配置，配置xml和properties文件将这个框架串起来

##### web.xml
web.xml是唯一会随服务器启动自动加载的文件
是web project生命周期的第一步（所以我把它看做入口）
路径为/src/main/webapp/WEB-INF/web.xml

{% codeblock web.xml lang:xml %}
<?xml version="1.0" encoding="UTF-8"?>
<web-app id="WebApp_ID" version="3.0"
    xmlns="http://java.sun.com/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd">

    <!-- 配置 Spring -->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:application.xml</param-value>
    </context-param>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    <!-- 防止Spring内存溢出监听器 -->
    <listener>
        <listener-class>org.springframework.web.util.IntrospectorCleanupListener</listener-class>
    </listener>

    <!-- 配置springmvc -->
    <servlet>
        <servlet-name>springMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring-mvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springMVC</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <!-- 字符集过滤器 -->
    <filter>
        <filter-name>encodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
        <init-param>
            <param-name>forceEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
{% endcodeblock %}
web.xml中用到了application.xml来配置spring和spring-mvc.xml来配置springMVC

##### 配置Spring:application.xml
首先application.xml 配置Spring框架的文件
路径为/src/main/resources/application.xml
{% codeblock application.xml lang:xml %}
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xsi:schemaLocation="  
           http://www.springframework.org/schema/beans  
           http://www.springframework.org/schema/beans/spring-beans-3.0.xsd  
           http://www.springframework.org/schema/aop  
           http://www.springframework.org/schema/aop/spring-aop-3.0.xsd
           http://www.springframework.org/schema/context  
           http://www.springframework.org/schema/context/spring-context-3.0.xsd">

    <!-- 引入jdbc配置文件 -->
    <bean id="propertyConfigurer"
        class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
        <property name="locations">
            <list>
                <value>classpath:properties/*.properties</value>
                <!--要是有多个配置文件，只需在这里继续添加即可 -->
            </list>
        </property>
    </bean>

    <!-- 配置数据源 -->
    <bean id="dataSource"
        class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <!-- 不使用properties来配置 -->
        <!-- <property name="driverClassName" value="com.mysql.jdbc.Driver" /> 
            <property name="url" value="jdbc:mysql://localhost:3306/Users" /> <property 
            name="username" value="root" /> <property name="password" value="root" /> -->
        <!-- 使用properties来配置 -->
        <property name="driverClassName">
            <value>${jdbc_driverClassName}</value>
        </property>
        <property name="url">
            <value>${jdbc_url}</value>
        </property>
        <property name="username">
            <value>${jdbc_username}</value>
        </property>
        <property name="password">
            <value>${jdbc_password}</value>
        </property>
    </bean>

    <!-- 自动扫描了所有的XxxxMapper.xml对应的mapper接口文件，这样就不用一个一个手动配置Mpper的映射了，只要Mapper接口类和Mapper映射文件对应起来就可以了。 -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.njm.dao" />
    </bean>

    <!-- 配置Mybatis的文件 ，mapperLocations配置**Mapper.xml文件位置，configLocation配置mybatis-config文件位置 -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource" />
        <property name="mapperLocations" value="classpath*:com/njm/mapper/**/*.xml" />
        <property name="configLocation" value="classpath:mybatis/mybatis-config.xml" />
        <!-- <property name="typeAliasesPackage" value="com.tiantian.ckeditor.model" 
            /> -->
    </bean>

    <!-- 自动扫描注解的bean -->
    <context:component-scan base-package="com.njm.service" />

    <!-- 引入dubbo-Consumer -->
    <import resource="Consumer.xml" />
</beans>
{% endcodeblock %}
application.xml文件中导入了 jdbc.properties文件和Consumer.xml和mybatis-config.xml
jdbc.properties文件配置连接数据库属性
Consumer.xml用来配置dubbo的Consumer来接收provider的服务
mybatis-config.xml用来配置Mybatis

##### spring-mvc.xml配置springMVC
然后是spring-mvc.xml
路径为/src/main/resources/spring-mvc.xml
{% codeblock spring-mvc.xml lang:xml %}
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:mvc="http://www.springframework.org/schema/mvc"
    xsi:schemaLocation="  
    http://www.springframework.org/schema/beans  
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd  
    http://www.springframework.org/schema/context  
    http://www.springframework.org/schema/context/spring-context-3.0.xsd  
    http://www.springframework.org/schema/mvc    
    http://www.springframework.org/schema/mvc/spring-mvc-3.0.xsd">

    <!-- 默认的注解映射的支持 -->
    <mvc:annotation-driven />

    <!-- 自动扫描该包，使SpringMVC认为包下用了@controller注解的类是控制器 -->
    <context:component-scan base-package="com.njm.controller" />

    <!-- 定义跳转的文件的前后缀 ，视图模式配置 -->
    <bean
        class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!-- 这里的配置我的理解是自动给后面action的方法return的字符串加上前缀和后缀，变成一个 可用的url地址 -->
        <property name="prefix" value="/WEB-INF/jsp/" />
        <property name="suffix" value=".jsp" />
    </bean>
</beans>  
{% endcodeblock %}


##### jdbc.properties文件
路径/src/main/resources/properties/jdbc.properties
{% codeblock jdbc.properties lang:properties %}
jdbc_driverClassName=com.mysql.jdbc.Driver
jdbc_url=jdbc:mysql://localhost:3306/Users
jdbc_username=root
jdbc_password=root
{% endcodeblock %}

##### mybatis-config.xml
路径/src/main/resources/mybatis/mybatis-config.xml
此项目比较简单，没有更多的配置
{% codeblock mybatis-config.xml lang:xml %}
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"  
"http://mybatis.org/dtd/mybatis-3-config.dtd">  
<configuration>    
</configuration>
{% endcodeblock %}

##### Consumer.xml
路径/src/main/resources/Consumer.xml
Consumer文件来定义从zookeeper上接收dubbo服务
{% codeblock Consumer.xml lang:xml %}
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
    xsi:schemaLocation="http://www.springframework.org/schema/beans    
        http://www.springframework.org/schema/beans/spring-beans.xsd    
        http://code.alibabatech.com/schema/dubbo    
        http://code.alibabatech.com/schema/dubbo/dubbo.xsd    
        ">
    <!-- 消费方应用名，用于计算依赖关系，不是匹配条件，不要与提供方一样 -->
    <dubbo:application name="consumer-of-helloworld-app" />
    <!-- 使用multicast广播注册中心暴露发现服务地址 -->
    <dubbo:registry protocol="zookeeper" address="127.0.0.1:2181" />
    <!-- 生成远程服务代理，可以和本地bean一样使用demoService -->
    <dubbo:reference id="demoService" interface="demo.service.DemoService" />
</beans> 
{% endcodeblock %}

终于配置完了！启动项目！想想还有点小激动

#### 启动项目
添加一个tomcat服务器，启动服务
在浏览器中进行测试
{% codeblock Test %}
// 第一步，最简单的测试SpringMVC是否可用
浏览器测试 http://localhost:8080/main-project/hello
输出 hello MVC 测试通过

// 第二步，测试Mybatis
测试 http://localhost:8080/main-project/
输出 Demo@email.com
这里偷了个懒，直接将 “/”映射到查询id为1的用户的email

// 第三步，测试Dubbo
http://localhost:8080/main-project/dubbo
输出 hello dubbo! 测试通过
{% endcodeblock %}

***
### 一些报错
#### JDK版本统一
1. 右键项目 -> Properties -> Java Build Path 
2. 右键项目 -> Properties -> Project Facet -> Java
3. Eclipse -> Preference -> Java -> Compiler 
以上三处版本需要统一

如果每次Maven update都会变化的话
在POM文件中添加
{% codeblock compiler-plugin lang:xml %}
// 此处JDK版本为1.8
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.1</version>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
            </configuration>
        </plugin>
    </plugins>
</build>
{% endcodeblock %}

然后就能统一了！

#### SQLNonTransientConnectionException异常
java.sql.SQLNonTransientConnectionException异常
解决方案：
mysql-connector-java版本是6+的驱动是不能连接mysql5+的数据库，换了5+的驱动版本，连接正常

*** 

### 参考
{% link Spring+Mybatis+Maven+MySql搭建实例 http://blog.csdn.net/evankaka/article/details/48784641 %}




