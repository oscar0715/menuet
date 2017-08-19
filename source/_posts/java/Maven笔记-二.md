---
title: Maven笔记(二)
tags:
  - Maven
  - Java
categories:
  - Technology
  - Tool
date: 2016-05-16 15:51:54
---
Maven笔记(二) 主要介绍坐标，依赖
<!-- more -->

***


# 坐标

坐标规划的原则是基于项目域名衍生
1. GroupId: 定义了Maven项目隶属的实际的项目
2. artifactId: 定义了实际项目中的一个Maven模块（项目）
3. version: <主版本>.<次版本>.<增量版本>-<限定符>
* 主版本主要表示大型架构变更，次版本主要表示特性的增加，增量版本主要服务于bug修复，而限定符如alpha、beta等等是用来表示里程碑。
4. Packaging: 打包方式
5. Classifier: 定义构建输出的一些附属附件
* 如果能是dog-cli-1.0-dist.zip就最好了。这里的dist就是classifier，默认Maven只生成一个构件，我们称之为主构件，那当我们希望Maven生成其他附属构件的时候，就能用上classifier。常见的classifier还有如dog-cli-1.0-sources.jar表示源码包，dog-cli-1.0-javadoc.jar表示JavaDoc包等等。

# 依赖

## 依赖范围 scope
三种classpath
1. 编译classpath
2. 测试classpath
3. 运行classpath

依赖范围
1. compile: 对三种classpath都有效
2. test: 对测试classpath有效
3. provided: 已提供依赖范围，编译和测试时有效。在运行时，容器已经提供，所以不需要重复提供
4. Runtime: 运行时依赖范围，测试和运行classpath有效，编译时不需要。
5. system: 范围与provided一致，此类依赖与本机系统绑定而不是Maven仓库（不可移植）
6. import: 

## 传递性依赖
{% blockquote  Maven依赖机制简介 http://ifeve.com/maven-dependency-mechanism/ 并发编程网 %}
假设你的项目依赖于一个库，而这个库又依赖于其他库。你不必自己去找出所有这些依赖，你只需要加上你直接依赖的库，Maven会隐式的把这些库间接依赖的库也加入到你的项目中。这个特性是靠解析从远程仓库中获取的依赖库的项目文件实现的。一般的，这些项目的所有依赖都会加入到项目中，或者从父项目继承，或者通过传递性依赖。
{% endblockquote %}


## 依赖调解
版本选择时
1. 路径最近者优先（依赖长度比较短的优先 
2. 第一声明者优先（声明顺序靠前的优先

## 最佳实践

### 排除依赖
当不想引入某个传递依赖时，用exclusion

###归类依赖
假如有个项目有很多关于SpringFramework的依赖，它们分别是org.springframework:spring-core:2.5.6、org.springframework:spring-bean:2.5.6、org.springframework:spring-context:2.5.6，它们是来自同一项目的不同模块。因此，所有这些依赖的版本会一起升级。因为它们版本是相同的，所以应该在一个唯一的地方定义版本，并且在dependency声明中引用这一版本。这样，在升级时只需要修改一处即可
{% codeblock Spring的归类依赖 lang:xml %}
<project>
   
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.juven.mvnbook.account</groupId>
    <artifactId>accout-email</artifactId>
    <version>1.0.0-SNAPSHOT</version>

    <properties>
        <springframework.version>1.5.6</springframework.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${springframework.version}</version>
        </dependency> 
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>${springframework.version}</version>
        </dependency>       
    </dependencies>
</project>
{% endcodeblock %}

# 仓库
## 快照版本
Maven中的快照版本的概念很有意思。
如果是稳定的Release版本，那么同样的版本和同样的坐标，Maven不会对照远程仓库更新。
如果是Snapshot版本，例如2.1-SNAPSHOT，maven会自动打上时间戳，即使版本号相同，Maven会在发现新的更新时间时及时跟新版本。

## 镜像
配置中央仓库在国内的镜像，下载会快一些。修改~/.M2文件夹下的setting.xml文件
{% codeblock lang:xml %}
<mirrors>
    <!--     
     | mirror
     | Specifies a repository mirror site to use instead of a given repository. The repository that
     | this mirror serves has an ID that matches the mirrorOf element of this mirror. IDs are used
     | for inheritance and direct lookup purposes, and must be unique across the set of mirrors.
     | -->
    <mirror>
      <id>maven.net.cn</id>
      <mirrorOf>central</mirrorOf>
      <name>Maven China Mirror</name>
      <url>http://maven.net.cn/content/groups/public/</url>
    </mirror>
  </mirrors>  
{% endcodeblock %}



