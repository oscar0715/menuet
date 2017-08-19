---
title: Mac环境下布置Eclipse中的Maven
tags:
  - Maven
  - Eclipse
  - Tool
  - Java
categories:
  - Technology
  - Tool
date: 2016-05-14 12:45:48
---
如题，Mac环境下布置Eclipse中的Maven
<!-- more -->

***

## Maven 安装

### Homebrew 不能更赞
一条命令解决
{% codeblock  %}
brew install maven

// 安装完成后查看版本，测试是否安装成功
mvn -v
{% endcodeblock %}

### M2_HOME 环境变量
brew安装Maven后，maven的安装目录为/usr/local/Cellar/maven/{版本号}}/libexec/
打开bash执行
{% codeblock  %}
$ export M2_HOME=/usr/local/Cellar/maven/{版本号}}/libexec
$ export PATH=$PATH:$M2_HOME
// 等号前后不能有空格，会报错
// 将这两行命令添加到.bash_profile文件中，启动terminal是可以自动运行。
// .bash_profile文件在用户文件夹下(cd ~). 
// 确保 $PATH 中有 JAVA_HOME和M2_HOME路径 
{% endcodeblock %}

### setting.xml文件
复制M2_HOME/conf/setting.xml文件到~/.m2/setting.xml


## Eclipse中配置Maven
### Eclipse中m2eclipse的安装
安装m2eclipse的原因是，Eclipse没有默认对maven的集成，于是m2eclipse插件可供将aven和Eclipse完美结合

1. 菜单栏 Help --> Install new software --> add
2. Location处 输入 http://download.eclipse.org/technology/m2e/releases
3. 重启后安装完成

### Eclipse中添加本地的Maven目录
1. 菜单栏 Eclipse --> preference --> 左边Maven点来下拉菜单 --> Installations
2. 右边 add --> installation home，如果用brew安装的话 地址为/usr/local/Cellar/maven/{你的电脑上Maven的版本号}/libexec


## 补充
在新版本的eclipse Maven插件环境下
导入旧版本的project
project会报错

`Plugin execution not covered by lifecycle configuration: org.apache.maven.pl`

##Over！