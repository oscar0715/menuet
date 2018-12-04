---
title: Materialize CSS
tags:
  - Materialize
categories:
  - Techonology
  - Materialize
date: 2016-12-26 16:15:58
---
学习一下 Materialize CSS

<!-- more -->

***

# 颜色 Color
Materialize 提供了一系列常用的颜色，这些颜色是根据路标，便利贴这些日常生活中的东西设计的

red / pink / purple / deep-purple / indigo / blue / light-blue / cyan / teal / green / light-green / lime / yellow / amber / orange / deep-orange / brown / grey / blue-grey / black / white / transparent

另外每一种颜色还有相应的浓淡：

lighten-1 / lighten-2 / lighten-3 / lighten-4 / lighten-5 / darken-1 / darken-2 / darken-3 / darken-4 / accent-1 / accent-2 / accent-3 / accent-4

# 栅格 Grid
和 bootstrap 类似， Materiize 也有 12 列的栅格结构。通过 `raw` 和 `col` 的 `style` 来定义行和列:

Materialize 针对不同大小的屏幕也有不同的标签
小屏 s1 - s12 (手机屏幕)
中屏 m1 - m12 (平板电脑)
大屏 l1 - l12 (笔记本电脑)

``` html
<div class="row">
   <div class="col s2 m4 l3">
	  <p>This text will use 2 columns on a small screen, 4 on a medium screen, and 3 on a large screen.</p>
   </div>
</div>

```

# 辅助类 Helpers
Materialize 提供了一些非常有用的小工具：
1. 颜色工具
	Color utility classes - Examples: .red, .green, .grey and so on
2. 排列工具
	Alignment utility classes - Examples: .valign-wrapper, .left-align, .right-align, .center-align, .left, .right
3. 隐藏工具，针对不同大小的设备可以隐藏一些组件
	Hiding Content utility classes as per device size - Examples: .hide, .hide-on-small-only, .hide-on-med-only, .hide-on-med-and-down, .hide-on-med-and-up and .hide-on-large-only
4. 格式工具
	Formatting utility classes - Examples: truncate, hoverable
	truncate: 不换行，设备大小不够的话，文字就用省略号省略
	hoverable: 这个鼠标悬停效果好看的！

# 多媒体 Media
Material 也提供了能够使图片和视频根据设备大小而自动调整大小的响应式设计

1. responsive-img - It makes an image to resize itself based on screen size.
2. video-container - For responsive container having embedded videos
3. responsive-video - To make HTML5 videos responsive.

# 阴影 Shadows 
Materialize 提供了能把 container 展示成卡片纸的阴影的类。比如可以加在 div 外面。

1. z-depth-0
Remove the shadow of elements having z-depth by default.
2. z-depth-1
Styles a container for any HTML content with 1px bordered shadow. Adds z-depth of 1.
3. z-depth-2
Styles a container for any HTML content with 2px bordered shadow. Adds z-depth of 2.
4. z-depth-3
Styles a container for any HTML content with 3px bordered shadow. Adds z-depth of 3.
5. z-depth-4
Styles a container for any HTML content with 4px bordered shadow. Adds z-depth of 4.
6. z-depth-5
Styles a container for any HTML content with 5px bordered shadow. Adds z-depth of 5.

# 表格 Table
Materialize 还可以画好看的表格

1. None
	Represents a basic table without any border.
2. stripped
	Displays a stripped table.
3. bordered
	Draws a table with a border across rows.
4. highlight
	Draws a highlighted table.
5. centered
	Draws a table with all the text center aligned in the table.
6. responsive-table
	Draws a responsive table to show a horizontal scrollbar, if the screen is too small to display the content.

# 字体 Typography
Materialize 的默认字体是 Robot 2.0. 可以用 css 覆盖

html {
   font-family: GillSans, Calibri, Trebuchet, sans-serif;
}

# 标记 Badges
1. badge
Identifies element as an MDL badge component. Required for span element.
2. new
Adding a "new" class to a badge component gives it a background. 要和 badge 连在一起用

# 按钮 button
Materialize 提供了不同的按钮样式

1. btn
Sets button or anchor as an Materialize button, required.Sets raised display effect to a button.
2. btn-floating
Creates a circular button.  这个很奇怪为啥 float 是圆形
3. btn-flat
Sets flat display effect to button, default.
4. btn-large
Creates large buttons.
4. disabled
Creates disabled button.
5. type="submit"
Represents an anchor or button as primary button.
6. waves-effect
Sets ripple click effect, can be used in combination with any other classes.

# 面包屑导航 BreadCrumb
不太喜欢面包屑导航。一层一层的，不好用。

``` html
<nav>
<div class="nav-wrapper">
	<div class="col s12">
		<a href="#" class="breadcrumb">Home</a>
		<a href="#" class="breadcrumb">Technology</a>
		<a href="#" class="breadcrumb">HTML5</a>
	</div>
 </div>
</nav>
```


# 卡片 Cards
卡片的装饰嘛，放在 div 上

# Chips 

类似于可以用来展示 tag 的？

``` html
<div class="chip">           
	<img alt="HTML5" src="html5-mini-logo.jpg">HTML 5            
</div>
<div class="chip">           
	HTML 5<i class="material-icons">close</i>
</div>		
```

# Collection
用来展示一个列表嘛

# form
Materialize 的 form 好看的！
不在这里详述了。

# Bug 

## Select 无法渲染
Materialize 的 Select 类出现无法渲染的情况，需要覆盖 js
[stackoverflow](http://stackoverflow.com/questions/28258106/materialize-css-select-doesnt-seem-to-render)

然后 ajax 改了 select 以后也很麻烦，具体如下
[How to dynamically modify <select> in materialize css framework](http://stackoverflow.com/questions/29132125/how-to-dynamically-modify-select-in-materialize-css-framework)

## Select 选项无效
The select didn't work for me with these files:
这样是无效的
```
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.0.0-alpha1/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/materialize/0.97.5/js/materialize.min.js"></script>
```

到官网上找最新的最靠谱


***
