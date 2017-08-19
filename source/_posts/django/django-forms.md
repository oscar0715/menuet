---
title: Django forms 学习
tags:
	- Python
	- Django
categories:
	- Techonology
	- Django
date: 2016-12-27 16:15:58
---
Django forms 学习

<!-- more -->

***

[参考：Django Document - Working with forms](https://docs.djangoproject.com/en/1.10/topics/forms/)

***

# Forms in Django
在 html 中的表单 (form) 是用来接受用户的输入的 (input)。提交表单是一项很复杂的工作。Django 提供的表单功能简化和自动完成了大量的工作。

Django 可以完成三项涉及表单的不同的工作：
1. 为 rendering 准备好数据
2. 给数据创造一个 html 表格
3. 接受用户提交的表单

Django 的 form 的含义可以包括 html 中的 form 标签，也可以包括创造这些 html 表单的 Form，也可以包括用户提交的结构化数据。

***

# The Django Form class
整个 Django Form 系统的核心是 Form 类。Model 类用来表示对象的逻辑，相对的，form 类则用来表示一个表单的结构和工作方式。Model 类对应数据库，而 Form 类则对应 HTML 表单。

# Instantiating, processing, and rendering forms

Django 在 render 一个对象的时候，要做这些事情
1. 在 View 中把对象从数据库里面取出来
2. 把这个对象传到 template 里面
3. 用 template 的变量把对象渲染成 html 标记

Render 一个 Form 的过程是类似的。不同之处是 render 的对象总是有数据的，但是 form 可能是没数据的（我们要求用户填数据的时候），另外的情况就是我们先取出已有的数据，比如：
1. 从 model 中取出数据
2. 从其他地方也能取来数据
3. 也有可能从之前用户提交的 HTML 中提取数据

***

# Building a form

用一个例子来理解 Django 的 form 

假如我们要创建一个提交用户名的表单。在template里我们会这样写：
``` html
<form action="/your-name/" method="post">
	<label for="your_name">Your name: </label>
	<input id="your_name" type="text" name="your_name" value="{{ current_name }}">
	<input type="submit" value="OK">
</form>
```

但是这只是一个很简单的情况，如果有几十几百个变量要填写，有些可能要预加载一些数据，有些还要做验证，我们就可以利用 Django 来帮我们做很多工作

创建一个 forms.py 文件，创建一个 NameForm 类
``` python
# forms.py
from django import forms

class NameForm(forms.Form):
	your_name = forms.CharField(label='Your name', max_length=100)
	# label='Your name' 对应的是 html 的 label
	# max_length=100 这里有两层意思：
	# 第一是浏览器会阻止用户出入超过这个长度的文本，
	# 第二在 django 收到数据的时候也会做 validation
```

一个 Form 实例，有一个 `is_valid` 方法，会检查所有的 fields 是否合法，如果都合法，那么这个方法返回 true， 然后把表单信息放到 `cleaned_data` 属性里面。

这个 form 类渲染完以后就是这样：

``` html 
<label for="your_name">Your name: </label>
<input id="your_name" type="text" name="your_name" maxlength="100" required />
```

Form 类不会 render `<form>` 标签和提交按钮，这些都要自己写

用户提交的 Form 类中的数据在传回 Django 以后需要 view 来处理，通常是发布这个 form 的 view，这样可以复用一些相同的逻辑。

``` python 
# views.py

from django.shortcuts import render
from django.http import HttpResponseRedirect

from .forms import NameForm

def get_name(request):
	# if this is a POST request we need to process the form data
	if request.method == 'POST':
		# create a form instance and populate it with data from the request:
		form = NameForm(request.POST)
		# check whether it's valid:
		if form.is_valid():
			# process the data in form.cleaned_data as required
			# ...
			# redirect to a new URL:
			return HttpResponseRedirect('/thanks/')

	# if a GET (or any other method) we'll create a blank form
	else:
		form = NameForm()

	return render(request, 'name.html', {'form': form})
```

这段代码很清楚，if 成立的情况下，我们收到一个 Post 请求，那么我们就处理表单，不成立的时候则是一个 Get 请求，新建一个空表单，然后 render。

`form = NameForm(request.POST)` 这句话的意思是把收到的数据和 Form 类进行绑定。

有了 form 和 view 以后，我们在template 里面要做的事情就不多了:

``` html 
<!-- name.html -->

<form action="/your-name/" method="post">
	{% csrf_token %}
	{{ form }}
	<input type="submit" value="Submit" />
</form>
```

***

# More about Django Form classes

所有的 form 类都是 django.forms.Form 的子类。

>如果只是单纯地添加或者修改一个， ModelForm 会是一个节约时间的好办法
[ModelForm Documents](https://docs.djangoproject.com/en/1.10/topics/forms/modelforms/)
PS: 激动！找到了我要找的东西！！

区分两个定义：Bound and unbound form instances
- unbound form: 需要用户填写的空表单
- bound form: 是和用户提交的数据绑定好的表单

# Working with form templates

在 template 中调用我们写好的 form 类，只需要 {% raw %}{{ form }}{% endraw %} ，Django 就会帮我们渲染好 `<label>` 和 `<input>`

也可以把这个 form 渲染成别的样子，表格或者列表：
There are other output options though for the `<label>/<input>` pairs:
1. {{ form.as_table }} will render them as table cells wrapped in <tr> tags
2. {{ form.as_p }} will render them wrapped in <p> tags
3. {{ form.as_ul }} will render them wrapped in <li> tags

# Rendering fields manually

也可以不用 Django 提供的 form 

***

# Creating forms from models

我们的表单通常和数据库中的对象是对应的，所有我们没有必要重新定义一遍表单，直接从 model 里面创建一个表单就好了.

# The save() method

***

# Tips

## 让 form.non_field_errors 变好看一点
先让它变成一个list
[Formatting django form.non_field_errors in a template](http://stackoverflow.com/questions/15451869/formatting-django-form-non-field-errors-in-a-template)
``` python 
{% for error in form.non_field_errors %}
    {{error}}
{% endfor %}
```

然后再想办法现在还没有更好的办法。

## Choices 显示具体内容
``` python
{{ person.get_gender_display }}
```

## RadioSelect 去掉------选项
Set blank=False (or just remove it) and also add default='Unspecified'

http://stackoverflow.com/questions/5824037/how-to-get-rid-of-the-bogus-choice-generated-by-radioselect-of-django-form

***
