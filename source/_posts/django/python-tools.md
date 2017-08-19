---
title: Python Tools
date: 2016-12-22 09:47:46
tags:
	- Tool
	- Python
categories:
	- Technology
	- Tool
---

Python 中使用的工具:
(1). virtualenv
<!-- more -->

***

# virtualenv

Reference: [用virtualenv建立多个Python独立开发环境](http://www.nowamagic.net/academy/detail/1330228)

virtualenv 就是你可以新建不同的 python 环境，不同的环境下分别安装不同的或者不同版本的第三方包，然后你的 application 只要选择相应的 python 环境就好了。这样不同 python 环境下的包是独立，也就以为着你的 application 可以有自己独立的环境，不会和别的 application 混在在一起

安装：
``` sh
pip install virtualenv
```

使用：
``` sh
mkdir envs # 新建一个文件夹创建不同的环境

cd envs 
virtualenv new-env # 创建一个新的环境，同时创建了一个文件夹
# virtualenv -p python3 new-env 在python 3 环境下安装

cd new-env
source bin/activate # 启动当前这个 python 环境

# 可以用 which python 或者 which pip 来看到我们已经是在新的 python 环境下

```


