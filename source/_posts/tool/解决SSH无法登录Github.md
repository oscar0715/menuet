---
title: 解决SSH无法登录Github
tags:
  - SSH
  - Tool
categories:
  - Technology
  - Tool
date: 2016-05-18 14:09:24
---
Hexo deploy的时候报错
`ssh connect to host github com port 22 operation timed out`

`ssh -T git@github.com` 也连不上

Github 官方给出解决方案:通过HTTPS端口连接SSH
{% link Using SSH over the HTTPS port https://help.github.com/articles/using-ssh-over-the-https-port/ %}
<!-- more -->

***

### 解决

1. 先测试通过HTTPS端口连接SSH是否可行
{% codeblock run this SSH command lang:shell  https://help.github.com/articles/using-ssh-over-the-https-port/ Github %}
$ ssh -T -p 443 git@ssh.github.com
Hi username! You've successfully authenticated, but GitHub does not
provide shell access.
{% endcodeblock %}
如果显示如上，则可行

2. 在~/.ssh/config文件中添加如下设置
{% codeblock config lang:shell  https://help.github.com/articles/using-ssh-over-the-https-port/ Github %}
$ ssh -T -p 443 git@ssh.github.com
Host github.com
  Hostname ssh.github.com
  Port 443
{% endcodeblock %}

3. 再运行SSH命令
{% codeblock run this SSH command lang:shell  https://help.github.com/articles/using-ssh-over-the-https-port/ Github %}
ssh -T git@github.com
Hi username! You've successfully authenticated, but GitHub does not
provide shell access.
{% endcodeblock %}

Works！

***

### 参考
1. {% link Using SSH over the HTTPS port https://help.github.com/articles/using-ssh-over-the-https-port/ %}

