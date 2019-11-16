---
layout:     post
title:      "Java函数式编程——Lambda表达式（一）"
date:       2019-11-07
author:     "林柯秀"
header-img: "img/post-bg-2015.jpg"
tags:
    - 函数式编程
    - Java
---

# Java函数式编程——Lambda表达式（一）

## 1. 什么是函数式编程(Functional Programming)？

> 面向对象编程(OOP)通过封装变化使得代码更易理解。
> 函数式编程(FP)通过最小化变化使得代码更易理解。
> -- Michacel Feathers（Twitter） 

### 1.1 函数式编程的特点

我们先来看一段 Wikipedia 上对函数式编程的描述：

> In computer science, functional progrmming is a **programming paradigm** that treats computation as the evaluation of **mathematical functions** and avoid **changing-state** and **mutable data.**

短短一段话包含许多的重点，我将逐一做出简要阐述。

- **函数式编程是一种编程范式。** 一些语言是专门为某个特定的范式设计的（如Smalltalk和Java支持面向对象编程，而Haskell和Scheme则支持函数式编程），而另一些语言则支持多种范式（如Ruby、Common Lisp、Python）。
- **函数式编程的抽象程度很高。** 这很好理解，越是抽象，越是贴近数学计算。函数式编程的思想和数学意义上的函数的思想有异曲同工之妙。函数简单来说就是一个从定义域到值域的映射过程，而函数式编程实质上也是一个从入参到返回值的黑盒子。即函数式编程属于声明式编程， 这种范式会描述一系列的操作，但并不会暴露它们是如何实现的或是数据流如何传过它们。 
- **函数式编程没有副作用。** 对于任意函数，只要输入是确定的，输出就是确定的，这种纯函数被称为没有副作用，我们也可以说它是 **引用透明** 的。函数式编程想要避免状态变化和可变数据，就需要是没有副作用的。而允许使用变量的函数，由于内部变量状态的不确定，确定的输入可能得到不同的输出，这样的函数就是有副作用的。
	- 需要注意的是， 这里的“没有副作用”是指没有可观测到的副作用。 实际上函数不可能完全没有副作用，函数在执行时可能会发生内存耗尽或者堆栈溢出的错误，进而导致应用程序奔溃，正在某种意义上就是一个可观测到的副作用。或许，可以说函数式编程 **没有故意的副作用**。

### 1.2 函数式编程示例

光说不练是要不得的，接下来通过下面的几个非常简单Java程序来说明什么是函数式编程。

```java
public static int add(int a, int b){
    while(b>0){
        a++;
        b--;
    }
    return a;
}
```

这段代码遵循的就是函数式编程的范式，虽然有可能会发生溢出等错误，但这并不影响它是函数式的。

```java
public static void add(int a, int b){
    while(b>0){
        a++;
        b--;
    }
    System.out.println(a);
}
```

也许会有人认为这段代码是函数式的，但它并不是。这就牵涉到常见的副作用有哪些的问题，可以列举部分：

- 改变一个全局的变量、属性或数据结构
- 改变一个函数参数的原始值
- 处理用户输入
- 抛出一个异常
- 屏幕打印或记录日志
- 查询 HTML 文档，浏览器的 Cookie 或访问数据库

```java
public static int div(int a, int b){
    return a/b;
}
```

这个代码乍一看是符合函数式编程范式的，但它在 $b=0$ 时会抛出异常，可见它并不是函数式的。该代码可以通过简单的类型转换称为函数式的，大家感兴趣也可以思考一下。

### 1.3 函数式编程的优点

- 函数式程序具有确定性，易于推断。
- 函数式程序更加易于测试。
- 函数式程序更加模块化。
- 函数式程序总是线程安全的。
- 函数式程序是惰性求值的。

## 2 Java中的Lambda表达式

### 2.1 Java 8

Java 8是在2014年3月发布的，这次升级让Java有了许多新的特性。其中最主要的为以下三个：

- Lambda（匿名函数）
- 流
- 默认方法

在Java 8之前，开发类库的程序员时常认为Java的抽象级别还不够。例如在面对大型数据集合时，Java欠缺高效的并行操作。而使用Java 8的新特性使得在多核CPU上高效运行复杂的集合处理算法成为可能。

> From a slightly revisionist viewpoint, the addition of Streams in Java 8 can be seen as a direct cause of the two other additions to Java 8: concise techniques to pass code to methods (method references, lambdas) and default methods in interfaces.
>
> <p align="right">--- <i>Java 8 in Action</i></p> 

当然以上这些不全是本文讨论的重点，在这篇文章里我们只需要知道：为了编写这类处理批量数据的并行类库，Java在语言层面所需的修改就是：增加Lambda表达式。

### 2.2 Lambda表达式

> Functional programming has its origins in lambda calculus.

 在进入主题之前，我们先要看一段注册事件监听器的代码。

```java
button.addActionListener(new ActionListener(){
    public void actionPerformed(ActionEvent event){
        System.out.println("button clicked");
    }
});
```

这段代码实现了`ActionListener`接口，这个接口只有一个方法`actionPerformed`。我们使用了匿名内部类来实现该方法。设计匿名内部类的目的就是方便Java程序员将代码作为数据进行传递。但是匿名内部类有一些显而易见的缺点：样板代码造成程序的冗长，降低了代码的易读性，不能清楚表现程序的意图。比如以上面这段代码为例，我们想要传入的是一个行为，但是我们实际传入的却是一个对象。

如果我们使用Lambda表达式来完成同样的功能，我们需要怎样编写代码呢？答案如下：

````java
button.addActionListener(event -> System.out.println("button clicked"));
````

学过R语言的同学会发现这和R语言的语法结构非常相似。和传入一个对象不同，以上代码传入的是一段代码块——一个没有名字的函数。`event`是参数名，`->` 将参数和Lambda表达式的主体分开。

我们可以由此总结出Lambda表达式的三个部分：

- 参数列表——对比两段代码，匿名内部类需要显式地声明参数类型`ActionEvent event`，而在Lambda表达式中无需指定类型。这是因为`javac`根据程序的上下文（`addActionListener`方法的签名）在后台推断出了参数`event`的类型，这使得参数类型不言自明。当然，Java 8同样支持在声明参数时包括类型信息；
- 箭头——箭头`->`将参数列表与主体分离开；
- Lambda主体——表达式就是Lambda的返回值。

Lambda的基本语法是：`(parameters) -> expression`或`(parameters) -> {statements;}`

```java
//编写Lambda表达式的不同形式

Runnable noArguments = () -> System.out.println("Hello World");

ActionListener oneArguments = event -> System.out.println("button clicked");

Runnable multiStatement = () -> {
    System.out.print("Hello");
    System.out.println(" World");
}

BinaryOperator<Long> add = (x,y) -> x+y;

BinaryOperator<Long> addExplicit = (Long x, Long y) -> x+y;
```

### 2.3 函数式接口与Lambda表达式

函数式接口就是只定义了一个抽象方法的接口。例如：

```java
public interface Comparator<T>{
    int compare(T o1, T o2);
}
```

Lambda表达式大多的使用场景是在函数式接口中，因此也有这样的说法：“函数接口是只有一个抽象方法的接口，用作Lambda表达式的类型。”

Lambda表达式允许直接以内联的形式为函数式接口的抽象方法提供实现，并把整个表达式作为函数式接口的实例。

```java
Runnable r1 = () -> System.out.println("Hello World 1");

Runnable r2 = new Runnable(){
    public void run(){
        System.out.println("Hello World 2");
    }
}

public static void process(Runnable r){
    r.run();
}
process(r1); //打印“Hello World 1”
process(r2); //打印“Hello World 2”
process(() -> System.out.println("Hello World 3")); //直接传递Lambda作为实例
```



## 参考文献

1. *Java 8 in Action: Lambdas, Streams, and functional-style programming-Manning* by Raoul-Gabriel Urma, Mario Fusco, Alan Mycroft 
