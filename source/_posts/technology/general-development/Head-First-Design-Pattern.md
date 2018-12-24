---
title: 图解《Head First Design Pattern》
tags:
  - Design Pattern
categories:
  - Technology
  - General
date: 2017-12-05 14:14:15
---

Head First Desgin Pattern
<!-- more -->

***

# Strategy Pattern
  
举个例子：Duck 类
不同种类的鸭子继承了 Duck 父类：

![简单继承](https://menuet-1258369060.cos.ap-shanghai.myqcloud.com/design-pattern/strategy/1.1-simple-extend.png)

但是，当我们要给鸭子加入一个 `fly()` 方法的时候，这样的方式就会遇到麻烦：不会飞的鸭子怎么办呢？只能在 Rubber Duck 中重写 `fly()` 方法。也就是说：
1. 我们改动了父类就会影响到所有的子类，
2. 造成代码重复，所有不会飞的鸭子都要重写这个方法

![简单继承](https://menuet-1258369060.cos.ap-shanghai.myqcloud.com/design-pattern/strategy/1.2-simple-extend.png)

那我们换用接口的方式来实现：
1. 我们仍然无法避免代码重复的问题，所有会飞的鸭子都要自己实现一遍 `fly()` 方法

![简单接口](https://menuet-1258369060.cos.ap-shanghai.myqcloud.com/design-pattern/strategy/2-simple-interface.png)

这个时候就可以考虑设计模式了

策略模式：
1. 把 `fly()` 方法 和  `quack()` 方法从 Duck 类中拆出来。定义自己的接口和实现类
2. Duck 类中用接口类型定义 Fly 和 Quack 的行为变量
3. Duck 类中用 performFly() 方法触发  `fly()` 方法，但并不关心这个方法是怎么实现的
4. 甚至可以写 setter 方法，在运行时改动 FlyBehavior 的实现

![策略模式](https://menuet-1258369060.cos.ap-shanghai.myqcloud.com/design-pattern/strategy/3-strategy-pattern.png)


主要的思想有两点:
1. 具体的行为和 Client 分开
2. 组合比继承更加灵活

<br>

***

<br>

# Observer Pattern

背景：
1. 一个气象中心提供天气信息（温度，湿度，气压）
2. 三个不同的显示设备，需要到气象中心获取天气信息，并做不同样式的展示。（Clients）
    - currentConditionDisplay
    - statisticsDisplay
    - forecastDisplay

![Weather Data Class](https://menuet-1258369060.cos.ap-shanghai.myqcloud.com/design-pattern/observer/WeatherDataClass.png?v=2)


``` Java
// 最直接的实现方法
public class WeatherData {

    public void measurementsChanged() {
        
        // getter 获取最新信息
        float temp = getTemperature();
        float humidity = getHumidity();
        float pressure = getPressure();

        // 更新三个客户端
        currentConditionDisplay.update(temp, humidity, pressure);
        statisticsDisplay.update(temp, humidity, pressure);
        forecastDisplay.update(temp, humidity, pressure);
    }
}
```

上面的3个 update 方法，有两个问题：
1. 每次增加或者改动客户端，都要涉及到 WeatherData 端的改动
2. 显然代码重复了

所以使用观察者模式：

![观察者模式](https://menuet-1258369060.cos.ap-shanghai.myqcloud.com/design-pattern/observer/ObserverPattern.png?v=2)

要点：
1. 核心是 Subject 和 Observer 两个接口
2. Subject的实现类中维护一个 Observer 的列表
    - 用 register 方法增加新的 Observer
    - 用 remove 方法移除 Observer
    - notify 方法通知列表中所有的 Observers（调用 Observer 的 update 方法）
    - Observer 中也可以维护一个 Subject 对象，来主动选择 register 或者 remove
3. 观察者模式实现了 push 的机制

<br>

***

<br>


# Decorator Pattern

**背景：**
Starbuzz 咖啡店卖四种咖啡：HouseBlend, DarkRoast, Decaf, Espresso

![Class Diagram](https://menuet-1258369060.cos.ap-shanghai.myqcloud.com/design-pattern/decorator/1-background.png)

咖啡店还提供了四种佐料：milk, soy, mocha, whip
每种咖啡都能加入任意种佐料，每种都要收费，那么如何才能灵活计费呢？

第一种方法：
简单地在父类中，将是否含有佐料用 Boolean 类型的变量标记出来：
![Simple Extension](https://menuet-1258369060.cos.ap-shanghai.myqcloud.com/design-pattern/decorator/2-inheritance.png)

1. 显然这样方法最大的问题在于，一旦有新的佐料，我们就要对所以子类的 `cost()` 进行修改。
2. 如果某种佐料的价格发生了变化，我们也要对现有的代码进行修改
3. 总之这种代码可拓展性太差

设计原则：
>类应该对拓展开放，而拒绝修改

那么接下来，**装饰器模式**就要登场了。

思路就是，我们有一杯 Beverage，然后在 RunTime 对它添加佐料。

比如说，消费者要了一杯 DarkRoast 开发，要求添加 Mocha 和 Whip
1. 那我们就创建一个 DarkRoast Object
2. 再装饰一个 Mocha Object
3. 再装饰一个 Whip Object
4. 然后 cost() 方法通过 Delegation（委托）去累加佐料的价格

我们可以将装饰的对象想象成一个 wrapper 类（包装类）

![Wrapper](https://menuet-1258369060.cos.ap-shanghai.myqcloud.com/design-pattern/decorator/3-wrapper.png)

调用cost的方法的时候，我们调用最外层 Whip类的 cost 方法，然后不断向里层调用，然后类似递归地不断向外累加并返回

（很熟悉啊，链表+递归？）

![Call cost()](https://menuet-1258369060.cos.ap-shanghai.myqcloud.com/design-pattern/decorator/4-call-cost%28%29-method.png)

来看一看**装饰器模式**的定义
>装饰器模式能够动态地给对象添加新的职责。

这种灵活性可以代替继承关系

![Decorator Pattern on Coffee](https://menuet-1258369060.cos.ap-shanghai.myqcloud.com/design-pattern/decorator/5-decorator-on-coffee.png)

要点：
1. 装饰器的类和咖啡的实现类是 same type，因为类型相同，装饰器类才能包在 Beverage 的实现类外面。
2. 装饰器的实现类(例如 Milk)类中有一个 Beverage 的变量，可以看做是接口类型的变量，达到了“组合”的目的。所以此处装饰器类并没有利用“继承”来使用`cost()`方法，而是利用了“组合”

``` Java
// Beverage 基类
public abstract class Beverage {
    String description= "unknown beverage";

    // getDescription() 已经实现好了
    public String getDescription() {
        return description;
    }

    // cost() 是抽象方法    
    public abstract double cost();
}

// 装饰器的抽象类
// 继承了 Beverage 基类， 所以是和别的 Beverage 可替换的
public abstract class CondimentDecorator extends Beverage {
    // 所有的装饰者实现类都要用类似于链表的方式重新实现 getDescription() 方法
    public abstract String getDescription();
}

```

``` Java
// Beverage 的实现类
public class Espresso extends Beverage {

    // 重写构造器，将基类中的变量 description 重新赋值
    public Espresso() {
        description = "Esprsso";
    }

    // 实现 cost()   
    public double cost() {
        return 1.99;
    };
}

// 另外的 HouseBlend, DarkRoast, Decaf 都同理
```

``` Java
// 装饰器的实现类
// 继承了 condimentDecorator（CondimentDecorator 继承了 Beverage）
public class Mocha extend CondimentDecorator {
    private Beverage beverage;

    // 构造器，初始化了，这个装饰器装饰的 Beverage 类
    public Mocha(Beverage beverage) {
        this.beverage = beverage;
    }
    
    // 重新getDescription()方法
    // 这里就是所谓的 delegate
    public String getDescription() {
        return beverage.getDescrition + ", Mocha";
    }

    // 实现cost()方法，完全就是链表啊
    public double cost() {
        return beverage.cost() 0.2;
    }
}
```

``` Java 
public class Starbuzz {
    public static void main() {
        Beverage beverage = new Espresso();
        beverage = new Mocha(beverage);
        beverage = new Whip(beverage);
        System.out.println(beverage.getDescription() 
            + " $ " + beverage.cost()
        )
        // 假设 Espresso 1.99, Mocha 0.2, Whip 0.2
        // 输出如下
        // Espresso, Mocha, Whip $ 2.39
    }
}
```

<br>

***

<br>

# Factory Pattern

背景：仍然是Pizza店，假设我们要根据不同的输入来定制不同的 Pizza：

``` Java
Pizza orderPizza (String type) {
    // Pizza 是一个Interface（或者父类）
    Pizza pizza;

    if(type.equal("cheese")) {
        pizza = new CheesePizza();
    } else if (type.equal("greek")) {
        pizza = new GreekPizza();
    } else if (type.equal("pepperoni")) {
        pizza = new PepperoniPizza();
    }

    pizza.prepare();
    pizza.bake();
    pizza.cut();
    pizza.box();
    return pizza;
}

// 如果是这样实现的话，每次有新的 pizza，都要更改 orderPizza这个方法
```

可以发现，变动的代码只是那一堆用来 new 出一个 Pizza 类的 if / else 
那么，我们就可以把这一段代码抽出来

``` Java
public class SimplePizzaFacrtory {
    public Pizza createPizza(String type) {
        if(type.equal("cheese")) {
            pizza = new CheesePizza();
        } else if (type.equal("greek")) {
            pizza = new GreekPizza();
        } else if (type.equal("pepperoni")) {
            pizza = new PepperoniPizza();
        }
        return pizza;
    }
}
```


重新写一下 PizzaStore 类：
``` Java

public class PizzaStore {
    SimplePizzaFactory factory;

    public PizzaStore(SimplePizzaFactory factory) {
        this.factory = factory;
    }

    public Pizza orderPizza (String type) {
        Pizza pizza;

        pizza = factory.createPizza(type);

        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();
        return pizza;
    }
}

```

![简单工厂方法](https://menuet-1258369060.cos.ap-shanghai.myqcloud.com/design-pattern/factory/simple-factory.png)

要点：
1. Pizza Store 是 SimplePizzaFactory 的 Client。抽象出来抽象出来以后，这个 SimplePizzaFactory 类，既可以提供给订单系统，也可以提供给菜单系统，也可以提供给外卖系统
而一旦有新增或修改 Pizza，我们只要修改 Factory 类就好了。
2. Pizza 是我们的 Product。 SimplePizzaFactory 是唯一关心 Concrete Pizza 的部分

（其实把 SimplePizzaFactory 抽象出一个接口来，就变成了抽象工厂方法 Abstract Factory）

***

接下来介绍 工厂方法模式 Factory Method Pattern。

有时候不同地方的披萨店需要制作不同的Pizza

![Pizza Factory Method Pattern](https://menuet-1258369060.cos.ap-shanghai.myqcloud.com/design-pattern/factory/pizza-factory.png)
``` Java 
// 抽象类，用于不同的店去继承
public abstract class PizzaStore {

    public Pizza orderPizza(String type) {
        Pizza pizza;

        pizza = createPizza(type);

        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();
        return pizza;
    }

    // 把 Factory 类的 createPizza 方法移到 PizzaStore 中
    // 作为一个抽象方法，让子类自己去实现
    abstract Pizza createPizza(String type);
}
```

例如在 Chicago 的披萨店可以这样实现：
``` Java 
public Pizza createPizza(String type) {
    if(type.equal("cheese")) {
        pizza = new ChicagoStyleCheesePizza();
    } else if (type.equal("greek")) {
        pizza = new ChicagoStyleGreekPizza();
    } else if (type.equal("pepperoni")) {
        pizza = new ChicagoStylePepperoniPizza();
    }
    return pizza;
}
```

而在 New York 的披萨店则可以这样实现
``` Java 
public Pizza createPizza(String type) {
    if(type.equal("cheese")) {
        pizza = new NYStyleCheesePizza();
    } else if (type.equal("greek")) {
        pizza = new NYStyleGreekPizza();
    } else if (type.equal("pepperoni")) {
        pizza = new NYStylePepperoniPizza();
    }
    return pizza;
}
```

order() 方法是在父类中实现好的，也就是说，可以做统一的控制，属于不变动的代码
create()方法则属于需要变动的部分，所以让各个子类自己去实现

工厂方法:
1. 负责对象的 initialization 和 encapsulation
2. 工厂方法的部分，从父类的代码中解耦出来，在子类中去实现

PizzaStore 这个类的要点：
1. PizzaStore 是一个 abstract creator. 类中定义了抽象工厂方法。
2. PizzaStore 中的 order() 方法依赖一个 abstract product. 它不需要关心这个pizza到底是什么种类的，只要它是一个 pizza 就行了
3. NYStylePizzaStore 继承了 PizzaStore，并自己实现一个工厂方法

另一个角度看
1. NYStylePizzaStore 封装了所有 NYStylePizza 的实现方式，所以只有 NYStylePizzaStore 知道 NYStylePizza 的细节。 ChicagoStylePizzaStore 同理
2. 抽象类 PizzaStore 只知道 Pizza 这个抽象类 的细节。（PizzaStore 是一个 level 更高的类， Pizza 也是 high level 的 Product）
3. 所以说，抽象工厂方法封装了具体实现的细节


![Factory Pattern](https://menuet-1258369060.cos.ap-shanghai.myqcloud.com/design-pattern/factory/factory-pattern.png)
我们再抽象一下工厂方法模式

1. Creator 中的 abstract factoryMethod() 负责生产一个 Product。
2. 生产 Product 的工厂方法是一个接口，所以 Creator 中的 operationMethod() 方法可以对接口进行统一的操作，而不需要关心具体的实现
3. Concreate Creator 来决定 这个 Product 的具体实现，也只有 ConcreateCreator 关心它的 Product 的具体实现
4. 最终的效果是，实现了 ConcreteProduct 与 Creator 类的解耦


<br>

***

<br>

# Singleton Pattern

单例模式，两个要点：
1. 一个类只有是一个实例
2. 这个类提供了一个全局获取这个实例的口子

```java
public class Singleton {
    private Singleton uniqueSingleton;

    // private 的构造器
    private Singleton() {};

    // 公共调用的静态方法
    // 可以通过 Singleton.getInstance 获取实例
    public static Singleton getInstance() {
        if (uniqueSingleton == null) {
            uniqueSingleton = new Singleton();
        }

        return uniqueSingleton;
    }
}
```

以上的代码
1. 构造器为 private 保证了只有一个实例
2. 公共静态方法 getInstance() 是一个全局的获取该唯一实例的入口

但是这段代码有一个问题，在 uniqueSingleton 初始化的时候，在多线程的情况下会出现 new 出多个实例的情况：
1. 线程1 new 出一个实例，正在使用这个类，例如赋值操作
2. 线程2 同时进来，又new 出了一个实例，线程1 赋的值就废了

一个粗暴的方法就是给 getInstance() 方法加上 synchronized ：

```java 
public static synchronized Singleton getInstance() {
    if (uniqueSingleton == null) {
        uniqueSingleton = new Singleton();
    }

    return uniqueSingleton;
}
```

以上代码：
1. 保证了只会调用构造器一次，但是性能就很差了
2. 事实上只有在类初始化的时候，才需要 synchronized 来防止多次调用构造器，只要 uniqueSingleton实例化以后，就不怕多线程了
 
改进一下，用 `double-checked locking` 来保证只有在初始化的时候才用到 synchronized ：
```java
public class Singleton {
    private volatile Singleton uniqueSingleton;

    // private 的构造器
    private Singleton() {};

    public static Singleton getInstance() {
        if (uniqueSingleton == null) {
            // 只有在初始化的时候才会进到 synchronized 的代码块
            synchronized （Singleton.class) {
                // 进来以后再判断一下有没有别的先进来的线程已经 new 过了
                if (uniqueSingleton == null) {
                    uniqueSingleton = new Singleton();
                }
            }
        }

        return uniqueSingleton;
    }
}
```

<a href="/java/Java%E5%86%85%E5%AD%98%E5%8F%AF%E8%A7%81%E6%80%A7%E3%80%90Java%E7%B3%BB%E5%88%97%E7%AC%94%E8%AE%B0%E3%80%91/">关于 Volatile 和 synchronized 可以看着一篇文章</a>

<br>

***

<br>

# Command Pattern
背景:
下面的类图展示了不同家电的开关接口，不同的家电开关的方法都不一样，现在要给这些家电做一个统一的开关，放在一把统一的遥控器上：

![不同的电器的类图](https://menuet-1258369060.cos.ap-shanghai.myqcloud.com/design-pattern/command/Background.png)

怎么做呢

1. 定义一个控制器的接口，这个控制器是一个统一接口，只有一个 `execute()` 方法
``` Java
public interface Command {
    public void  execute();
}
```

2. 接下来，给灯写一个开灯的控制器类，这个类实现了上面的控制器接口
    实际上这个控制器就是在 Light 类外面封装了一层，这样可以不侵入 Light 类的代码，用execute()方法实现了统一的开关封装
``` Java
public class LightOnCommand implement Command {
    Light light;

    public LightOnCommand(Light light) {
        this.light = light;
    }

    public void execute() {
        light.on();
    }
}
```

3. 然后写遥控器类
```java
public class SimpleRemoteControl {
    
    Command slot;

    public SimpleRemoteControl() {}

    // setter
    public setCommand(Command command) {
        slot = command;
    }

    public void buttonWasPressed() {
        // 触发 slot 的 execute 方法
        slot.execute();
    }
}
```

4. 写一个 Test 类把上面的类整合在一起
``` Java
public class RemoteControlTest {
    public static void main (String[] args) {

        // 遥控器
        SimpleRemoteControl remote = new SimpleRemoteControl();

        // 开灯的控制类
        Light light = new Light();
        LightOnCommand lightOn = new LightOnCommand(light);

        // 把控制类set到 遥控器中去
        remote.set(lightOn);

        // 调用遥控器的 按按钮方法
        remote.buttonWasPressed()
    }    
}
```

Command Pattern 的类图如下：


![Command Pattern](https://menuet-1258369060.cos.ap-shanghai.myqcloud.com/design-pattern/command/CommandPattern.png)

图中的元素分解：
1. client（就是上面的 Test类）：
    - new 一个控制器实例（例如 LightOnCommand）
    - set 控制器的Receiver （Light）
2. invoker（就是 RemoteControl）
3. Receiver （Light）
    - Command的接受者
    - 也就是具体的业务方
4. command 接口
    - 定义了统一的execute() 
5. ConcreteCommand (就是SimpleRemoteControl)
    - execute() 封装了 Receiver 的具体 action 方法（例如 on()）

<br>

***

<br>

# Adapter Pattern

定义：
>把一个接口转化成另外一个接口，方便适配 Client

适配器模式：
1. client 代码和 Server 代码接口不匹配
2. 写一个中间的 Adaptor 作为适配器
3. Adaptor 和 Server 代码是 Compose 关系

举个例子：
1. 你要操作一只鸭子，但是只有一只火鸡
2. 没办法，只能用火鸡代替一下鸭子
3. 所以要写一个火鸡到鸭子的适配器

``` Java

// 你想要的 鸭子
public interface Duck() {
    
    public void quack();
    public void fly();
}

```

``` Java

// 现在只有火鸡
public interface Turkey() {
    
    public void gobble();
    public void fly();
}

```

``` Java

// 写一个适配器
public class TurkeyAdaptor implements Duck() {
    
    Turkey turkey;

    public TurkeyAdaptor(Turkey turkey) {
        this.turkey = turkey;
    }

    public void quack() {
        turkey.gobble();
    };

    public void fly() {
        // 鸭子比火鸡飞得远
        // 鸭子飞一下，火鸡要飞 5 下
        for(int i = 0; i < 5; i++) {
            turkey.gobble();
        }
    };

}
```

来看看适配器模式的 UML 图
![Adapter Pattern](https://menuet-1258369060.cos.ap-shanghai.myqcloud.com/design-pattern/adapter/Adaptor-Pattern.png)

1. Target 是用户想要操作的类，也就是上面的 Duck
2. Adaptee 是现有能提供的类，也就是上面的 Turkey
3. Adaptor 就是中间的适配器，也就是上面的 TurkeyAdaptor

Adapter 对 Duck 完成了包装，Client 完全不会感知 Duck 类。
Duck 也不需要为了 Client做出任何改动。这就是 Adapter 类的作用

<br>

***

<br>

# Facade Pattern

Facade 定义：为复杂的子系统创建一个简化的，统一的接口。
（在子系统上包装了一层简单的接口）

Facade 和 Adapter 的联系：
1. 目的不同：
    - Facade 提供了一个简化的接口
    - Adapter 提供了一个适配 Client 的接口
2. 两者都进行了包装
    - Facade 对子系统进行了包装
    - Adapter 对 Adaptee 进行了包装
3. 两者都通过包装完成了 Client 对内部系统的解耦，如果遇到接口的变化，之间要改变 Facade 或者 Adapter 就好了

![Facade Pattern](https://menuet-1258369060.cos.ap-shanghai.myqcloud.com/design-pattern/facade/Facade-Pattern.png)

Facade Pattern 体现了一个设计原理：
Principle of Least Knowledge：
减少需要交互对象的数量，只关心最亲近的对象就好了，这样能减少依赖，降低对象间的耦合。

举一个例子
``` Java
public float getTemp() {
    Thermometer thermometer = station.getThermometer();
    return thermometer.getTemperature();
    // 这个方法从 Station 中获取 Thermometer 对象
    // 再去操作 thermometer
    // 那 getTemp() 方法就不太纯粹了，多依赖了 Thermometer 对象
    // 破坏了 Least KnowLedge 原则
}
```

``` Java
public float getTemp() {
    return station.getTemperature();
    // 如果直接从 station 方法获得温度，就比较好
    // 让 station 去关心 thermometer 对象就好了
}
```


<br>

***

<br>

# Template Pattern

举个例子，一家店同时提供了咖啡和茶，因为制作茶和咖啡的方法不同，所以两个类也有所不同：

![Problem background](https://menuet-1258369060.cos.ap-shanghai.myqcloud.com/design-pattern/template/problem-background.png)

1. 父类 `Caffeine` 中有两个子类共有的三个方法
2. 子类都要 override prepareRecipe() 方法，因为咖啡和茶的的配方不同
3. 咖啡和茶两个类中各自有一些特有的方法

但是其实我们还可以进一步抽象:
1. brewCoffeeGrinds() 和 steepTeaBag() 方法都是用热水冲泡
2. addSugarAndMilk() 和 addLemon() 则其实是添加一些佐料

也就是我们可以把泡咖啡和泡茶抽象成四个步骤:
1. 煮开水
2. 用热水冲泡咖啡或者茶
3. 把泡好的咖啡或者茶倒到杯子里
4. 添加适当的佐料

所以我们可以这样抽象:

![further abstraction](https://menuet-1258369060.cos.ap-shanghai.myqcloud.com/design-pattern/template/further-abstraction.png)

只要在子类里面去重写 brew() 和 addCondiment() 方法就好了

在这里 CaffeineBeverage 定义了一系列规范的流程。

然后我们可以进一步推出我们的模板模式
![template pattern](https://menuet-1258369060.cos.ap-shanghai.myqcloud.com/design-pattern/template/template-pattern.png)

注意：
1. 上图的 templateMethod() 依次调用了 primitiveOperation1(), primitiveOperation2()

所以模板模式的定义为：用接口的模板方法定义了 algorithm的各个步骤，在子类中去实现具体的步骤，而不会影响到算法的结构。

好处就是：把固定的步骤和具体的实现结构开来
1. 接口来定义步骤和顺序
2. 子类去完成每个步骤具体的实现


再来看看具体的代码
``` Java
abstract class AbstractClass {

    // 模板方法 final 子类不能改动
    final void templateMethod() {
        
        // 定义了算法的步骤和顺序
        primitiveOperation1();
        primitiveOperation2();
        concreateOperation;
    }

    // 抽象方法1，需要子类重写
    abstract void primitiveOperation1();

    // 抽象方法2，需要子类重写
    abstract void primitiveOperation2();

    // concreateOperation 在父类中实现了一个具体的方法
    // 这个方法是所有子类都通用的，而且子类不能修改它
    final void concreateOperation() {
        // implementation 
    }

    // 这里有个什么都不干的 hook 方法
    // 子类可以选择override ，也可以选择无视它
    void hook() {
        // do nothing 
    }

    // 钩子方法可以选择性地插入到算法的流程当中
}
```

当算法的某个部分是 optional 的时候
可以选择用 hook method 

举个具体的例子：
```java 
public class CaffeineBeverageWithHook {

    // 模板方法 
    void prepareRecipe() {
        
        // 定义了算法的步骤和顺序
        boilWater();
        brew();
        pourInCup();
        if (customerWantsCondiments()) {
            addCondiments();
        }
    }

    void brew() {
        // 重写
    }

    // 抽象方法，需要子类重写
    void addCondiments() {
        // 重写
    }

    // 所有子类都通用的方法，在父类里面就实现好了
    void boilWater() {
        // implementation 
    }

    void pourInCup() {
        // implementation 
    }

    // 这就是一个 hook 方法
    // 子类可以选择性地去重写
    boolean customerWantsCondiments() {
        return true
    }
}
```

<br>

***

<br>

# Iterator Pattern

迭代器模式：封装了集合的底层数据结构，提供了统一的遍历集合的方法

<br>

***

<br>

# State Pattern 

背景：一台糖果自动贩卖机

状态图如下：

![State Diagram](https://menuet-1258369060.cos.ap-shanghai.myqcloud.com/design-pattern/state/state-diagram.png)

接下来实现一个糖果机的类


``` Java
public class GumballMachine {
    // 四种状态码
    final statict int SOLD_OUT = 0;
    final statict int NO_QUARTER = 1;
    final statict int HAS_QUARTER = 2;
    final statict int SOLD = 3;

    // 当前状态
    int state SOLD_OUT;

    // 剩余的糖果
    int count = 0;

    // 构造器
    public GumballMachine(int count) {
        this.count = count;
        if (count > 0) {
            state = NO_QUARTER;
        }
    }

    // 具体的Action
    public void insertQuarter() {
        // 针对不同的状态做出不同的处理
        if (state == HAS_QUARTER) {
            System.out.println("You cannot insert another quarter");
        } else if (state == NO_QUARTER) {
            state = HAS_QUARTER;
            System.out.println("You inserted a quarter");
        } else if (state == SOLD_OUT) {
            // implement
        } else if (state == SOLD) {
            // implement
        }
    }

    // 其他各个操作也会对不同的状态做出不同的处理
    // 此处不作重复的处理
    public void insertQuarter() {}
    public void ejectQuarter() {}
    public void turnCrank() {}
    public void dispense() {}
}

```

但是这段代码有非常严重的问题，假如新增了一个状态，那么就要对每个 Action 都要进行改动，整个代码就无法维护。

所以需要进行一次重构 refactor 

重构后的类图如下图所示

![state-pattern](https://menuet-1258369060.cos.ap-shanghai.myqcloud.com/design-pattern/state/state-pattern%20.png)

1. 定义了 State 接口，定义了糖果机所有的 Action 方法
2. 每种状态都会去实现这个 State 接口在特定状态下的对应的方法

举个例子：
``` Java
public class NoQuarterState implements State {
    GumballMachine gumballMachine;

    // 每个状态内会有一个 gumballMachine 的指针
    // 用来在别的方法里面去改变糖果机的状态
    public NoQuarterState (GumballMachine gumballMachine) {
        this.gumballMachine = gumballMachine;
    }

    // 具体的实现，
    public void insertQuarter() {
        // 有些实现会涉及到状态的变化
        gumballMachine.setState(gumballMachine.getHasQuarterState());
    }
    public void ejectQuarter() {}
    public void turnCrank() {}
    public void dispense() {}
}
```

再来看看 GumballMachine 的实现
``` Java
public class GumballMachine {
    
    State soldOutState;
    State soldState;
    State noQuarterState;
    State hasQuarterState;

    // 这时候的 state 指向一个对象
    State state = soldOutState;
    int count = 0;

    public gumballMachine(int numberGumballs) {
        // 构造器给各个变量赋值
        // 省略了。。。。
    }

    public void insertQuarter() {
        state.insertQuarter();
    }

    // 别的方法以此类推

    // setter 方法
    void setState(State state) {
        this.state = state;
    }
}
```

总结一下我们所做的改变：
1. 我们把具体行为的实现放到了每个状态的实现类中
2. 我们再也不用处理一堆 if 代码
3. GumballMachine 类变得可拓展，而不需要改变。所有的改变都在各个状态的实现类中实现。

Client 对 GumballMachine 的操作最终会 delegate 到具体的状态实现中去操作。

所以状态模式的定义为：
对象的行为可以随着内在的状态的改变而改变。

状态模式的类图如下：

![class diagram](https://menuet-1258369060.cos.ap-shanghai.myqcloud.com/design-pattern/state/state-pattern%20-diagram.png)

1. Context 就是具有不同状态的对象，就是之前例子中的`Gumball Machine`
2. Context 每次接到一个请求，都会去调用 handle() 方法，然后 delegate 到给**当前的状态实现类**去处理

其实结构上状态模式和策略模式还是一样的，但是状态模式的重点在于，可以在不同的状态间进行转化。

<br>

***

<br>






