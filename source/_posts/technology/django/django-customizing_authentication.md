---
title: Customizing authentication in Django
tags:
	- Python
	- Django
categories:
	- Techonology
	- Django
date: 2016-12-23 11:15:58
---
Customizing authentication in Django
<!-- more -->

***

# 用户验证
Django 中的用户验证系统包括了维护了用户账号，用户分组，基于 cookie 的 session 管理。

安装：
1. 在 INSTALLED_APPS 数组中添加 
		- 'django.contrib.auth' : 默认就有的
		- 'django.contrib.contenttypes' : 维护对不同 model 的权限
2. 在 MIDDLEWARE 中
		- SessionMiddleware ： 维护每次请求的 session
		- AuthenticationMiddleware：维护用户的 session

## Django 验证系统
Django authentication system 验证系统同时包含了 authentication 和 authorization.

User 对象是验证系统的核心。有以下属性：
1. username
2. password
3. email
4. first_name
5. last_name

## Extending the existing User model

如果要把额外的信息和 User Object 关联起来，有两种方法.一种是基于 User 创建一个 proxy model。第二种是利用 OneToOneField 创建一个 profile model。

示例：
``` python
from django.contrib.auth.models import User

class Employee(models.Model):
		user = models.OneToOneField(User, on_delete=models.CASCADE)
		department = models.CharField(max_length=100)
```


***
