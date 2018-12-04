---
title: 使用 django-userena
tags:
	- Python
	- Django
categories:
	- Techonology
	- Django
date: 2016-12-29 16:15:58
---
注册模块不好做啊，继续学习 django-userena

<!-- more -->

***

# Installation
用 pip 安装嘛

***

# Required settings
在 Django 中修改三处设置：
`AUTHENTICATION_BACKENDS`, `INSTALLED_APPS` 和 optionally `MIDDLEWARE_CLASSES`

1. 在 `INSTALLED_APPS` 中
	- 添加 `userena`, `guardian` 和 `easy_thumbnails`
	- `django.contrib.sites` 也要有
2. 在 `AUTHENTICATION_BACKENDS` 中
	- 添加 `UserenaAuthenticationBackend` 和 django-guardian 中的 `ObjectPermissionBackend`
	``` python
	AUTHENTICATION_BACKENDS = (
		'userena.backends.UserenaAuthenticationBackend',
		'guardian.backends.ObjectPermissionBackend',
		'django.contrib.auth.backends.ModelBackend',
	)
	```

***

# Start New App
为 `Userena`  创建一个 App: `account`:
`python manage.py startapp accounts`

创建好以后，也要把 `account` 添加到 `INSTALLED_APPS`

***

# Email Backend
接下来就要设置 Email Backend, 默认用 SMTP backend， 如果 SMTP 有问题，则要在 `setting.py` 重新设置： 
`EMAIL_BACKEND = 'django.core.mail.backends.dummy.EmailBackend'`

如果要用 `GMail SMTP`， 则在 `setting.py` 中设置 ：
``` python
EMAIL_USE_TLS = True
EMAIL_HOST = 'smtp.gmail.com'
EMAIL_PORT = 587
EMAIL_HOST_USER = 'yourgmailaccount@gmail.com'
EMAIL_HOST_PASSWORD = 'yourgmailpassword'
```

***

# Profiles
Userena 需要定义一个 profile，提供给 Django 的 `AUTH_PROFILE_MODULE` 使用的。
有两个抽象类可以继承：
1. `UserenaBaseProfile`:
	一个基本的 profile，包括了头像和一些基本的属性
2. `UserenaLanguageBaseProfile`:
	多了一个用户可以选择多语言的属性

``` python
from django.contrib.auth.models import User
from django.utils.translation import ugettext as _
from userena.models import UserenaBaseProfile

class MyProfile(UserenaBaseProfile):
	user = models.OneToOneField(User, 
		unique=True, 
		verbose_name=_('user'),
		related_name='my_profile')
	favourite_snack = models.CharField(_('favourite snack'),
		max_length=5)
```

***

# The URI’s
我们之前创建了 `account` app，现在要给相关的 view 定义 URL。 修改 url.py 中的 URLconf ：
`(r'^accounts/', include('userena.urls')),`

***

# Required settings

设置 `ANONYMOUS_USER_ID` 为 -1 （一脸懵逼）。
把定制的 profile 设置到 AUTH_PROFILE_MODULE
``` python
ANONYMOUS_USER_ID = -1

AUTH_PROFILE_MODULE = 'accounts.MyProfile'
```

为了把 Userena 整合到 Django 里，还要做以下设置：
``` python
USERENA_SIGNIN_REDIRECT_URL = '/accounts/%(username)s/'
LOGIN_URL = '/accounts/signin/'
LOGOUT_URL = '/accounts/signout/'
```

***

# 一些报错

## cannot import name 'patterns'
from django.conf.urls import url, patterns, include, handler404, handler500
ImportError: cannot import name 'patterns'

更新一下 django-guardian 的版本
`pip install django-guardian --upgrade`

## Permission matching query does not exist

Sometimes Django decides not to install the default permissions for a model and thus the change_profile permission goes missing. To fix this, run the `check_permissions` in Commands.. This checks all permissions and adds those that are missing.

`./manage.py check_permissions`

重新装了一遍数据库，这个要重新来一遍…………不要再忘了

## SMTPAuthenticationError
https://www.google.com/settings/security/lesssecureapps
到这里修改成启用

***

# 一些 reference
1. http://www.dear-shen.com/2015/04/09/django-userena%E4%BD%BF%E7%94%A8%E8%AE%B0%E5%BD%95/
2. http://www.ctolib.com/topics-45280.html
3. https://segmentfault.com/q/1010000000435039
4. [How to override a view from an external Django app](http://blog.yourlabs.org/post/19777151073/how-to-override-a-view-from-an-external-django-app)
5. [Django第三方应用开发经验记录](https://github.com/wellown/FlexyGears/blob/master/docs/02%20-%20Django%E5%BC%80%E5%8F%91%E7%BB%8F%E9%AA%8C/02%20-%20django%E7%AC%AC%E4%B8%89%E6%96%B9%E7%BB%84%E4%BB%B6%E4%BD%BF%E7%94%A8%E8%AE%B0%E5%BD%95.rst)

# 重写 Userena 的 template

This is the signin method signature for Userena(source) -

>def signin(request, auth_form=AuthenticationForm,
	   template_name='userena/signin_form.html',
	   redirect_field_name=REDIRECT_FIELD_NAME,
	   redirect_signin_function=signin_redirect, extra_context=None):

As you can see, there is a template_name method that holds the template location. You can override this. In your urls.py, you can use it like -

>url(r'^signin/', 'userena.views.signin', {'template_name': 'signin.html'}, name="signin"),

You can then create the signin.html page inside your templates folder and extend base.html. The signin view sends the login form in a variable called form. You can see the source. You can use the form on your template signin.html like {{ form.as_p }}. You can also format each field individually if you can follow the userena.forms. AuthenticationForm. Again, check the source code. You can do the same for any view Userena has that allows overriding like this.

When in doubt, read the source code. :)

[Reference](http://stackoverflow.com/questions/15652462/django-userena-customization/15652729#15652729)


重写以后可以放在叫做 account 的 app 里。这里要特别注意 template 的导入顺序
[[Django学习]Template导入顺序](http://blog.donews.com/limodou/archive/2006/03/10/761749.aspx)

# 获取当前的用户，注意获取的是 user 类的 profile
``` python
	user = models.OneToOneField(User, 
		on_delete=models.CASCADE,
		unique=True,
		verbose_name=_('user'),
		related_name='my_profile')
```

如果user是这么定义的

要获取这个 profile的话，我们就用 related_name：
`current_user = request.user.my_profile`
***
