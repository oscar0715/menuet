---
title: Hexo Tag Plugins
date: 2016/05/12 15:14:21

tags:
- Tool
- Hexo

categories:
- Technology
- Tool

---
关于Hexo Tags Plugin的介绍。
参照{% link Hexo官网 https://hexo.io/docs/tag-plugins.html hexo官网 %}
此文主要给自己当模板用

<!-- more -->

***

## 引用格式
{% blockquote [author[, source]] [link] [source_link_title] %}
content
{% endblockquote %}

### 举个栗子 包括作者和书
{% blockquote 王尔德,道连格雷的画像 %}
只有肤浅的人才不以貌取人
{% endblockquote %}

### 另一个栗子 啥都没有
没有作者的情况
{% blockquote %}
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque hendrerit lacus ut purus iaculis feugiat. Sed nec tempor elit, quis aliquam neque. Curabitur sed diam eget dolor fermentum semper at eu lorem.
{% endblockquote %}

### Pull Quote 
和引用长得一样啊
{% pullquote [class] %}
content
{% endpullquote %}

## 代码块 
### 格式
{% codeblock [title] [lang:language] [url] [link text] %}
code snippet
{% endcodeblock %}

### 第一个栗子
{% codeblock %}
alert('Hello World!');
{% endcodeblock %}

### 第二个栗子 
指定语言的
{% codeblock lang:objc %}
[rectangle setX: 10 y: 10 width: 20 height: 20];
{% endcodeblock %}

### 第三个例子
代码段前附加说明–
{% codeblock Array.map %}
array.map(callback[, thisArg])
{% endcodeblock %}

## Include Code
插入 source 文件夹内的代码文件。
{% include_code [title] [lang:language] path/to/file %}

## markdown 语法写表格
| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |



## image
{% asset_path slug %}
{% asset_img slug [title] %}
{% asset_link slug [title] %}












