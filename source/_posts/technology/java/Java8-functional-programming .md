---
title: Java8函数式编程 【Java系列笔记】
tags:
  - Java
categories:
  - Technology
  - Java
date: 2018-06-01 11:40:00
---

Java8 functional programming 

<!-- more -->

***

***

# Lambdas

``` Java
List<String> words = Arrays.asList("Java", "is", "the", "best", "language");
Collections.sort(words);
System.out.println(words);

// [Java, best, is, language, the]
```

```  Java
List<String> words = Arrays.asList("Java", "is", "the", "best", "language");
Collections.sort(words, new Comparator<String>() {
    @Override
    public int compare(String s1, String s2) {
        return Integer.compare(s1.length(), s2.length());
    }
});
System.out.println(words);

// [is, the, Java, best, language]
```

``` Java
// 入参里面里面有一个叫 Comparator 的接口
public static <T> void sort(List<T> list, Comparator<? super T> c) {
    list.sort(c);
}
```

``` Java
@FunctionalInterface  // 函数式接口！有且仅有一个抽象方法
public interface Comparator<T> {
    
    int compare(T o1, T o2);

    boolean equals(Object obj);  // Object 的 public 方法

}
```

``` Java
Collections.sort(words, (s1, s2) -> Integer.compare(s1.length(), s2.length()))

// lambda 表达式的组成：
// 参数列表， 箭头->， 表达式
```


``` Java
// Type inference 类型推断

public interface Comparator<T> {  // <T>  是 String
    int compare(T o1, T o2);
}

```


``` Java
// 方法引用
// 方法引用是一个Lambda表达式
// 是一个更加紧凑，易读的Lambda表达式

Collections.sort(words, Comparator.comparingInt(String::length));
```

``` Java
// Separation of Concern
Collections.sort(words, Comparator.comparingInt(String::length));

/************************************/
// 关心创建一个 Comparator，怎么比较
Comparator.comparingInt()

// 静态工厂方法
public static <T> Comparator<T> comparingInt(ToIntFunction<? super T> keyExtractor) {
    Objects.requireNonNull(keyExtractor);
    return (Comparator<T> & Serializable)
        (c1, c2) -> Integer.compare(keyExtractor.applyAsInt(c1), keyExtractor.applyAsInt(c2));
}

/************************************/
// 关心拿什么去比较
String::length
```

``` Java
/**
 * 前置符合条件的元素
 */
private static <T> List<T> prepositionSelectedElement(List<T> list, Predicate<T> predicate) {

    List<T> rs = list.stream().filter(predicate::test).collect(Collectors.toList());

    List<T> rest = list.stream().filter(i -> !predicate.test(i)).collect(Collectors.toList());
  
    // 这也里也可以用list.removeAll(), 但是就会改变list
    // 函数最好做到 side effect free
  
    rs.addAll(rest);

    return rs;
}
```

# Stream

``` Java
List<Long> list1 = Arrays.asList(1L ,2L, 3L);
List<Long> list2 = Arrays.asList(4L ,5L, 6L);

String str1 = JSON.toJSONString(list1);
String str2 = JSON.toJSONString(list2);

List<String> jsonStrings = Arrays.asList(str1, str2);

```


``` Java
// 
List<Long> rs = jsonStrings.stream()
            .filter(StringUtils::isNotBlank)
            .map(jsonStr -> JSON.parseArray(jsonStr, Long.class))
            .filter(CollectionUtils::isNotEmpty)
            .flatMap(Collection::stream)
            .collect(Collectors.toList());
            

            
```


## Predicate

返回值为 boolean 的 Function

``` Java
Stream<T> filter(Predicate<? super T> predicate);
```

``` Java
@FunctionalInterface
public interface Predicate<T> {
    
    boolean test(T t);

}
```

``` Java
// 判断妆品分数是不是达到 80 分的 predicate
Predicate<DresserItemCollocationDTO> ratePredicate = dto ->
    dto.getScore() > DRESSER_PROBLEM_SCORE_RULES.MATCH_RATE_BASELINE;
                
if (ratePredicate.test(recommendDTO)) {}

```

## collect

``` Java
List<Person> olderThan20 = people.stream()
            .filter(person -> person.getAge() > 20)
            .collect(ArrayList::new, ArrayList::add, ArrayList::addAll);
```

``` Java
@Override
public final <R> R collect(Supplier<R> supplier,
                           BiConsumer<R, ? super P_OUT> accumulator,
                           BiConsumer<R, R> combiner) {

    return evaluate(ReduceOps.makeRef(supplier, accumulator, combiner));
}

// Supplier: 返回结果的容器
// Accumulator: 添加元素到容器中
// combiner: 把多个结果容器merge到一起 （多线程的时候会用到）

public static <T> Collector<T, ?, List<T>> toList() {
    return new CollectorImpl<>((Supplier<List<T>>) ArrayList::new, List::add,
                               (left, right) -> { left.addAll(right); return left; },
                               CH_ID);
}
```

# Lazy Evaluation 

# Intermediate and terminal 
Streams:
1. intermediate (filter,map)
2. terminal (terminal)

``` Java
public static void main(final String[] args) {
    
    List<String> names = Arrays.asList("Brad", "Kate", "Kim", "Jack", "Joe",
        "Mike", "Susan", "George", "Robert", "Julia", "Parker", "Benson");

    final String firstNameWith3Letters = names.stream()
             .filter(name -> length(name) == 3)
             .map(name -> toUpper(name)) 
             .findFirst()    // terminal
             .get();
    
}
```

fusing operation:
all the functions in the intermediate operations are fused together into one function that is evaluated for each element, as appropriate, until the terminal operation is satisfied.
所有的中间操作的会被合并成一个函数，然后对每个元素进行计算。只要终止操作被满足，整个流计算就马上会退出。

# infinite Stream

``` Java
public class Primes {
    private static int primeAfter(final int number) {
        if(isPrime(number + 1)) 
            return number + 1;
        else
            return primeAfter(number + 1);
    }
    
    public static List<Integer> primes(final int fromNumber, final int count){ 
        return Stream.iterate(primeAfter(fromNumber - 1), Primes::primeAfter)
                     .limit(count)
                     .collect(Collectors.<Integer>toList());
    }
    //...
}
```

# Singleton Pattern

``` Java
// 大对象
public class Heavy {
    
    public Heavy() { 
        System.out.println("Heavy created"); 
    }
    
    public String toString() { 
        return "quite heavy"; 
    } 
}
```


``` Java
public class HolderNaive { 

    // 只有被用到的时候才会被赋值
    private Heavy heavy;
    
    public HolderNaive() { 
        System.out.println("Holder created");
    }

    public Heavy getHeavy() { 
        
        if(heavy == null) {
            heavy = new Heavy(); 
        }
        
        return heavy; 
    }

    //...
}

// 这个 Getter 在多线程的情况下，可能出现不同的线程会 new 不同的实例的情形
```


``` Java
// synchronized
public synchronized Heavy getHeavy() { 
    
    if(heavy == null) {
        heavy = new Heavy(); 
    }
    
    return heavy; 
}

// Synchronized 锁住了 Holder 对象的实例，不同的实例之间的锁互不影响
// 这里问题是当 Heavy 实例new好了以后，其实就不用加锁了，可以提供给多个线程同时调用了。
// 这样的结果是，Holder 实例的getHeavy方法，还是同时只能被一个线程调用。

```

``` Java 
// Double-checked Locking
public Heavy getHeavy() { 
    
    if(heavy == null) {
        // 假设两个线程T1，T2都进来了
        synchronized (this) { 
            // T2 进来的时候check T1 new 的对象，不会再 new 了
            if(heavy == null) {
                // T1 先 new 了对象
                heavy = new Heavy(); 
            }
        }

    }
    
    return heavy; 
}

```

``` Java

public class Holder {

    private Supplier<Heavy> heavySupplier = () -> createAndCacheHeavy();

    public Holder() { 
        System.out.println("Holder created");
    }

    public Heavy getHeavy() {
        return heavySupplier.get();
    }

    private synchronized Heavy createAndCacheHeavy() { 

        class HeavyFactory implements Supplier<Heavy> {
            
            private final Heavy heavyInstance = new Heavy(); 
            
            public Heavy get() { 
                return heavyInstance; 
            }
        }
        
        // 此时 heavySupplier = null
        if(!HeavyFactory.class.isInstance(heavySupplier)) { 
            // 给 heavySupplier 赋值一个 HeavyFactory 的实例
            heavySupplier = new HeavyFactory();
        }

        return heavySupplier.get(); 
    }
}

// heavySupplier 被赋值了一个 HeavyFactory 实例以后，后面的线程访问 Holder的 Getter 方法的时候，会直接调用 HeavyFactory的实例的 get 方法，把既有的 heavyInstance 返回，这里已经不会进入 synchronized 方法了，也不需要再做任何判断了。

```



 



