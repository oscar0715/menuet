---
title: Django 导入 json 数据到数据库
tags:
	- Python
	- Django
categories:
	- Techonology
	- Django
date: 2017-01-04 11:15:58
---
Django 导入 json 数据到数据库
<!-- more -->

***
Djano 支持导入 json 数据到数据库中。

不过 json 数据要符合 [fixture 格式](https://docs.djangoproject.com/en/1.10/howto/initial-data/)：

``` json
[
	{
		"model": "myapp.person",
		"pk": 1,
		"fields": {
			"first_name": "John",
			"last_name": "Lennon"
		}
	},
	{
		"model": "myapp.person",
		"pk": 2,
		"fields": {
			"first_name": "Paul",
			"last_name": "McCartney"
		}
	}
]
```

不需要指定pk时，pk 可以为 null

再用这条命令导入`python manage.py loaddata initial_data.json`

***

回顾一下流程
1. 创建 model 和 数据库
2. 把 json 改成 fixture 制定格式
3. 用 loaddate 命令导入



***
