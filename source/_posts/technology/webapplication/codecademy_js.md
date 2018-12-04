---
title: Codecademy Javascript
tags:
  - Javascript
categories:
  - Technology
  - Web
date: 2016-10-12 23:19:25
---
Codecademy

Javascript

reference
[JavaScript教程](http://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000 'JavaScript教程')
<!-- more -->

***


## Basics
1. We don't need to write semicolon is Javascript
2. String, using single quotation.


## Intro

{% codeblock lang:javascript %}
// 交互
comfirm("Helllo world")

// 需要输入的交互
prompt("What is your name")

// log to console and printing out
// console 是用来交互的最方便和最常用的方法
console.log(2 * 5)

// substring(startPosition, endPosition)
"wonderful day".substring(3,7) 
// then get derf, 不包括 endPosition

// variable
var myAge = 23

// NaN   --> not a number
// If you call isNaN on something, it checks to see if that thing is not a number. So:
isNaN('berry'); // => true
isNaN(NaN); // => true
isNaN(undefined); // => true
isNaN(42);  // => false

//  .toUpperCase() or .toLowerCase()
{% endcodeblock %}

## 数据类型和运算符
1. Number 类型
	js 不区分整数和浮点数，统一用Number
	``` js
	123; // 整数123
	0.456; // 浮点数0.456
	1.2345e3; // 科学计数法表示1.2345x1000，等同于1234.5
	-99; // 负数
	NaN; // NaN表示Not a Number，当无法计算结果时用NaN表示
	Infinity; // Infinity表示无限大，当数值超过了JavaScript的Number所能表示的最大值时，就表示为Infinity
	```
2. String 字符串
	用双引号和单引号都可以。记得以前问过一个师兄，师兄说单引号比较省力，所以用单引号。
3. bool 布尔值
4. && 与， || 或，! 非
5. 比较运算符 >, >=, ==, ===
	注意 `==`和`===`的区别
	- `==` 会自动转换数据类型，再比较
	- `===` 如果数据类型不一致，返回false
	- `1 / 3` === `(1 - 2 / 3)`; 浮点数在计算时候会有误差
6. null 和 undefined, undefined用的比较少
7. 数组
	直接方括号定义
	``` js
	var arr = [1, 2, 3.14, 'Hello', null, true];
	```
8. 对象 Object
	由键值对组成，放在大括号里面
	``` js
	// 注意键值对中的 key 都是 string 类型
	var person = {
	    name: 'Bob',
	    age: 20,
	    tags: ['js', 'web', 'mobile'],
	    city: 'Beijing',
	    hasCar: true,
	    zipcode: null
	};
	```
9. 变量 var

## 字符串
几种特殊用法
1. 下标获取字符
	``` js
	var s = 'Hello, world!';
	s[0]; // 'H'
	s[6]; // ' '
	s[7]; // 'w'
	s[12]; // '!'
	s[13]; // undefined 超出范围的索引不会报错，但一律返回undefined
	// js 中的字符串也是不可变的，所以不能用下标修改字符串
	```
2. toUpperCase() / toLowerCase() / indexOf()
3. substring()

## Array
- 基础知识
``` js
// var arrayName = [data, data, data];
var names = ["Mao","Gandhi","Mandela"];

var sizes = [4, 6, 3, 2, 1, 9];

var mixed = [34, "candy", "blue", 11];

// 用下表读数组
mixed[3]

// add element into array
mixed.push()

// two dimension
var a = [[],[],[]]

// jagged array
var jagged = [[1,2,3],[2,3]]
```

- 获取数组城长度用 length 属性
注意，用下表修改数组的时候，如果超出原来数组长度，则会更改数组的长度。
``` js
var arr = [1, 2, 3];
arr[5] = 'x';
arr; // arr变为[1, 2, 3, undefined, undefined, 'x']
```

- Array 也有 indexOf()
``` js
var arr = [10, 20, '30', 'xyz'];
arr.indexOf(10); // 元素10的索引为0
```

- slice() 是数组版本的 substring()
注意只有一个参数的情况下是，从该参数位置到末尾！！！
``` js
var arr = ['A', 'B', 'C', 'D', 'E', 'F', 'G'];
arr.slice(0, 3); // 从索引0开始，到索引3结束，但不包括索引3: ['A', 'B', 'C']
arr.slice(3); // 从索引3开始到结束: ['D', 'E', 'F', 'G']

// 不传参数的时候可用用来复制数组！
var arr = ['A', 'B', 'C', 'D', 'E', 'F', 'G'];
var aCopy = arr.slice();
aCopy; // ['A', 'B', 'C', 'D', 'E', 'F', 'G']
aCopy === arr; // false 两个数组直接比较返回的是false
```

- push和pop 把数组当成栈来使
- unshift和shift
	``` js
	var arr = [1, 2];
	arr.unshift('A', 'B'); // 返回Array新的长度: 4
	arr; // ['A', 'B', 1, 2]
	arr.shift(); // 'A'
	arr; // ['B', 1, 2]
	```
- sort() 排序，可以传入一个函数来定义如何比较，比较像Java compareTo()
- revers() 反转
- splice(startIndex, length, argsItemToadd) 在某个位置删除添加
- concat() 连接两个数组
- join() 把当前数组用指定的字符串连接起来
	``` js
	var arr = ['A', 'B', 'C', 1, 2, 3];
	arr.join('-'); // 'A-B-C-1-2-3'
	```
- filter() 传入一个函数，遍历每个元素，根据返回的布尔值来过滤元素
- 

## Function
- Javascript的function是个变量？！
``` js

// This is what a function looks like:

var divideByThree = function (number) {
	var val = number / 3;
	console.log(val);
};

// we call the function by name
// Here, it is called 'dividebythree'
// We tell the computer what the number input is (i.e. 6)
// The computer then runs the code inside the function!
divideByThree(6);

// just want it to return a value.
var timesTwo = function(number) {
	return number * 2;
};
```

- arguments
	这是 Javascript 免费赠送的变量，指向当前函数的传入的所有
 

## Object
- 基础
``` js
// An Object 
// js 用大括号定一个对象
var phonebookEntry = {};

// 可以在大括号外面定义属性
phonebookEntry.name = 'Oxnard Montalvo';
phonebookEntry.number = '(555) 555-5555';
phonebookEntry.phone = function() {
	console.log('Calling ' + this.name + ' at ' + this.number + '...');
};

// literal notation
// 也可以在大括号里面定义属性
var friends = {
	name : ['Tom', 'Tony']
}

// Constructor
var me = new Object()
// assign value 
// 访问属性可以用. 但要求必须是有效地变量名
me.age = 23
// 如果无效的话，可用方括号来表示
xiaohong['middle-school'];
me["name"] = 'Jiaming'

phonebookEntry.phone();

// object in for
// for in 本质上是遍历这个对象的所有属性
var list = function(object){
	for(var key in object){
		// key
		console.log(key)
		// value
		console.log(object[key])
	}
}
```

- 对象中的方法
``` js
// method in object
var bob = new Object();
bob.age = 17;
// this time we have added a method, setAge
bob.setAge = function (newAge){
  bob.age = newAge;
};

// Constructor
// js 的构造器都不是一个方法
// 就是加参数，然后声明的时候赋值就好了
function Person(name,age) {
  this.name = name;
  this.age = age;
}

// make bob using our constructor
var bob = new Person("Bob Smith", 30);

// construct a method
function Person(job, married) {
	this.job = job;
	this.married = married;
	// add a "speak" method to Person!
	this.speak = function(){
		console.log('Hello!')
	}
}

// add method to the prototype

// create your Animal class here
function Animal(name, numLegs){
	this.name = name 
	this.numLegs = numLegs
}
// create the sayName method for Animal
Animal.prototype.sayName = function(){
		console.log("Hi my name is " + this.name)
}
```

- type of
``` js
// type of 
var anObj = { job: "I'm an object!" };
console.log( typeof  anObj); // should print "object"

// hasOwnProperty()
var myObj = {
	// finish myObj
	name : 'Jiaming'
};
console.log( myObj.hasOwnProperty('name') ); // should print true
console.log( myObj.hasOwnProperty('nickname') ); // should print false
```


- Inherit class
``` js
// the original Animal class and sayName method
function Animal(name, numLegs) {
	this.name = name;
	this.numLegs = numLegs;
}
Animal.prototype.sayName = function() {
	console.log("Hi my name is " + this.name);
};

// define a Penguin class
function Penguin(name){
	this.name = name
	this.numLegs = 2
}

// set its prototype to be a new instance of Animal

Penguin.prototype = new Animal();

var penguin = new Penguin('Jiaming')
penguin.sayName();

```

- private variables / method
``` js
function Person(first,last,age) {
	this.firstname = first;
	this.lastname = last;
	this.age = age;
	// bankBalnce is a private variable
	var bankBalance = 7500;
	  
	// private method
	var returnBalance = function() {
		  return bankBalance;
	};
	
	this.askTeller = function(){
		return returnBalance
	}
}

```


## Control Flow
``` js
// 和Java并没有什么区别

// for
for (var i = start; i < end; i++) {
  // do something
}

// while
while (loopCondition) {
	
}

// do while
do {
   
} while (loopCondition);

// switch
switch (expression) {
  case value1:
	//Statements executed when the result of expression matches value1
	[break;]
  ...
  case valueN:
	//Statements executed when the result of expression matches valueN
	[break;]
  
  default:
	//Statements executed when none of the values match the value of the expression
	[break;]
}

// for循环的一个变体是for ... in循环，它可以把一个对象的所有属性依次循环出来
```

## Map, Set, Iterable
- Map
``` js
// get 方法
var m = new Map([['Michael', 95], ['Bob', 75], ['Tracy', 85]]);
m.get('Michael'); // 95

// set 和 delete 方法
var m = new Map(); // 空Map
m.set('Adam', 67); // 添加新的key-value
m.set('Bob', 59);
m.has('Adam'); // 是否存在key 'Adam': true
m.get('Adam'); // 67
m.delete('Adam'); // 删除key 'Adam'
m.get('Adam'); // undefined
```

- Set
``` js
var s1 = new Set(); // 空Set
// 创建一个mao 提供一个数组作为输入
var s2 = new Set([1, 2, 3]); // 含1, 2, 3
// 自动过滤重复的元素
var s = new Set([1, 2, 3, 3, '3']);
s; // Set {1, 2, 3, "3"}
// delete 方法删除元素
s.delete(3);
s; // Set {1, 2, "3"}
```

- for..of 遍历Iterable 集合 （Array Set Map 都属于 Iterable 集合）
``` js
var a = ['A', 'B', 'C'];
var s = new Set(['A', 'B', 'C']);
var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
for (var x of a) { // 遍历Array
    alert(x);
}
for (var x of s) { // 遍历Set
    alert(x);
}
for (var x of m) { // 遍历Map
    alert(x[0] + '=' + x[1]);
}
```


## Map Reduce
[map/reduce](http://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/001435119854495d29b9b3d7028477a96ed74db95032675000 'map/reduce')

## Generator


***
Finished!