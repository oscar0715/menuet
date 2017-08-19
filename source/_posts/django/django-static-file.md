---
title: Django 静态文件的管理
tags:
	- Python
	- Django
categories:
	- Techonology
	- Python
date: 2017-01-16 23:15:58
---

开一篇文章谈谈静态文件的管理

官方文档: [Managing static files](https://docs.djangoproject.com/en/dev/howto/static-files/)

<!-- more -->

***

配置就按照官方文档来



# App 中用到的 static 文件
setting 中设置

``` python
STATIC_URL = '/static/'
```

这样就可以在 app 文件夹中设置 static folder了

然后我们讨论两种情况：
1. 在 template 中引用静态 css 文件
2. 在 css 中引用静态文件

## 在 template 中引用静态 css 文件
1. 在 app 目录下创建 static 文件夹
2. 然后在 static 文件夹下创建 以 app 为名字的文件夹
3. 然后里面就可以根据自己的需要来放静态文件了，例如
    `my_app/static/my_app/css/example.css`
4. 然后在 template 中，首先 `{% raw %}{% load static %} {% endraw %}`
5. 然后用 `{% raw %} {% static 'my_app/css/example.css' %} {% endraw %}` 来引用 css 文件
6. 这个 `static/my_app` 这个文件夹就是为了和其他 app 中的 static 文件夹区分开来

## 在 css 中引用静态文件
1. 比如，我们在 example.css 需要用到背景图片
2. 图片的路径是这样的 `my_app/static/my_app/image/a.jpg`
3. 那我们 css 应该这样写
    ``` css
    body {
        background: url("../image/a.jpg") ;
    }
    ```

# 有些公共的静态文件
在base folder中建立 static 文件夹

但是 setting 文件的 STATICFILES_DIRS 数组中添加

``` python
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, "static"),
]

# BASE_DIR 已经设置过了
# BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
```

# deploy 环境下，要设置

``` python
STATIC_ROOT = os.path.join(BASE_DIR, "static_root/")
```


然后把所有 static 文件夹中的静态文件 collect 到这个 root 中，然后服务器统一到这个 root 下取静态文件。

命令如下：
```
python manage.py collectstatic
```
***
