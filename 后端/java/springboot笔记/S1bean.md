# bean的学习
[spring bean是什么](https://www.awaimai.com/2596.html)

## 1 定义
Spring 官方文档对 bean 的解释是：

```
In Spring, the objects that form the backbone of your application and that are managed by the Spring IoC container are called beans. A bean is an object that is instantiated, assembled, and otherwise managed by a Spring IoC container.
```


翻译过来就是：
```
在 Spring 中，构成应用程序主干并由Spring IoC容器管理的对象称为bean。bean是一个由Spring IoC容器实例化、组装和管理的对象。
```
概念简单明了，我们提取处关键的信息：

 1. bean是对象，一个或者多个不限定
 2. bean由Spring中一个叫IoC的东西管理
 3. 我们的应用程序由一个个bean构成  
 
第1和3好理解，那么IoC又是什么东西？

## 2 控制反转（IoC）
控制反转英文全称：* Inversion of Control *，简称就是IoC。

控制反转通过依赖注入（DI）方式实现对象之间的松耦合关系。

程序运行时，依赖对象由【辅助程序】动态生成并注入到被依赖对象中，动态绑定两者的使用关系。

Spring IoC容器就是这样的辅助程序，它负责对象的生成和依赖的注入，让后在交由我们使用。

简而言之，就是：IoC就是一个对象定义其依赖关系而不创建它们的过程。

这里我们可以细分为两个点。

### 2.1 私有属性保存依赖
第1点：使用私有属性保存依赖对象，并且只能通过构造函数参数传入，

构造函数的参数可以是工厂方法、保存类对象的属性、或者是工厂方法返回值。

假设我们有一个Computer类：

```java
public class Computer {
    private String cpu;     // CPU型号
    private int ram;        // RAM大小，单位GB

    public Computer(String cpu, int ram) {
        this.cpu = cpu;
        this.ram = ram;
    }
}
```
我们有另一个Person类依赖于Computer类，符合IoC的做法是这样：

```java

public class Person {
    private Computer computer;

    public Person(Computer computer) {
        this.computer = computer;
    }
}
```
不符合IoC的做法如下：

```java

// 直接在Person里实例化Computer类
public class Person {
    private Computer computer = new Computer("AMD", 3);
}

// 通过【非构造函数】传入依赖
public class Person {
    private Computer computer;
    
    public void init(Computer computer) {
        this.computer = computer;
    }
```
### 2.2 让Spring控制类构建过程
第2点：不用new，让Spring控制new过程。

在Spring中，我们基本不需要 new 一个类，这些都是让 Spring 去做的。

Spring 启动时会把所需的类实例化成对象，如果需要依赖，则先实例化依赖，然后实例化当前类。

因为依赖必须通过构建函数传入，所以实例化时，当前类就会接收并保存所有依赖的对象。

这一步也就是所谓的依赖注入。

### 2.3 这就是IoC
在 Spring 中，类的实例化、依赖的实例化、依赖的传入都交由 Spring Bean 容器控制，

而不是用new方式实例化对象、通过非构造函数方法传入依赖等常规方式。

实质的控制权已经交由程序管理，而不是程序员管理，所以叫做控制反转。

## 3 Bean？
至于bean，则是几个概念。

概念1：Bean容器，或称spring ioc容器，主要用来管理对象和依赖，以及依赖的注入。
概念2：bean是一个Java对象，根据bean规范编写出来的类，并由bean容器生成的对象就是一个bean。
概念3：bean规范。


bean规范如下：

1. 所有属性为private
2. 提供默认构造方法
3. 提供getter和setter
4. 实现serializable接口