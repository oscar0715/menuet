---
title: Codecademy Angular JS
tags:
  - Javascript
categories:
  - Technology
  - Web
date: 2016-10-16 01:27:25
---
Codecademy

Angular JS
<!-- more -->

***
# Introduction

## A small project
1. Define a module in `app.js`
{% codeblock lang:javascript %}
var app = angular.module("myApp", []);
{% endcodeblock %}

2. In `index.html` 
{% codeblock lang:html %}
<!-- ng-app is called a directive -->
<!-- It tells AngularJS that the myApp module will live within the <body> element -->
<!-- define the application scope of the module -->
<body ng-app="myApp">
{% endcodeblock %}

3. In `MainController.js `
{% codeblock lang:javascript %}
// define a new controller called MainController
// we use the property title to store a string, and attach it to $scope.
app.controller('MainController', ['$scope',
function($scope) { 
 	$scope.title = 'Top Sellers in Books'; 
}]);
{% endcodeblock %}

4. In `index.html` 
{% codeblock lang:html %}
<!-- ng-controller is a directive that defines the controller scope.  -->
<!-- properties attached to $scope in MainController become available to use within `<div class="main">`. -->
<div class="main" ng-controller="MainController">
	<div class="container">
		<!-- we accessed $scope.title using {{ title }} -->
		<h1> {{ title }} </h1>
	</div>
</div>
{% endcodeblock %}

5 tips
	- `{{ product.price | currency }}`  可以选择价格符号

***
{% codeblock lang:javascript %}
{% endcodeblock %}
