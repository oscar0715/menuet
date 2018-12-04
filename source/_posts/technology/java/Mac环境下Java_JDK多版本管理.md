---
title: Mac环境下Java_JDK多版本管理
tags:
  - Tool
  - Java_JDK
  - jenv
categories:
  - Technology
  - Tool
date: 2016-05-14 16:15:44
---
现在电脑上安装的JDK版本为1.8，感觉出现了很多问题。
打算把1.8卸载掉，安装1.7版本。
查资料的时候顺便又发现了Mac上JDK多版本的管理工具，顺便学习了。
<!-- more -->

***

## 卸载JDK 1.8版本
1. 在iTerm（或者Terminal）中执行命令
{% codeblock lang:bash %}
sudo rm -fr /Library/Internet\ Plug-Ins/JavaAppletPlugin.plugin
sudo rm -fr /Library/PreferencePanes/JavaControlPanel.prefpane
{% endcodeblock %}

2. 上述命令删除了plugin和System Perference中的控制项，而不是删除实际执行程序。
删除执行程序的方法参考 {% link Removing Java 8 JDK from Mac http://stackoverflow.com/questions/19039752/removing-java-8-jdk-from-mac %}
{% codeblock lang:bash %}
sudo rm -rf /Library/Java/JavaVirtualMachines/jdk<version>.jdk
// 我的版本为 jdk1.8.0_51.jdk
// 即  sudo rm -rf /Library/Java/JavaVirtualMachines/jdk1.8.0_51.jdk
sudo rm -rf /Library/PreferencePanes/JavaControlPanel.prefPane
sudo rm -rf /Library/Internet\ Plug-Ins/JavaAppletPlugin.plugin
sudo rm -rf /Library/LaunchAgents/com.oracle.java.Java-Updater.plist
sudo rm -rf /Library/PrivilegedHelperTools/com.oracle.java.JavaUpdateHelper
sudo rm -rf /Library/LaunchDaemons/com.oracle.java.JavaUpdateHelper.plist
sudo rm -rf /Library/Preferences/com.oracle.java.Helper-Tool.plist
{% endcodeblock %}

3. 至此 JDK 1.8已经完全卸载了 再输入 `JAVA -version` 系统会提示安装JDK

## 安装JDK 1.7
1. 去Oracle官网下{% link 1.7版本JDK http://www.oracle.com/technetwork/cn/java/javase/downloads/jdk7-downloads-1880260.html %}
2. 下来一个dmg文件， 安装之 
3. 继续安装JDK 1.8，与上同理。安装完执行 `java -version` 版本来到了1.8
4. 我们来瞅瞅当前系统的Java版本，执行 `/usr/libexec/java_home -verbose`
{% codeblock %}
OscarMacBook:downloads oscar$ /usr/libexec/java_home -verbose
Matching Java Virtual Machines (2):
    1.8.0_91, x86_64:   "Java SE 8" /Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home
    1.7.0_79, x86_64:   "Java SE 7" /Library/Java/JavaVirtualMachines/jdk1.7.0_79.jdk/Contents/Home
{% endcodeblock %}

## 设置终端中 通过 java7 和 java8 来切换环境
1. 在 `~/.bash_profile` 中添加
{% codeblock lang:bash %}
export JAVA_7_HOME="`/usr/libexec/java_home -v '1.7*'`"
export JAVA_8_HOME="`/usr/libexec/java_home -v '1.8*'`"

alias java7='export JAVA_HOME=$JAVA_7_HOME'
alias java8='export JAVA_HOME=$JAVA_8_HOME'

# Default java7
export JAVA_HOME="`/usr/libexec/java_home -v '1.7*'`"

{% endcodeblock %}

2. 然后，我们就可以用`java7` 和 `java8` 来切换java环境了。 ** 但素，这只能影响终端中java的执行版本，不能影响图形程序执行的java版本  **

## jEnv管理Java多版本
用 jEnv 是另一种管理方法，下面给出配置，但是个人觉得bash就够了，两套不要搞乱了
博主就是先搞了bash，再搞了jenv，然后两套一起弄乱了，所以不是对jenv特别好奇就别往下看了，毕竟bahs命令已经炒鸡方便了。
当然，如果你有时间把两套都同时figure out了，还请留言指教。

### 安装jenv和配置
1. 安装 homebrew jenv(默认你的mac已经安装了brew) 
{% codeblock lang:bash %}
$ brew install jenv
{% endcodeblock %}

2. 在 `~/.bash_profile` 的最后添加
{% codeblock %}
export PATH="$HOME/.jenv/bin:$PATH"
eval "$(jenv init -)"

export JENV_ROOT=/usr/local/opt/jenv
// 鳖问我为什么，我也不知道
{% endcodeblock %}

3. 用`jenv add` 加入java版本
{% codeblock lang:bash %}
// 添加JDK 1.7
$ sudo jenv add /Library/Java/JavaVirtualMachines/jdk1.7.0_79.jdk/Contents/Home
oracle64-1.7.0.79 added
1.7.0.79 added
1.7 added
// 执行后得到
oracle64-1.7.0.79 added
1.7.0.79 added
1.7 added
//添加JDK 1.8
$ sudo jenv add /Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home
// 执行后得到
oracle64-1.8.0.91 added
1.8.0.91 added
1.8 added
{% endcodeblock %}

4. 执行 `sudo jenv versions`
{% codeblock lang:bash %}
$ sudo jenv versions
* system (set by /Users/oscar/.jenv/version)
  1.7
  1.7.0.79
  1.8
  1.8.0.91
  oracle64-1.7.0.79
  oracle64-1.8.0.91
{% endcodeblock %}

5. 执行  `sudo jenv local 1.7` 或者`sudo jenv local 1.8` 随便玩
{% codeblock lang:bash %}
$ sudo jenv local 1.7
$ java -version
java version "1.7.0_79"
Java(TM) SE Runtime Environment (build 1.7.0_79-b15)
Java HotSpot(TM) 64-Bit Server VM (build 24.79-b02, mixed mode)

//显示java路径
$ jenv which java 
{% endcodeblock %}  

6. 太乱了 `jenv remove`
额 这部分(6)先跳过，乱删版本后面会出错，先这样吧………………
{% codeblock lang:bash %}
$ sudo jenv remove 1.8
JDK 1.8 removed
{% endcodeblock %}  
直到
{% codeblock lang:bash %}
sudo jenv versions
  system
* 1.7.0.79 (set by /Users/oscar/.jenv/version)
  1.8.0.91
{% endcodeblock %}  
开心了

>整了半天没觉得 jenv和第三部分bash里设置有什么区别哎呀

### 要开始使用jEnv管理了
1. 先玩几个命令
查看当前java版本
{% codeblock lang:bash %}
$ echo $JAVA_HOME
{% endcodeblock %}  
列出所有版本的 JAVA_HOME：
{% codeblock lang:bash %}
$ /usr/libexec/java_home -V
{% endcodeblock %}  

2. 全局配置, 这个根据前面 `jenv versions` 命令的结果来选
{% codeblock lang:bash %}
$ jenv global 1.7
{% endcodeblock %}  

3. 单个项目设置
{% codeblock lang:bash %}
$ jenv local 1.7.0.79
{% endcodeblock %}  

4. shell 设置
{% codeblock lang:bash %}
$ jenv shell 1.7.0.79
{% endcodeblock %}  


## 参考
1. {% link Mac OS X JAVA多版本并存 http://blog.huatai.me/2015/12/07/multiple-java-versions-on-Mac-OS-X/ %}
2. {% link Mac上管理多个java版本 https://segmentfault.com/a/1190000004332179 %}
3. {% link 在OS X中使用jEnv管理多个Java版本 http://boxingp.github.io/blog/2015/01/25/manage-multiple-versions-of-java-on-os-x/ %}



