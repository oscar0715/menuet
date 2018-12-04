---
title: Codecademy JQuery
tags:
  - Javascript
categories:
  - Technology
  - Web
date: 2017-01-10 08:50:25
---
Codecademy

Jquery
<!-- more -->

***

# Introduction
The Document Object Model (DOM)
HTML由很多个元素组成，这些元素由组成了一个树状结构

JQuery 是用 JavaScript 写的一个库

``` html
把 js 文件引入到 html 文件中
<script type="text/javascript" src="script.js"></script>
```

``` js
// $() 是 jQuery 用来选取元素的符号
// $(document) 就是把 document 转变为一个 jQuery 对象
// .ready() 是一个方法，在 document ready 的时候启用，
// 然后执行 () 内的语句
// 值得注意的是 因为 document 是一个特殊元素，所以不用引号
// 这里要执行的是一个 function ， 可以理解成一个 action
$(document).ready(function() {
	// 选中所有的 div 
	$('div').fadeOut(1000);

	// 选中 id 为 green 的元素
	// # 用来表示 id 
	// 要带单引号
	$('#green').fadeOut(1000);
});
```

``` js
// 这段代码是通过把鼠标移到 div 上，来改变 div 的透明度
// 鼠标移开以后也会改变
$(document).ready(function() {
	$('div').mouseenter(function() {
		$('div').fadeTo('fast', 1);
	});
	
	$('div').mouseleave(function() {
		$('div').fadeTo('fast', 0.5);
	});
});

```

<br>
***
<br>

# jQuery Functions
举个例子，要隐藏最后一个 li
``` html 
<li>
	<ol>
		<li>Start with the function keyword</li>
		<li>Inputs go between ()</li>
		<li>Actions go between {}</li>
		<li>jQuery is for chumps!</li>
	</ol>
</li>
```


``` js
// $('') 引号里面写的是 css 的 selector
$(document).ready(function() {
	$target = $('li:last-child')
	$target.fadeOut('fast');
});
```

***

再举一个例子
``` html
<div class="vanish"></div>
<div class="vanish"></div>
<div class="vanish"></div>
<div class="vanish"></div>
<br/><button>Click Me!</button>
```

``` js
$(document).ready(function() {
	$('button').click(function() {
		// .vanish 选中 vanish class
		// 区别于 # 用来选 id 
		$('.vanish').fadeOut('slow');
	});
});

// 另外,可以同时选择多个，比如：
$('.red, .pink').fadeTo('slow',0);
```


***

this 是一个很重要的
``` js
$(document).ready(function() {
	$('div').click(function() {
		$(this).fadeOut('slow');
	});
});
```

<br>
***
<br>

# Dynamic HTML
jQuery 可以创建新的 HTML 元素，我们直接把整个 HTML 元素放到 $() 里面。
例如创建一个 h1 元素：
``` js
$h1 = $("<h1>Hello<\h1>");
```

接下来要把这个新建的元素添加到 DOM 中，有以下四类方法：
一、`after()` 和 `before()` 方法的区别
1. `after()` ——其方法是将方法里面的参数添加到 jquery 对象后面去；
	如：`A.after(B)` 的意思是将 B 放到 A 后面去；
2. `before()`——其方法是将方法里面的参数添加到 jquery 对象前面去。
	如：`A.before(B)` 的意思是将 A 放到 B 前面去； 
3. 他们也可以移动现有的元素

二、`append()` 和 `appendTo()`方法的区别
1. `append()` ——其方法是将方法里面的参数添加到 jquery 对象中来；
	如：`A.append(B)`的意思是将 B 放到 A 中来，后面追加, A 的子元素的最后一个位置；
2. `appendTo()` ——其方法是将jquery对象添加到 appendTo 指定的参数中去。
	如：`A.appendTo(B)`的意思是将 A 放到 B 中去，后面追加, B 的子元素的最后一个位置；

三、`prepend()` 和 `prependTo()`方法的区别
1. `append()` ——其方法是将方法里面的参数添加到 jquery 对象中来；
	如：`A.append(B)` 的意思是将B放到A中来，插入到A的子元素的第一个位置；
2. `appendTo()` ——其方法是将jquery对象添加到appendTo指定的参数中去。
	如：`A.appendTo(B)`的意思是将A放到B中去，插入到B的子元素的第一个位置；
 
四、`insertAfter()` 和 `insertBefore()`的方法的区别
1. 其实是将元素对调位置；
	可以是页面上已有元素；也可以是动态添加进来的元素。
	如：`A.insertAfter(B);` 即将A元素调换到B元素后面；
	如 `<span>CC</span><p>HELLO</p>` 使用 `$("span").insertAfter($("p"))` 后，就变为 `<p>HELLO</p><span>CC</span>` 了。两者位置调换了
 


例如要在 `.info` 这个 class 前后添加 paragraph 元素
``` js
// 这两个效果相同
// 注意方法括号内是不需要 $() 的
$(".info").append("<p>Stuff!</p>");
$('<p>Stuff!</p>').appendTo('.info');

$(".info").prepend("<p>Stuff!</p>");
```

****

移除元素 remove()
``` js
// 直接 remove 就好了
.remove($('p'));
```

``` js
// 点击某个 .item class 的元素的时候，把自己remove掉
$(document).ready(function(){
	
	$(document).on('click', '.item', function() {
		$(this).remove();
	});
});
```


***

添加和移除 classes
``` js
$('selector').addClass('className');
$('selector').removeClass('className');

// class 本来有了就remove，没有就 add
$('selector').toggleClass('className');
```

***

Changing Your Style，改变 css

``` js
// 通常来讲这样写
$("div").css("background-color","#008800");

// height 和 width 太通用了，所以直接提供了函数
$("div").height("100px");
$("div").width("50px");
```

***

改变 HTML 元素的内容
``` js
// 改变第一个 div 的内容
$('div').html("I love jQuery!");

// val() 函数可以获取 form 元素的值
$('input:checkbox:checked').val();
```

<br>
***
<br>

# jQuery Events

1. .click()
2. .dblclick()
3. .hover()
	``` js
	// hover 函数允许有了两个 function
	// 第一个在鼠标移上去的的时候实现
	// 第二个则在鼠标移开的时候实现
	$(document).ready(function(){

	  $('div').hover(
		function(){
			$(this).addClass('active');
		},
		function(){
			$(this).removeClass('active');
		}
	  );

	});
	```
4. focus()
	``` js
	// form 中鼠标 focus 的时候改变输入框的css 
	$(document).ready(function(){

		$('input').focus(function(){
			$(this).css('outline-color','#FF0000');
		});

	});
	```
5. .keydown() 按下键盘的时候触发
	``` js
	// key 参数指的是按下的那个建
	$(document).keydown(function(key){});
	```
6. .animate() 动画效果
	``` js
	// 对 document 元素调用 keydown() 方法
	// 触发后，对 div 元素进行右移，时间为 500
	$(document).ready(function(){

		$(document).keydown(function(){
			$('div').animate({left:'+=10px'},500);
		});

	});

	```

<br>
***
<br>

# jQuery Effects

接下来介绍 jQuery UI
``` html
<!-- 引入 library -->
<!-- <script src="//ajax.googleapis.com/ajax/libs/jqueryui/1.9.1/jquery-ui.min.js"> -->
```

``` js
// 爆炸效果
$(document).ready(function(){
	$('div').click(function(){
		// 爆炸
		$(this).effect('explode')；
		// 在500毫秒内跳3次
		$(this).effect('bounce', {times:3}, 500);
		// 划入效果
		$(this).effect('slide');
	});
});
```

``` js
$(document).ready(function(){
	// 使得 id 为 car 的元素可以被鼠标拖动
	$('#car').draggable();

	// 自适应大小，自动填充
	$('div').resizable();

});

```

``` js
$(document).ready(function(){
	// 作用在 list 上（ol 或者 ul），使得 <li> 选中后会有效果
	// 配合 css 一起使用
	$('ol').selectable();

	// 可以拖动排序
	// 和 selectable 不能一起使用Orz
	$('ol').sortable();

});
```

下面来做一个下拉通告栏效果
``` html
<div id="menu">
	<h3>Section 1</h3>
	<div>
		<p>I'm the first section!</p>
	</div>
	<!--Add two more sections below!-->
	<h3>Section 2</h3>
	<div>
		<p>I'm the second section!</p>
	</div>
	
	<h3>Section3</h3>
	<div>
		<p>I'm the third section!</p>
	</div>
</div>
```

``` js
$(document).ready(function(){
    $('#menu').accordion()
});
```

***

哈哈 上完了！