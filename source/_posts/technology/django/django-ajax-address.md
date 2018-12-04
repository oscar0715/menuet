---
title: Django 前端的地址四级联动 - Ajax 实现
tags:
	- Python
	- Django
categories:
	- Techonology
	- Python
date: 2017-01-09 05:15:58
---
国家-省-市-区
地址四级联动效果在 Django 中的 Ajax 实现

<!-- more -->

***

# 综述
在选择地址时，需要有动态的三级联动效果，[这里有大神做了一个Demo](http://jquerywidget.com/jquery-citys/)

我的项目里要实现，国家，省，市，区的四级联动，原理是一样的，只不过多一层而已。

这里说明实现国家-省的联动。选择中国时，显示中国所有的省，选择别的国家时，则显示国家名

# Model 
首先在数据库中存有地址信息，有两个表。一个是国家表（ Country ），一个是中国的地区信息表 （ District ）

``` python
class District(models.Model):
	code = models.IntegerField()
	name = models.CharField(max_length = 50)

class Country(models.Model):
	name = models.CharField(max_length = 50)
	code = models.IntegerField()    
```

主要用到的 field 是 `name` 和 `code`

# Form
``` python 
# form.py

# default country list
countries = Country.objects.all()
COUNTRY_CHOICES = []
## add a empty choice
COUNTRY_CHOICES.append(["00","-------"])
for country in countries:
	COUNTRY_CHOICES.append([country.code,country.name])

# default province list
provinces = District.objects.filter(code__regex = r'(0){4}$')
PROVINCE_CHOICES = []
## add a empty choice
PROVINCE_CHOICES.append(["0000000","-------"])
for province in provinces:
	PROVINCE_CHOICES.append([province.code,province.name])

class yourForm(forms.Form):
	post_country = forms.ChoiceField(
		widget =forms.Select(
			attrs={
				'class':'select',
				# 用于触发 Ajax 的函数
				'onChange':'getProvince(this.value)'
			}
		),
		choices = COUNTRY_CHOICES,
		label = u'选择国家',
		initial='86'
	)
	post_province = forms.ChoiceField(
		widget =forms.Select(
			attrs={
				'class':'select',
				# 这里是联动选择市，本文暂时使用不到
				'onChange':'getCity(this.value)'
			}
		),
		choices = PROVINCE_CHOICES,
		label = u'选择省',
		initial='0000000'
	)
```

国家列表是后台直接准备好的，前端显示即可，设定默认值为 86 ，即中国的国家码
默认的省列表也是后台准备好的，默认值是所有中国的市。

# View
view.py 中准备用于被 Ajax 调用的函数
``` python 
# 这里省略了渲染 html 的 view 函数需要您自己提供

# 这个是 Ajax 会用到的函数
def getProvinceList(request):
	provinceList = []
	
	# 从 Request 中获取国家码
	countryCode = request.GET.get('countryCode', None)

	# 判断如果国家码是 '86' 
	# 注意这里从 Request 中取到的是字符串
	if countryCode == '86':
		# get all the provinces
		# 正则表达式获取数据
		# 这里和数据库中数据的形式有关，各自的项目应该会有所不同
		provinces = District.objects.filter(code__regex = r'(0){4}$')

		# append to the list
		for province in provinces:
			c = {}
			c['name'] = province.name
			c['code'] = province.code
			provinceList.append(c)
	else:
		province = Country.objects.get(code = countryCode)
		c = {}
		c['name'] = province.name
		c['code'] = province.code
		provinceList.append(c)
	
	# https://rayed.com/wordpress/?p=1508
	# 返回一个 json 列表
	return HttpResponse(json.dumps(provinceList),content_type = "application/json;charset=utf-8")

```

# Template
``` html
<!-- template 中只要很简单的地用 Django 的 form 就好了 -->
<!-- 还有要引用 JQuery -->

<form action='' method='POST'>
	{{ form.as_p }}
	<div class='fieldWrapper'> <p><input type='submit' value='提交'></p></div>
</form>
```

# Ajax
``` js

// 这个函数是 Materialize CSS 渲染 Select 用的
$(document).ready(function() {
	$('select').material_select();
});

// 这个函数是改变 Select 内容时，提醒 Materialize 做出相应改版用的 
$('select').on('contentChanged', function() {
	// re-initialize (update)
	$(this).material_select();
  });

<!-- Ajax 函数 -->
function getProvince(countryCode){  
	$.ajax({   
		type: "GET",
		url: "/posts/getProvinceList?countryCode="+countryCode,       
		dataType:'json',   
		success: function(data,textStatus){
			// 找到 Select 元素
			var provinceSelect = document.getElementById("id_post_province");
			// <!-- 这句话是用来清空原来的选项的，猜的 -->
			for ( var i=provinceSelect.options.length-1; i>-1; i--){   
				provinceSelect[i] = null;   
			}
			// 把 response 中的Data 包含的 json 取出来，放到 option 中
			if(data.length > 0) {
				// $("#id_post_province").show();  
				for ( i=0; i<data.length; i++ ) {   
					provinceSelect.options[i] = new Option();   
					provinceSelect.options[i].text = data[i].name;   
					provinceSelect.options[i].value = data[i].code; 
				}
				// trigger event 来提醒 Materialize
				$("#id_post_province").trigger('contentChanged');
			}else
				$("#id_post_province").hide();  
			  
		}    
	})   
}  

```

# URL
记得要在 url.py 中添加出发 Ajax 请求的 url

# Reference
1. [How to Work With AJAX Request With Django](https://simpleisbetterthancomplex.com/tutorial/2016/08/29/how-to-work-with-ajax-request-with-django.html)
2. [django+jquery 实现级联选择菜单](http://sarlmolapple.is-programmer.com/posts/25844.html)
3. [Django Web实现动态三级联动](http://xpleaf.blog.51cto.com/9315560/1716482)
4. [Python的Django框架中forms表单类的使用方法详解](http://www.jb51.net/article/87046.htm)


***
Over ！