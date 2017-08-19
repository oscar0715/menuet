---
title: Start Developing iOS Apps (Swift)
tags:
  - Swift
  - Xcode
categories:
  - Technology
  - IOS
date: 2016-10-21 16:30:25
---
Start Developing iOS Apps
就是下面这篇文章，跟着走一遍，自己不懂得地方强调一下，翻一下。（别人看看不太懂吧，主要自己看。）
{% link Start Developing iOS Apps (Swift) 
https://developer.apple.com/library/content/referencelibrary/GettingStarted/DevelopiOSAppsSwift/index.html#//apple_ref/doc/uid/TP40015214-CH2-SW1 Start Developing iOS Apps (Swift)
 %}
<!-- more -->

***

# Swift 语法基础

## 常量与变量

``` swift
// var 声明一个常量
var myVariable = 42
myVariable = 50

// let 声明一个常量
let myConstant = 42

// 显示常量一个变量的类型
let explicitDouble: Double = 70

// 常量的连接
let label = "The width is "
let width = 94
// 此处与Java不同，Int 不能自动转换String
// 需要显式强制转换
let widthLabel = label + String(width) 

// string interpolation
// 字符串 差值
let apples = 3
let oranges = 5
let appleSummary = "I have \(apples) apples."
let fruitSummary = "I have \(apples + oranges) pieces of fruit."
```

## Optinal
``` swift
// optionals
// 在类型后面加上一个问号，表明这个常量可能是一个Int，也可能是nil，也是值缺失的情况
let optionalInt: Int? = 9

// force unwrap operator (!)
// !用在确定值不为nil的时候
let actualInt: Int = optionalInt!

// 举个例子
// 下面三行代码是对的，但是myString有可能不是数字
var myString = "7"
var possibleInt = Int(myString)
print(possibleInt) // 7

// 比如这样，String 不能被转为 Int
myString = "banana"
possibleInt = Int(myString)
print(possibleInt) // nil

// implicitly unwrapped optional
// 假设变量在初始化以后一定非空，隐式
var implicitlyUnwrappedOptionalInt: Int!
```

还是很懵逼啊
查了一下资料：
{% link Swift语法之 ---- ?和!区别 http://blog.sina.com.cn/s/blog_71715bf80102ux3v.html Swift语法之 ---- ?和!区别 %}
Swift语言使用var定义变量，但和别的语言不同，Swift里不会自动给变量赋初始值，也就是说变量不会有默认值，所以要求使用变量之前必须要对其初始化。如果在使用变量之前不进行初始化就会报错.

这时候`Optional`就有用了：`Optional`其实是个`enum`，里面有`None`和`Some`两种类型。其实所谓的`nil`就是`Optional.None`, 非`nil`就是`Optional.Some`, 然后会通过`Some(T)`包装`（wrap）`原始值


## 数组
``` swift
// 数组和Java 差不多吧
var ratingList = ["Poor", "Fine", "Good", "Excellent"]
ratingList[1] = "OK"

// Creates an empty array.
// 初始化一个空数组
let emptyArray = [String]()
```

## 控制流
``` swift
// 0 到 3
for i in 0..<4 {
    total += i
}

// 0 到 4
for i in 0...4 {
    total += i
}
```


## 函数
``` swift
// 参数   person为变量名:String为类型
// 返回值 -> String
func greet(person: String, day: String) -> String {
    return "Hello \(person), today is \(day)."
}
greet(person: "Bob", day: "Tuesday")
```

``` swift
// 可以返回多个返回值了，良心！
func calculateStatistics(scores: [Int]) -> (min: Int, max: Int, sum: Int) {
    var min = scores[0]
    var max = scores[0]
    var sum = 0
    
    for score in scores {
        if score > max {
            max = score
        } else if score < min {
            min = score
        }
        sum += score
    }
    
    return (min, max, sum)
}
let statistics = calculateStatistics(scores: [5, 3, 100, 3, 9])
print(statistics.sum)
print(statistics.2)
```

``` swift
// 函数的返回值是函数
func makeIncrementer() -> ((Int) -> Int) {
    func addOne(number: Int) -> Int {
        return 1 + number
    }
    return addOne
}
var increment = makeIncrementer()
increment(7)
```

## 类
``` swift
// 一个带有构造器的类
class NamedShape {
    // fields
    var numberOfSides: Int = 0
    var name: String
    
    // 构造器
    init(name: String) {
        self.name = name
    }
    
    // method
    func simpleDescription() -> String {
        return "A shape with \(numberOfSides) sides."
    }
}

// 子类, 父类为NamedShape
class EquilateralTriangle: NamedShape {
    var sideLength: Double = 0.0
    
    init(sideLength: Double, name: String) {
        self.sideLength = sideLength
        super.init(name: name)
        numberOfSides = 3
    }
    
    var perimeter: Double {
        get {
            return 3.0 * sideLength
        }
        set {
            sideLength = newValue / 3.0
        }
    }
    
    override func simpleDescription() -> String {
        return "An equilateral triangle with sides of length \(sideLength)."
    }
}
var triangle = EquilateralTriangle(sideLength: 3.1, name: "a triangle")
print(triangle.perimeter) // 调用的是 getter 返回 9.3
triangle.perimeter = 9.9  // 调用的是 setter
print(triangle.sideLength) // 3.3
```

## Enumeration
在默认情况下，swift的 raw value 从0开始
但是在下面enumeration中，制定了 raw value 从1开始，所以就从1开始往后递增
``` swift
enum Rank: Int {
    case ace = 1
    case two, three, four, five, six, seven, eight, nine, ten
    case jack, queen, king
    func simpleDescription() -> String {
        switch self {
        case .ace:
            return "ace"
        case .jack:
            return "jack"
        case .queen:
            return "queen"
        case .king:
            return "king"
        default:
            return String(self.rawValue)
        }
    }
}
let ace = Rank.ace
let aceRawValue = ace.rawValue
```

## Struct 
Structure 

# 用Storyboard制作一个基本的UI
Apple这个Guide要做一个叫 `FoodTracker` 的小 demo
## Source Code 源代码
1. `AppDelegate.swift` 英文Delegate是代表团的意思。这个文件有两个作用
    - app进入的入口，并创造一个循环可以用于不断向app输入事件。此功能由`UIApplicationMain`这个属性来完成，该属性创造了一个`application object`，这个object用于管理APP的生命周期，同时也管理着 一个`app delegate object`，即`AppDelegate `类
    - `AppDelegate`这个类创造了展现app内容的窗口，也可以在这个窗口内展示app状态的转换。
    - `AppDelegate`这个类只有一个变量`window`。app delegate 通过这个window变量来跟踪管理app的窗口。这个window变量是optional的，也就是说，有时候它可以是nil

2. `ViewController.swift`
    - 这个文件定义了`UIViewController`的一个子类：`ViewController`。

## Storyboard

**View**
`View`可以显示在画布上，`View`也可以包含别的`View`，然后产生一个`View`的结构树。
顶层的`View`称为`Content View`。
`text` `field`, `label`, `button`这些都是`View`，是在`content view`下的子`View`

**TextField**
1. Placeholder 可以显示文本框中提示输入
2. 在`Attributes inspector`中设置键盘
    - `Return Key`这个属性设置为`Done`。这样设置以后，弹出的键盘上，`return`那个键就会显示为`Done`
    - 把`Auto-enable Return Key`那个选项打上勾勾。这样的效果呢就是用户必须输入内容以后才能点击`Done`。


## Auto Layout
设置元素在不同屏幕中都可以适配

1. Stack View
2. 修改Pin的值，add constriants

先照顾外层的view，再照顾里层的view

# Connect the UI to Source Code
在Storyboard中，`View controller`执行app的行为。所有的`View Controller`都是`UIViewController`类型的实例，或者它的子类的实例。

那么在代码中，可以新建一个 
`View Controller`类型的实例来控制Storyboard中`View Controller`的行为。
在 `Identity inspector`中可以编辑两者的联系。

如果要通过`ViewController.swift`来控制那些`子View`元素的交互，除了定义`View Controller`的联系以外，那些`子View`也要建立联系——`outlets`和`actions`。

## 新建一个Outlet
打开  assistant editor 模式，方便UI界面和代码界面一起看。

进入`ViewController.swift `
选中左边视图中的一个元素，按住control，并用鼠标把元素拖到右边的代码界面中，例如一个输入框textField。这样，在代码中就生成了一个指向这个textField元素的pointer:
``` 
// IBOutlet的意识这个元素是从Interface Builder，也就是左边的试图中链接到这里的，所有有一个IB的前缀
// weak 表明这个元素可能是nil
// 这个变量的类型是UITextField
// 最后的叹号表明这个变量是 implicitly unwrapped optional 类型的
@IBOutlet weak var nameTextField: UITextField!

对label做同样的操作
@IBOutlet weak var mealNameLabel: UILabel!
```


## 定义app要执行的动作Action
IOS app 是`事件导向`的程序。
用户操作 --> 代码运行 --> 事件执行

`Action`: 某个事件响应的代码

与创建`outlet`一样，创建`action`也是用`control`+`drag`。不过这时候不是定义一个变量，而是定义一个方法。

选择类型的时候有一个选项是 `anyObject`. 在swift中`anyObject`是用来描述可以属于任意一种类的类型。

我们对一个`button`做创建`Action`操作，并选择类型为`UIButton`
``` swift
// sender 参数指向负责触发这个方法的对象，此处是一个button
// IBAction 的前缀与IBOutlet同理
@IBAction func setDefaultLabelText(_ sender: UIButton) {
    // 定义一个动作
    mealNameLabel.text = "Default Text"

}
```

这个动作呢，实质上我们使用了`target-action pattern`这个IOS的设计模式：
1. 一个对象A向另一个对象B发送消息E
2. 消息E触发对象B发生一个具体的事件

具体来说
1. 消息E就是代码中的action 方法
2. 目标对象B就是一个可以执行时间的的对象
3. 发送消息的对象A通常是一个控制对象，例如`button`，`slider`，`switch`。
4. 用于触发事件的动作可以包括`tap`， `drag`， `value change`。

## 操作用户的输入
接下来要实现的功能是，在输入框中输入字符串，回车后改变一个Label的文字

这个时候需要一个delegate对象，这个对象拥有一个目标对象的指针，通过这个指针取操作目标对象。

一个对象要作为另外一个对象的`delegate`的时候需要一个协议`protocol`。
对于一个输入框来说，这个协议就是`UITextFieldDelegate`

在`ViewController.swift`中
原来的类定义是
``` swift
class ViewController: UIViewController {}
```

现在改变为
``` swift
// 这样就确认了UITextFieldDelegate这个协议
class ViewController: UIViewController, UITextFieldDelegate {}
// We gave the ViewController class the ability to identify itself as a UITextFieldDelegate
// 也就是说，现在ViewController有了作为Delegate的能力
```

于是
``` swift
override func viewDidLoad() {
    super.viewDidLoad()
    
    // Handle the text field’s user input through delegate callbacks.
    nameTextField.delegate = self
    // 这个时候ViewController这个类就是nameTextField的delegate了
}
```

IOS app中 first responder这个概念就是代码中接收用户动作的地方

我们在键盘上输入东西的时候，输入框是 first responder ，输入完成之后，输入框要交出它的 first responder状态，那下面这个方法就是指向这个动作的方法啦
``` swift
func textFieldShouldReturn(_ textField: UITextField) -> Bool {
    // Hide the keyboard.
    textField.resignFirstResponder()
    return true
}
```

然后是通过输入框的输入改变label
``` swift
func textFieldDidEndEditing(_ textField: UITextField) {
    mealNameLabel.text = textField.text
}
```

# 学习View Controller

## View Controller的生命周期
在这个小教程中，FoodTracker是一个单场景的app，所以整个UI纸杯一个 view controller控制。如果要开发更加复杂的app，则需要多个场景，那么对于每个场景就有 load 和 unload的操作，用来控制场景的切换。

来看看 `UIViewController` 的方法们，这个类的各个方法可以控制。Apple 给的大图Orz 

![UIViewController](https://developer.apple.com/library/content/referencelibrary/GettingStarted/DevelopiOSAppsSwift/Art/4_vclife_2x.png "UIViewController")

1. `viewDidLoad()`
`View Controller` 这时候会把 `content view` (在view hierarchy顶层的那个view) 创建出来，并从 `storyboard`加载出来。这个方法起一个初始化的作用。但是 `view` 有时候会因为资源的限制被清理掉，所有这个方法可能会被多次调用。
2. `viewWillAppear()`
来操作`view`出现前的一些准备动作
在`view`出现之前需要马上被调用，因可能会被其他的 `view` 抢占资源
3. `viewDidAppear()`
来操作`view`出现之后要加载的动画或者数据。
同样也需要马上被操作，因为可能被其他的`view`抢占资源


这里也运用了 MVC 结构
1. Model 用来跟踪数据 data
2. View 用来展示交互界面
3. Controller 用来控制 view，同时也是接通 Model 和 View 的一个通道



然后我们添加一个 Image View 
并添加到`ViewController.swift`中
添加`outlet`  

这里要注意 `Views` 和 `Controls` 的区别，
- `View` 用来展示内容
- `Control`  用来改变 `View。`
- `Control`(`UIControl`) 是个 `UIView`的子类

因为`Image View`不像`button`是`Control`，所以不能直接添加 `Action` 来操作。
这时候要借助于 `Gesture recognizers` 来给 `view` 对某个动作有响应的能力。

向`view`中添加一个 `tap gesture`
然后就可以把这个`tap gesture`进行添加`Action`的操作了

// 我debug了一下午…………呵呵……
UIImageView的用户交互默认是关闭的，也就是说添加到ImageView上的事件都不会响应，需要我们手动设置userInteractionEnabled属性为真

// 心太累的这个章节接下来的东西 大都是代码了 就不放上来了。


// 全都都是旧代码……swift2的 和 swift3 相差还是很大的……













