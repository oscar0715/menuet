---
title: Django Sending email
tags:
	- Python
	- Django
categories:
	- Techonology
	- Django
date: 2016-12-28 23:54:58
---
用户模块肯定是要用到邮件的，学习一下 Django的邮件系统

<!-- more -->

***
Django 的代码写在 django.core.mail 模块里面。

# Quick example
``` python 
from django.core.mail import send_mail

send_mail(
	'Subject here',
	'Here is the message.',
	'from@example.com',
	['to@example.com'],
	fail_silently=False,
)
```

SMTP 协议下发送邮件需要 host 和 port， 记录在 `EMAIL_HOST` 和 `EMAIL_PORT` 之中。 `EMAIL_HOST_USER` 和 `EMAIL_HOST_PASSWORD` 是用来验证 SMTP 服务器的， `EMAIL_USE_TLS` 和 `EMAIL_USE_SSL` 用来确定是否建立安全连接。

# send_mail()
Django 中发邮件最简单的方法是 `django.core.mail.send_mail()`

`send_mail(subject, message, from_email, recipient_list, fail_silently=False, auth_user=None, auth_password=None, connection=None, html_message=None)`

1. `subject` 邮件主题，string
2. `message` 邮件内容，string
3. `from_email` 发件人，string
4. `recipient_list` 收件人列表，列表中的收件人都可以互相看到
5. `fail_silently` 这是一个 boolean 值。如果是 false，会抛出一个 `smtplib.SMTPException` 异常
6. `auth_user` 和 `auth_password` 是 optional 的值，如果为空，则会用 `EMAIL_HOST_USER` 和 `EMAIL_HOST_PASSWORD` 来验证 SMTP 服务器。
7. `connection` 是 optional 的值，如果不填，就用默认的 `backend` 
8. `html_message` 这个不是很懂啊
9. 这个方法的返回值是发送成功的消息的数量，因为消息数量只有一条，所有就是 0 或 1

``` python
send_mail(
	'Subject',
	'Message.',
	'from@example.com',
	['john@example.com', 'jane@example.com'],
)
```

***

# send_mass_mail()
`django.core.mail.send_mass_mail()` 是用来群发邮件的。

`send_mass_mail(datatuple, fail_silently=False, auth_user=None, auth_password=None, connection=None)`

datatuple 是一个包含多条消息的元组。每条消息的结构如此： 
`(subject, message, from_email, recipient_list)` 

举个栗子：
``` python 
message1 = ('Subject here', 'Here is the message', 'from@example.com', ['first@example.com', 'other@example.com'])
message2 = ('Another Subject', 'Here is another message', 'from@example.com', ['second@test.com'])
send_mass_mail((message1, message2), fail_silently=False)
```
上面的例子， send_mass_mail 方法只建立一个连接，发送了两条消息。该方法返回值也是发送成功的消息数量。因为 send_mass_mail 发送多条消息只建立一个连接，所以在群发邮件时这个方法比较高效。

``` python
datatuple = (
	('Subject', 'Message.', 'from@example.com', ['john@example.com']),
	('Subject', 'Message.', 'from@example.com', ['jane@example.com']),
)
send_mass_mail(datatuple)
```

***

# mail_admins()
`django.core.mail.mail_admins()` 是用来向 admin 发送邮件的。

`mail_admins(subject, message, fail_silently=False, connection=None, html_message=None)`

EMAIL_SUBJECT_PREFIX 定义了 mail_admins() 方法发送邮件的 subject 的前缀，默认是 "[Django] ".

# mail_managers()
`django.core.mail.mail_managers()` 和 mail_admins 类似，是发送邮件给 manager 的。

***

# Preventing header injection
Header injection 是一种修改邮件头 “To:” and “From:” 的攻击。前面的这些方法都可以预防 Header Injection 攻击，如果邮件头部有修改，则会抛出 `django.core.mail.BadHeaderError` 异常。

``` python
from django.core.mail import send_mail, BadHeaderError
from django.http import HttpResponse, HttpResponseRedirect

def send_email(request):
	subject = request.POST.get('subject', '')
	message = request.POST.get('message', '')
	from_email = request.POST.get('from_email', '')
	if subject and message and from_email:
		try:
			send_mail(subject, message, from_email, ['admin@example.com'])
		except BadHeaderError:
			return HttpResponse('Invalid header found.')
		return HttpResponseRedirect('/contact/thanks/')
	else:
		# In reality we'd use a form class
		# to get proper validation errors.
		return HttpResponse('Make sure all fields are entered and valid.')
```


