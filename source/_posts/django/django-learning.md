---
title: Django Learning - Tutorial
tags:
	- Django
	- Python
categories:
	- Technology
	- Django
date: 2016-12-15 17:27:25
---
Django 框架的学习

[Django documentation](https://docs.djangoproject.com/en/1.10/ "Django documentation")

[How to Upload Files With Django](https://simpleisbetterthancomplex.com/tutorial/2016/08/01/how-to-upload-files-with-django.html "How to Upload Files With Django")
<!-- more -->

***
# Writing your first Django app, part 1

检查安装版本 
``` sh
$ python -m django --version 
```

***

## Creating a project

第一次使用 Django 的时候需要自动生成一些代码来创建一个项目，也就是一个 Django 项目的设置：包括了数据库的设置，Django 的选项设置和应用的选项设置。

Term 进入想要创建文件夹的目录以后，创建一个项目叫做 mysite，同时也会在你当前的文件夹下生成一个 叫做 mysite 的项目目录
``` sh
$ django-admin startproject mysite

# 执行完这条命令以后，你会看到这样一个目录结构
mysite/
	manage.py
	mysite/
		__init__.py
		settings.py
		urls.py
		wsgi.py
```

做一些解释：
1. 最外层的 mysite 目录就是一个 container，它的命名对代码没有影响
2. manage.py 文件是一个命令行工具，永安里帮助你和 Django 项目交互的。
3. mysite/ 目录实际上可以看成 python的 package.
	- __init__.py 这是一个空文件，用来告诉 Python 这个目录是一个 python 的 package
	- settings.py 用来设置 Django 项目的参数
	- urls.py 是 Django 项目的 url 参数
	- wsgi.py 是一个指向这个项目的网络服务器的指针

***

## The development server

到 mysite 目录下运行项目

``` sh
python manage.py runserver

# Starting development server at http://127.0.0.1:8000/


# 修改端口号
python manage.py runserver 8080

```

***

## Creating the Polls app

Projects vs. apps
1. Apps 是一个有具体功能的 Web application。
2. Project 是一系列 app 和 setting 的集合，一个app 可以在很多个 project 中
3. 理论上 app 的目录可以在 Python 路径的任何位置，在这个项目里面，我们就放在和 manage.py 和 mysite/ 同级的目录下

``` sh
# 切到和 manage.py 同级的目录下
python manage.py startapp polls

# 生成一个 poll 目录
polls/
	__init__.py
	admin.py
	apps.py
	migrations/
		__init__.py
	models.py
	tests.py
	views.py

```

***

## Write your first view
修改  polls/views.py 文件
``` python
# polls/views.py 
from django.http import HttpResponse


def index(request):
	return HttpResponse("Hello, world. You're at the polls index.")
```

这是一个 Django 中最简单的 view，我们还需要把这个 view 和 url 地址绑定在一起。

修改 polls/urls.py 文件

``` python
# polls/urls.py 
from django.conf.urls import url

from . import views

urlpatterns = [
	url(r'^$', views.index, name='index'),
]
```

url 包含了四个参数: regex 和 view 是 required, kwargs 和 name 是 optional
1. regex: 正则表达式
2. view: 在匹配了正则表达式以后，Django 就可以用一个 HttpRequest 找到相应的 view，并且把地址中所有的参数也都传进来
3. kwargs: 在这个教程里不会用到这个参数
4. name: 给 url 一个名字，这样就不会混淆了，这是 Django 的一个强大的功能，这样你就可以通过只修改一个文件来达到修改全局的效果



然后再修改 mysite/urls.py 文件
``` python
from django.conf.urls import include, url
from django.contrib import admin


urlpatterns = [
	url(r'^polls/', include('polls.urls')),  # include 了 polls app 下的路径
	url(r'^admin/', admin.site.urls),
]
```

然后就可以运行了
``` sh
python manage.py runserver

// Starting development server at http://127.0.0.1:8000/polls/
```


<br>
***
<br>

# Writing your first Django app, part 2

***

## Database setup
mysite/settings.py 文件是一个 Python 模块( module )，用来设置 Django 模块层次的变量。在默认情况下 Django 用的数据库是 SQLite。（ 反正要学 ios，就用 SQLite 就好了。）

首先，设置时区， TIME_ZONE。

在文件的顶部有一个保存了项目中用到的 Django 应用的数组， INSTALLED_APPS 。默认有以下应用
1. django.contrib.admin – The admin site. 管理系统
1. django.contrib.auth – An authentication system. 权限系统
1. django.contrib.contenttypes – A framework for content types. 
1. django.contrib.sessions – A session framework. 
1. django.contrib.messages – A messaging framework. 
1. django.contrib.staticfiles – A framework for managing static files.

这些应用中有些需要用到至少一个数据库表，所以我们在使用这些应用之前要建立数据库，可以通过执行下面的语句：
``` sh
python manage.py migrate

// migrate 命令用来检查 INSTALLED_APPS 这个数组，然后根据 mysite/settings.py 中的设置来创建必要的数据库
```

***

## Creating models
现在我们就可以来定义 module 了。

这个 Tutorial 是要做一个投票小应用。在项目里要创建两个 model : Question 和 choice. 一个 Question 有两个变量: question 和 publication date. 一个 Choice 也有两个变量: text of choice 和 vate tally。这些概念都是由简单的 python class 来实现的。

编辑 polls/models.py 文件

``` python 
# polls/models.py
from django.db import models 

class Question(models.Model):
	question_text = models.CharField(max_length=200)
	pub_date = models.DateTimeField('date published')
	# pub_date 就是数据库里面的列的名字
	# date published 是一个可读的名字，如果不指定，就默认是 pub_date
	# max_length=200 作用在数据库 和 参数校验

class Choice(models.Model):
	question = models.ForeignKey(Question, on_delete=models.CASCADE) # 外键
	choice_text = models.CharField(max_length=200)
	votes = models.IntegerField(default=0)
```

***

## Activating models

上个小节的 model 可以干两件事情:
1. 创建数据库 schema
2. 创建一个 python 数据库 API

现在就要把这个 model 安装到起来

我们先要在配置文件里，把这个 app 添加到项目里面：

在 polls/apps.py 文件里的 PollsConfig 类就是这个 app 的reference，路径是 'polls.apps.PollsConfig'

``` python
from django.apps import AppConfig

class PollsConfig(AppConfig):
	name = 'polls'
```

在 mysite/settings.py 文件中的 INSTALLED_APPS 数组中把 'polls.apps.PollsConfig' 路径添加进去：

``` python 
# mysite/settings.py

INSTALLED_APPS = [
	'polls.apps.PollsConfig',      # 加载这里
	'django.contrib.admin',
	'django.contrib.auth',
	'django.contrib.contenttypes',
	'django.contrib.sessions',
	'django.contrib.messages',
	'django.contrib.staticfiles',
]
```

通过运行下面的命令来让 Django 执行这次路径的添加

``` sh 
python manage.py makemigrations polls
```

makemigrations 命令是告诉 Django 我们对 model 做了一些改变，并且需要把这些改变保存到 `migration` 里面。 `migration` 是 Django 用来保存 model (也就是数据库 schema) 的改变的。记录在 `polls/migrations/0001_initial.py` 文件中。

下面这条命令可以用来查看第 0001 号 migration 执行了什么 SQL 语句
``` sh 
python manage.py sqlmigrate polls 0001
```

得到的输出如下：
``` sql
BEGIN;
--
-- Create model Choice
--
CREATE TABLE "polls_choice" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "choice_text" varchar(200) NOT NULL, "votes" integer NOT NULL);
--
-- Create model Question
--
CREATE TABLE "polls_question" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "question_text" varchar(200) NOT NULL, "pub_date" datetime NOT NULL);
--
-- Add field question to choice
--
ALTER TABLE "polls_choice" RENAME TO "polls_choice__old";

CREATE TABLE "polls_choice" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "choice_text" varchar(200) NOT NULL, "votes" integer NOT NULL, "question_id" integer NOT NULL REFERENCES "polls_question" ("id"));

INSERT INTO "polls_choice" ("question_id", "choice_text", "votes", "id") SELECT NULL, "choice_text", "votes", "id" FROM "polls_choice__old";

DROP TABLE "polls_choice__old";

CREATE INDEX "polls_choice_7aa0f6ee" ON "polls_choice" ("question_id");
COMMIT;
```

有几点要注意：
1. 输出的 sql 和选择的数据库有关
2. 表的名字是 app 名字 + 类的名字，小写
3. 自动用 `id` 作为主键
4. `question_id` 是一个外键，自动加上了 `_id` 

check 命令是，在不用对数据库执行命令的情况下，用来查看项目中是不是有问题的
``` sh
python manage.py check
```

所以在此之前，我们都没有真正对数据库做出改动，只是生成了一些语句。

运行 migrate 命令，来真正在数据库中创建表。migrate 命令在数据库中执行所有在 migration 中的记录
``` sh
python manage.py migrate
```

总结起来对 model 的修改有以下步骤
1. 对 model 做出改动。
2. 执行 makemigrations 来生成 migration，记录这些改动。
3. 执行 migrate，最终真正把改动执行到数据库中。

***

## Playing with the API
执行 shell 命令进入 python shell，这个命令设置了 DJANGO_SETTINGS_MODULE 的环境，这里可以执行数据库 API：
``` sh
python manage.py shell
```

``` python 
>>> from polls.models import Question, Choice

>>> Question.objects.all()
<QuerySet []>
# 为空

# 新建一个Question
>>> from django.utils import timezone
>>> q = Question(question_text="What's new?", pub_date=timezone.now())
>>> q.save() # 保存到数据库

>>> q.id
1
>>> q.question_text
"What's new?"
>>> q.pub_date
datetime.datetime(2016, 12, 19, 15, 11, 6, 12798, tzinfo=<UTC>)

# objects.all() displays all the questions in the database.
>>> Question.objects.all()
<QuerySet [<Question: Question object>]>
```

在查看所有 object 的时候，出现这么一句话 `<QuerySet [<Question: Question object>]>`，这个语句提供的信息量太少了，所以我们要修改一下：在 Question 类和 Choice 类中添加 \_\_str\_\_() 函数，类似于 Java 中的 toString()。所以我们在创建对象的时候尽量记得加 \_\_str\_\_() 函数，不仅自己交互的时候方便，Django 自动管理的过程中也会用到的。

``` python 
# polls/models.py
# 加上 __str__() 函数
from django.db import models
from django.utils.encoding import python_2_unicode_compatible

@python_2_unicode_compatible  # only if you need to support Python 2
class Question(models.Model):
	# ...
	def __str__(self):
		return self.question_text

@python_2_unicode_compatible  # only if you need to support Python 2
class Choice(models.Model):
	# ...
	def __str__(self):
		return self.choice_text
```

加上 was_published_recently() 函数
``` python 
# polls/models.py
# 加上 __str__() 函数
import datetime

from django.db import models
from django.utils import timezone


class Question(models.Model):
	# ...
	def was_published_recently(self):
		return self.pub_date >= timezone.now() - datetime.timedelta(days=1)
		# d = timezone.now() - timedelta(days=days_to_subtract) 是减去一天的意思
```

上面涉及到了 timezone 管理，具体可以看这个文档：[Time Zone Support Docs](https://docs.djangoproject.com/en/1.10/topics/i18n/timezones/ "time zone support docs")

修改完了以后，重新进入shell。
Django 提供了丰富的数据库查找 API。
``` sh
>>> from polls.models import Question, Choice

# 测试 __str__() 方法是不是有用了
>>> Question.objects.all()
<QuerySet [<Question: What‘s up?>]>

>>> Question.objects.filter(id=1)   # 用 id 找
<QuerySet [<Question: What’s up?>]>
>>> Question.objects.filter(question_text__startswith='What') # 开头字母
<QuerySet [<Question: What‘s up?>]>

# Request an ID that doesn't exist, this will raise an exception.
# 如果用 ID 查找的时候没找到，就会抛出异常
>>> Question.objects.get(id=2)
Traceback (most recent call last):
	...
DoesNotExist: Question matching query does not exist.

# 用主键查找是最常用的查找方式，所以 Django 提供了主键的简写 pk 来方便查找
# pk 就是 primary key 的缩写
# The following is identical to Question.objects.get(id=1).
>>> Question.objects.get(pk=1)
<Question: What’s up?>

# Make sure our custom method worked.
>>> q = Question.objects.get(pk=1)
>>> q.was_published_recently()
True

# 现在 q 是一个 question 实例
# 有一个 Choice 的集合来匹配这个 q，叫 choice_set
# 也就是这个 q 的外键的另外一边
# choice_set 这个集合是 Django 根据外键 和 Choice 这个类名自动创建的

# Create three choices.
>>> q.choice_set.create(choice_text='Not much', votes=0)
<Choice: Not much>
>>> q.choice_set.create(choice_text='The sky', votes=0)
<Choice: The sky>
>>> c = q.choice_set.create(choice_text='Just hacking again', votes=0)

# Choice objects have API access to their related Question objects.
# 从 Choice 到 Question
>>> c.question
<Question: What‘s up?>

# And vice versa: Question objects get access to Choice objects.
# 从 Question 到 Choice
>>> q.choice_set.all()
<QuerySet [<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]>
>>> q.choice_set.count()
3

>>> Choice.objects.filter(question__pub_date__year=current_year)
<QuerySet [<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]>

# Find all Choices for any question whose pub_date is in this year
# (reusing the 'current_year' variable we created above).
# 两条下划线连在一起表示‘分隔’关系，分隔关系的层数不受限制
>>> Choice.objects.filter(question__pub_date__year=current_year)
<QuerySet [<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]>

# Let's delete one of the choices. Use delete() for that.
# 删除
>>> c = q.choice_set.filter(choice_text__startswith='Just hacking')
>>> c.delete()

```

***

## Introducing the Django Admin
创建一个管理账户，这个账户可以登录 admin site

``` sh
python manage.py createsuperuser
Username: admin # jiaming    
Email address: admin@example.com # oscarnjm@gmail.com
Password: ********** # jiamingni    
Password (again): ********* # jiamingni    
Superuser created successfully.
```


``` sh
# 启动服务器
python manage.py runserver

# 访问 http://127.0.0.1:8000/admin/ 
# 输入用户名密码以后进入管理员界面
```

目前这样是无法在管理员界面看到 poll 这个 app 的。但是我们可以在 polls/admin.py 文件里做出修改。

``` sh
# polls/admin.py 

from django.contrib import admin

from .models import Question
from .models import Choice

# Register your models here.

admin.site.register(Question)
admin.site.register(Choice)

# 修改完了以后刷新 admin site 就能看到 Question 和 Choice
```

<br>
***
<br>

# Writing your first Django app, part 3

然后我们继续完成 Web-poll 这个应用，我们现在要创建界面: view 。我们需要四个 view ：
- Question index page：展示最新的几个问题
- Question detail page：展示一个问题的 text 和一个可以投票的 form
- Question result page：展示一个问题的投票结果
- Vote action：处理对某个问题的某个选项的投票

每个 view 都是由一个 python 的 function 或者是 class 里的 method 来表现的。Django 会根据 URL 来判断调用哪个 view。Django 利用 `URLconfs` 来实现从 URL 到 view 的映射。

***

***

## Writing more views
在 polls/views.py 文件中添加 view
``` python 
# polls/views.py

def detail(request, question_id):
	return HttpResponse("You're looking at question %s." % question_id)

def results(request, question_id):
	response = "You're looking at the results of question %s."
	return HttpResponse(response % question_id)

def vote(request, question_id):
	return HttpResponse("You're voting on question %s." % question_id)    
```

在 polls/urls.py 中添加 URL 映射，用正则表达式来实现
``` python 
# polls/urls.py
from django.conf.urls import url

from . import views

urlpatterns = [
	# ex: /polls/
	# ^匹配用来查找的字符串的开头，$匹配结尾。 也就是不跟东西嘛
	url(r'^$', views.index, name='index'),
	# ex: /polls/5/
	# ? 重复零次或一次, 
	# [0-9]+ 就代表一个数字，+ 重复一次或多次
	url(r'^(?P<question_id>[0-9]+)/$', views.detail, name='detail'),
	# ex: /polls/5/results/
	url(r'^(?P<question_id>[0-9]+)/results/$', views.results, name='results'),
	# ex: /polls/5/vote/
	url(r'^(?P<question_id>[0-9]+)/vote/$', views.vote, name='vote'),
]
```

用户在浏览器中输入 `/polls/34/` 的时候，Django 先去 `mysite.urls` 文件金找 `urlpattern` 这个数组，然后按顺序去找匹配的 regex. 找到了 `^polls/` 以后就把这一段截掉，然后剩下了 `34/`
，然后到 `polls.urls` 文件里找，然后匹配到 `'^(?P<question_id>[0-9]+)/$'`，然后就根据这个去找 `detail()` 这个 view。中间会生成这样一个 request :

``` python
detail(request=<HttpRequest object>, question_id='34')
```

注意 `?P<question_id>` 定义了一个变量名，后面的数字会被当做 `question_id` 这个变量。

***

## Write views that actually do something
在 view 做出响应的时候，要么返回一个 HttpResponse，要么抛出一个异常 HTTP 404。我们可以用 Django 自己的数据库 API 来实现。例如我们要在 index 页面根绝 publication date 返回 5 个最新的问题。修改 index() 函数如下：

``` python 
# polls/views.py
from django.http import HttpResponse

from .models import Question

def index(request):
	latest_question_list = Question.objects.order_by('-pub_date')[:5]
	output = ', '.join([q.question_text for q in latest_question_list])
	return HttpResponse(output)
```

如果只是这样的话，每次要修改 view 就必须要修改 python 文件了。我们可以创建 template 来把 Python 文件和 view design 分开来。

在 polls 文件夹下创建一个叫做 templates 的文件夹。Django 会在这个文件夹中寻找 templates 。在这个文件夹下再创建一个叫做 polls 的文件夹，这里用来存 polls 这个 app 相关的 templates。然后在创建一个叫做 index.html 的文件。总之目录就是 polls/templates/polls/index.html，我们可以直接用 /polls/index.html 来找到这个文件。

``` html
# /polls/index.html
{% if latest_question_list %}
	<ul>
	{% for question in latest_question_list %}
		<li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li>
	{% endfor %}
	</ul>
{% else %}
	<p>No polls are available.</p>
{% endif %}
```

template 创建好了以后，我们就可以在 view 里面使用这个template了

``` python
from django.http import HttpResponse
from django.template import loader

from .models import Question

def index(request):
	latest_question_list = Question.objects.order_by('-pub_date')[:5]
	template = loader.get_template('polls/index.html')
	context = {
		'latest_question_list': latest_question_list,
	}
	return HttpResponse(template.render(context, request))
```

这里用到了 render() 函数，有两个参数 context 和 request，我们也可用用另外一种方式来调用 render 函数。

``` python
# A shortcut: render()¶

# 直接引用了 render 而不用引用 loader 和 HttpResponse
from django.shortcuts import render

from .models import Question


def index(request):
	latest_question_list = Question.objects.order_by('-pub_date')[:5]
	context = {'latest_question_list': latest_question_list}
	return render(request, 'polls/index.html', context)

	# 这里的 render() 有三个参数
	# request，template 的名字 和 一个 context 字典（用来传参数）
```

***

## Raising a 404 error

然后我们来处理一个 question 的 detail 页面，这个页面要展示一个问题的 question_text

``` python
from django.http import Http404
from django.shortcuts import render

from .models import Question
# ...
def detail(request, question_id):
	try:
		question = Question.objects.get(pk=question_id)
	except Question.DoesNotExist:
		raise Http404("Question does not exist")
	return render(request, 'polls/detail.html', {'question': question})
```

然后 polls/templates/polls/detail.html 文件就是 detail 这个 view 的template。

``` python
# polls/templates/polls/detail.html
{{ question }} # Django 框架下变量的调用，从 render 函数里传过来的
```

这里其实又有一个关于 Http404 的 shortcut: get_object_or_404
1. 第一个参数是一个 Django 的 model
2. 然后任意数量的关键词参数
3. 先调用 get() 函数，找不到再跑出 404 异常

``` python
polls/views.py
from django.shortcuts import get_object_or_404, render

from .models import Question
# ...
def detail(request, question_id):
	question = get_object_or_404(Question, pk=question_id)
	return render(request, 'polls/detail.html', {'question': question})
```

另外还有一个 shortcut 就是 get_list_or_404()，和 get_object_or_404 类似，只不过这里调用的是 filter() 而不是 get()。

***

## Use the template system

我们继续修改 detail.html，让它展示问题的同时也展示所有和这个 question 关联的 Choice

``` html 
<h1>{{ question.question_text }}</h1>
<ul>
{% for choice in question.choice_set.all %}
	<li>{{ choice.choice_text }}</li>
{% endfor %}
</ul>
```

template system 就是可以用点号来调用对象的属性，比如 `{{ question.question_text }}`。 `question` 是 render 函数传过来的一个 dictionary，Django 通过这个 dictionary 找到这个类以后，再去找这个类的属性

## Removing hardcoded URLs in templates
在 index.html 的 template 中有这样的代码
``` html
<li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li>
```

这里就存在 hardcode 的问题，也就是说 url couple 在了一起。但是因为我们给每一个 url 都定义了一个名字，我们就可以用 url 的名字来的解耦。

改成这样
``` html
<li><a href="{% url 'detail' question.id %}">{{ question.question_text }}</a></li>
```

然后 django 就会更具 detail 这个名字去 `polls.url` 这个文件里取找相应的 url。

***

## Namespacing URL names
现在我们只有一个 polls app，如果我们有五个，十个，甚至更多 app 的时候，Django 要怎么区分不同 app 的地址呢。这个时候我们需要在 URLconf 中添加 namespace

``` python
polls/urls.py
from django.conf.urls import url

from . import views

app_name = 'polls'  # namespace
urlpatterns = [
	url(r'^$', views.index, name='index'),
	url(r'^(?P<question_id>[0-9]+)/$', views.detail, name='detail'),
	url(r'^(?P<question_id>[0-9]+)/results/$', views.results, name='results'),
	url(r'^(?P<question_id>[0-9]+)/vote/$', views.vote, name='vote'),
]
```


然后修改 index.html template 里面根据名字来查找 url 
``` html
<!-- polls/templates/polls/index.html -->
<li><a href="{% url 'polls:detail' question.id %}">{{ question.question_text }}</a></li>
```

<br>
***
<br>

# Writing your first Django app, part 4

这个部分就要开始写 form 了

## Write a simple form

在 polls/templates/polls/detail.html 作如下修改
``` html 
# polls/templates/polls/detail.html

<h1>{{ question.question_text }}</h1>

{% if error_message %}
	<p>
		<strong>{{error_message}}</strong>
	</p>
{% endif %}

<form action="{% url 'polls:vote' question.id %}" method="post">
	{% csrf_token %}
	{% for choice in question.choice_set.all %}
		<input type="radio" name="choice" id="choice{{ forloop.counter }}" value="{{choice.id}}"> 
		<label for="choice{{ forloop.counter}}">
			{{ choice.choice_text }}
		</label><br />
	{% endfor %}
	<input type="submit" name="Vote">
</form>

```

1. 用的 post 因为要修改数据
2. submit 的时候是提交的是 `id="choice{{ forloop.counter }}"` 
3. forloop.counter 是循环次数
4. `csrf_token` 防止跨站点请求伪造 —— Cross Site Request Forgeries（scrf）
5. 我们把这个提交到 `polls:vote` 这个 url 去

所以接下来我们要修改 `polls/views.py` 文件中的 vote view

``` python
def vote(request, question_id):
	question = get_object_or_404(Question, pk=question_id)
	try:
		# 从 request 的传过来一个 choice_id
		# 这个 choice 是从 detail.html 的 form 的选项的 name 来的
		# request.POST 的值是一个 string
		selected_choice = question.choice_set.get(pk=request.POST['choice'])
	except (KeyError, Choice.DoesNotExist):
		# 如果传过来的是空值，那么就报 KeyError 的错
		return render(request, 'post/detail.html',{
			'question': question,
			'error_message': "You didn't select a choice."
		})
	else:
		selected_choice.votes += 1
		selected_choice.save()
		# Always return an HttpResponseRedirect after successfully dealing
		# with POST data. This prevents data from being posted twice if a
		# user hits the Back button.
		return HttpResponseRedirect(reverse('polls:results',args=(question.id,)))  # 这里不知道为啥多一个逗号
```

vote 结束以后，跳转到 results 的页面，修改 results view 如下

``` python
# polls/views.py

from django.shortcuts import get_object_or_404, render


def results(request, question_id):
	question = get_object_or_404(Question, pk=question_id)
	return render(request, 'polls/results.html', {'question': question})
```

然后给 results view 加一个 template ，路径是 polls/templates/polls/results.html

``` html
<!-- polls/templates/polls/results.html -->

<h1>{{ question.question_text }}</h1>

<ul>
	{% for choice in question.choice_set.all %}
		<li>
			{{ choice.choice_text }} -- {{ choice.votes }} vote {{ choice.votes |pluralize }} 
		</li>
	{% endfor %}    
</ul>

<a href="{% url 'polls:detail' question_id %}">Vote again</a>
```

***

## Use generic views: Less code is better

其实 detail 和 results 这两个 view 的代码其实是一模一样，只有跳转的时候两个 url 的名字不一样。
``` python 

def detail(request, question_id):
	question = get_object_or_404(Question, pk=question_id)
	return render(request, 'polls/detail.html', {'question': question})

def results(request, question_id):
	question = get_object_or_404(Question, pk=question_id)
	return render(request, 'polls/results.html', {'question': question})
```

为了减少冗余，Django 提供了 generic views system。

这个时候我们要做这些事情：
1. Convert the URL conf
2. Delete some of the old unneeded views
3. Introduce new views based on Django's generic view 

***

## Amend URLconf

首先，修改 polls/url.py 如下

``` python
# polls/url.py

from django.conf.urls import url

from . import views


app_name = 'polls'
urlpatterns = [
	# ex: /polls/
	# ^匹配用来查找的字符串的开头，$匹配结尾。 也就是不跟东西嘛
	# 旧代码 url(r'^$', views.index, name='index'),
	url(r'^$', views.IndexView.as_view(), name='index'),
	# ex: /polls/5/
	# ? 重复零次或一次
	# 旧代码 url(r'^(?P<question_id>[0-9]+)/$', views.detail, name='detail'),
	url(r'^(?P<pk>[0-9]+)/$', views.DetailView.as_view(), name='detail'),
	# ex: /polls/5/results/
	# 旧代码url(r'^(?P<question_id>[0-9]+)/results/$', views.results, name='results'),
	url(r'^(?P<pk>[0-9]+)/results/$', views.ResultsView.as_view(), name='results'),
	# ex: /polls/5/vote/
	url(r'^(?P<question_id>[0-9]+)/vote/$', views.vote, name='vote'),
]
```

注意到第二个和第三个 url 中正则表达式中参数从 question_id 变成了 pk

***

## Amend views

然后到 polls/views.py 文件中修改 view ，这个时候我们可以用 generic views 来代替。

``` python 
#polls/views.py

from django.shortcuts import get_object_or_404, render
from django.http import HttpResponseRedirect
from django.urls import reverse
from django.views import generic

from .models import Choice, Question

# ListView: 展示一个数据列表
class IndexView(generic.ListView):
	template_name = 'polls/index.html'
	context_object_name = 'latest_question_list'

	def get_queryset(self):
		"""Return the last five published questions."""
		return Question.objects.order_by('-pub_date')[:5]


# DetailView: 展示某个对象的详细信息
# DetailView 需要从 url 中提取这个对象的 pk
# 所以在上一节，我们把 question_id 改成了 pk
class DetailView(generic.DetailView):
	model = Question
	template_name = 'polls/detail.html'


class ResultsView(generic.DetailView):
	model = Question
	template_name = 'polls/results.html'


def vote(request, question_id):
	... # same as above, no changes needed.
```

详细信息参见 : [generic views documentation](https://docs.djangoproject.com/en/1.10/topics/class-based-views/)


<br>
***
<br>

# Writing your first Django app, part 5

天呐，这个教程居然还教写测试

***

## Introducing automated testing

### What are automated tests?

测试就是检查代码的运行情况。测试分不同的层次。有些只覆盖一部分代码，有些则覆盖整个软件的运行。自动化测试则是由系统来完成测试。

### Why you need to create tests

1. 节约时间
2. 测试也会引导程序猿向正确的方向写代码
3. 测试是代码质量的保证，也能让别人认可你的代码
4. 测试可以提高团队合作的效率

## Writing our first test

代码中有一个 bug， Question 对象有一个的方法 was_published_recently()，如果这个方法的创建时间是未来的某个时间地点，返回值也会是 True 。这显然是不对的。

我们写一个测试，放在 polls 文件下

``` python 
# polls/tests.py
import datetime

from django.utils import timezone
from django.test import TestCase

from .models import Question


class QuestionMethodTests(TestCase):

	def test_was_published_recently_with_future_question(self):
		"""
		was_published_recently() should return False for questions whose
		pub_date is in the future.
		"""
		time = timezone.now() + datetime.timedelta(days=30)
		future_question = Question(pub_date=time)
		self.assertIs(future_question.was_published_recently(), False)
```

代码中创建了一个 django.test.TestCase 的子类 QuestionMethodTests。这个类中定义了一个方法来测试创建时间在未来的问题的 was_published_recently() 的返回值是不是 False。

运行以下代码来测试的
``` sh
$ python manage.py test polls
```

运行以后我们可以看见测试失败了，说明代码是有问题的。

我们到 model.py 中把 bug 修复

```  python 
# polls/models.py

def was_published_recently(self):
	now = timezone.now()
	return now - datetime.timedelta(days=1) <= self.pub_date <= now

```

再一次运行测试就好了。

***

## Test a view 

在 view 中也有一个 bug。如果一个 question 的创建时间是在未来，应该不能再 question 列表页面显示出来，但是现在其实是可以的。

Django 提供了一个工具 test client 。我们可以在 tests.py 文件中使用，甚至可以在 shell 中使用。

详情可以看文档 [The Django test client](https://docs.djangoproject.com/en/1.10/intro/tutorial05/#the-django-test-client)

## Improving our view

我们先来解决的上一节提到的 bug 。

``` python
# polls/views.py

# 引入 timezone
from django.utils import timezone

class IndexView(generic.ListView):
	template_name = 'polls/index.html'
	context_object_name = 'latest_question_list'

	def get_queryset(self):
		"""
		Return the last five published questions (not including those set to be
		published in the future).
		"""
		return Question.objects.filter(pub_date__lte=timezone.now()).order_by('-pub_date')[:5]
```


然后再增加一些测试类。此处省略了。

一些建议：
1. a separate TestClass for each model or view
每个 model 和 view 都有独立的测试类
2. a separate test method for each set of conditions you want to test
每个条件都单独写一个方法，也就是说同一个功能可能有很多测试方法
3. test method names that describe their function
测试方法的名字要体现它的功能

<br>
***
<br>


# 一些 Tips
## Detail View 要添加额外的 field
[官方文档的方法](https://docs.djangoproject.com/en/1.10/topics/class-based-views/generic-display/#adding-extra-context)

``` python
# 在 view 中添加 get_context_data 方法 
from django.views.generic import DetailView
from books.models import Publisher, Book

class PublisherDetail(DetailView):

    model = Publisher

    def get_context_data(self, **kwargs):
        # Call the base implementation first to get a context
        context = super(PublisherDetail, self).get_context_data(**kwargs)
        # Add in a QuerySet of all the books
        context['book_list'] = Book.objects.all()
        return context
        # 在 template 里面用 {{ book_list }} 调用就好了
```

如果要获取当前用户

``` python 
# 同样在 get_context_data 方法下
def get_context_data(self, **kwargs):
	# Call the base implementation first to get a context
	context = super(DetailView, self).get_context_data(**kwargs)
	current_user = self.request.user.my_profile
```


## Form 传参数进form
[Django Forms: pass parameter to form](http://stackoverflow.com/questions/14660037/django-forms-pass-parameter-to-form)

``` python
class UserAddressSelectForm(forms.Form):
	
	# 定一个 ChioceField
	addresses = forms.ChoiceField(label="收信地址")

	def __init__(self,*args,**kwargs):
		# 在这里获取传进来的参数
		self.user = kwargs.pop('user')
		# 要写在 super 方法前面
		super(UserAddressSelectForm,self).__init__(*args,**kwargs)

		# 设置 chioce field 的
		# 还是要注意 choice 和 queryset 的区别
		# 一个是显示哪些，一个是有效范围
		# 调用的时候要用 self.user，要加上 self
		address_set = User_Address.objects.filter(user=self.user)
		self.fields["addresses"].choices = [(address.id,address.__str__()) for address in address_set]
```

``` python
# 在view 文件中，新建form的时候就这样传入 user 参数
address_form = UserAddressSelectForm(user=current_user)
```

然后在 template 中就可以正常使用 form 了

## template select 选项的长度

``` python
# 如果 form 中有个 field 叫做 addresses
# 先要访问 addresses 的 field 属性
{% if address_form.addresses.field.choices|length %}
不为零的结果
```


## generic class view 不能加 decorator

``` python 
from django.contrib.auth.decorators import login_required
from django.utils.decorators import method_decorator

# 在类里面覆盖 dispatch 方法
@method_decorator(login_required)
def dispatch(self, request, *args, **kwargs):        
	return super(DetailView, self).dispatch(request, *args, **kwargs)

# 在setting.py 设置 login url
LOGIN_URL = '/users/signin/'
```

## filter 的关键词是一个list

``` python
# 用 __in
Blog.objects.filter(pk__in=[1, 4, 7])
```



<br>
***
<br>

 




