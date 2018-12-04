---
title: Hexo问题总结
tags:
  - Tool
  - Hexo
categories:
  - Technology
  - Tool
date: 2016-05-14 13:32:29
---
报错问题 
1. Hexo s 报错解决方案

Tips
1. 网站图标设置 favicon
2. 小图标设置
3. 主页添加menu选项

<!-- more -->

***

## 报错

### Hexo s -g报错

``` 
OscarMacBook:Hexo oscar$ hexo s -g
INFO  Start processing
ERROR Process failed: layout/.DS_Store
TypeError: Cannot read property 'compile' of undefined
    省略一堆乱七八糟的
```

解决方法
>cd 进到你使用的theme对应的目录，再进到layout/和layout/_partial/下.
分别执行rm .DS_Store

### 站点设置文件中的 `auto_detect` 会出现很多问题
讲其设置为 `auto_detect : false`

## 小伎俩 tips
### 页面显示文章数量控制
在hexo根目录下安装插件
``` sh
npm install hexo-generator-archive --save
npm install hexo-generator-index --save
npm install hexo-generator-tag --save
```

在站点配置文件_config.yml文件中添加
``` yaml
index_generator:
  per_page: 5

archive_generator:
  per_page: 0  //为0时表示不分页全展示
  yearly: true  //按年生成归档
  monthly: true //按月生成归档

tag_generator:
  per_page: 10
```

参考 
{% link Hexo程序archive页面数量设置 http://www.yuzhewo.com/2015/11/21/Hexo%E7%A8%8B%E5%BA%8Farchive%E9%A1%B5%E9%9D%A2%E6%95%B0%E9%87%8F%E8%AE%BE%E7%BD%AE/ %}

### 小图标设置
1. 下载一个自己喜欢的你的网站图标（就是chrome标签栏会出现的那个图标，或者自己做一个）
*   提供一个在线转换成icon文件的{% link 地址 http://tool.lu/favicon/ icon生成 %}
2. 把文件名改成favicon.ico，放到 `hexo/source` 文件夹下
3. 主题配置文件 `/themes/{你的主题}/_config.yml` 中 设置 `favicon: /favicon.ico`
4. 小图标更新这个浏览器缓存是个坑，如果更新没成功，清除一下浏览器缓存，重新打开浏览器试试

### 主页添加menu选项
例如，我想在主页菜单栏添加 technology 和 art 两个选项
1. 在主题设置文件中添加menu项目，设置icon
``` xml
menu:
  home: /
  technology : /technology
  algorithm : /algorithm
  art : /art
  archives: /archives
  about: /about

```
2. 在主题的语言文件中添加对应的翻译
Hexo/themes/next/languages目录下，打开对应的语言文件，添加
```
// 在hexo 的 config 里面设置language，根据那个language找相应的语言文件
menu:
  home: Home
  archives: Archives
  categories: Categories
  tags: Tags
  about: About
  search: Search
  technology: Technology
  art : Art
```

3. 修改菜单栏选项图标
``` yaml
menu_icons:
  enable: true
  #KeyMapsToMenuItemKey: NameOfTheIconFromFontAwesome
  #对应图标名称对照网址 http://fontawesome.dashgame.com/
  home: home
  about: user
  categories: th
  tags: tags
  archives: archive
  commonweal: heartbeat
  technology: laptop
  art: book
  algorithm: file-code-o
```

4. 在Hexo/sources文件夹下
添加名为technology的文件夹
在文件夹中添加index.md文件
文件头为
``` yaml
---
title: technology
date: 2016-05-12 22:20:55
comments: false
---
```

[参考](http://blog.junyu.io/posts/0005-next-theme-settings.html)

### next mist 副标题不显示
```
# /themes/next/source/css/_schemes/Mist/_logo.styl
.site-subtitle { display: true; }
```
但是好丑啊

### post 生成路径
``` yaml
url: http://menuet.xyz
root: /
permalink: :title/
permalink_defaults:
```
[hexo url](https://github.com/hexojs/hexo/issues/125)
***

### 重新安装 hexo 
``` sh
$ npm uninstall -g hexo-cli
$ npm install -g hexo-cli
$ rm -rf node_modules
$ npm install 
```


## Hexo 写 Latex

1. $r\_{ui}$ 短横要加反斜杠转义