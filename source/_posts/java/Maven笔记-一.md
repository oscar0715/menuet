---
title: Maven笔记(一)
tags:
  - Maven
  - Java
categories:
  - Technology
  - Tool
date: 2016-05-14 10:42:59
---
理一理Maven
根据许晓斌先生的《Maven实战》整理
我就挑实用的说，用人话说。

Maven笔记(一) 过了一遍前三章的内容
<!-- more -->

***
# Maven简介
Maven 主要服务于Java平台的项目构建，依赖管理和项目信息管理。

##构建
>构建（build）除了编写源代码，我们每天花很多时间在编译，运行单元测试，生成文档，打包和部署等工
作上，这就是构建。

Maven 定义了了完整的构建生命周期，能够自动化构建，实现构建任务，于是程序猿不必花时间在定义构建过程上。

##依赖管理 项目信息管理
1. 管理jar包依赖
2. 管理项目信息

# Maven使用入门

## 编写POM文件

### POM文件
{% codeblock lang:xml %}
<!-- xml头，指定该文档版本和编码方式 -->
<?xml version="1.0" encoding="UTF-8"?>  

<!-- project是POM.xml的根元素 -->
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
http://maven.apache.org/maven-v4_0_0.xsd">

    <modelVersion>4.0.0</modelVersion>
    <groupId>com.juvenxu.mvnbook</groupId>
    <artifactId>hello-world</artifactId>
    <version>1.0-SNAPSHOT</version>
    <name>Maven Hello World Project</name>
</project>
{% endcodeblock %}


## 编写主代码
Maven假设项目的主代码位于src/main/java目录下

## 编写测试代码
测试代码在src/test/java目录下。
首先要为项目添加JUnit依赖，在POM中添加dependencies元素，在下一级中添加一个dependency元素。
{% codeblock lang:xml %}
<dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.7</version>
      <!-- scope为依赖范围，如果在主代码中，import JUnit代码就会造成编译错误。 -->
      <!-- 如果不声明依赖范围，默认为compile，表示依赖对主代码和测试代码都有效 -->
      <scope>test</scope>
    </dependency>
  </dependencies>
{% endcodeblock %}

单元测试包含三个步骤:
1. 准备测试以及数据
2. 执行要执行的行为
3. 检查结果

{% codeblock 测试代码 lang:java %}
package com.juvenxu.mvnbook.helloworld;

import static org.junit.Assert.assertEquals;
import org.junit.Test;

import com.juvenxu.mvnbook.helloworld.HelloWorld;

public class HelloWorldTest
{
    @Test    // Junit4 中约定所有需要执行测试的方法都以@Test开头
    public void testSayHello()
    {
        HelloWorld helloWorld = new HelloWorld();

        String result = helloWorld.sayHello();  // 执行方法并保存在result变量中

        assertEquals( "Hello Maven", result ); //使用JUnit框架的Assert类检查结果
    }
}
{% endcodeblock %}

然后执行
{% codeblock lang:shell %}
mvn clean test
// 如果报错
// 可能原因是 compiler 插件默认只支持编译java1.3,而只有1.5以上版本才支持@的注释 
// 详见《Maven实战》P33
// 不过我没碰到
{% endcodeblock %}

## 打包和运行
{% codeblock lang:bash %}
<!-- jar插件的jar目标将项目主代码打包成一个jar文件 -->
$ mvn clean package
安装任务 install:install, 将任务将项目输出到Maven本地仓库中。
$ mvn clean install
{% endcodeblock %}

默认打包生成的jar文件不能直接运行，因为main方法不会添加到manifest中。
为了生成可执行的jar文件，需要借助maven-shade-plugin
{% codeblock lang:shell %}
plugin元素在 project, build, plugins
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-shade-plugin</artifactId>
    <version>1.2.1</version>
    <executions>
        <execution>
            <phase>package</phase>
            <goals>
                <goal>shade</goal>
            </goals>
            <configuration>
                  <transformers>
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                        <!-- 配置mainClass路径 -->
                        <mainClass>com.juvenxu.mvnbook.helloworld.HelloWorld</mainClass>
                    </transformer>
                  </transformers>
            </configuration>
        </execution>
    </executions>
</plugin>
{% endcodeblock %}

## 使用Archetype生成项目骨架
我们称Maven的目录结构和POM.xml为Maven文件的项目骨架 
{% codeblock lang:shell %}
<!-- Archetype插件创建骨架 -->
mvn archetype:generate
{% endcodeblock %}



***

Over！

