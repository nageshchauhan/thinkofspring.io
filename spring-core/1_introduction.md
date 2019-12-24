[Back to Spring Core](README.md)
# Spring Core

## Introduction

#### What is spring framework?<br>
It is a collection of frameworks put into one. Spring framework is famous for dependency injection.

#### What is Inversion of Control (IoC)?<br>
Inversion of Control is a principal in software engineering by which the control of the object is transferred to a container or framework.

#### Advantages of IoC
1. Decoupling the execution of a task from its implementation.
2. Making it easier to switch between different implementation.
3. Greater modularity of program.
4. Greater ease in testing a program by isolating a component or mocking its dependencies and allowing components to communicate through contracts.

IoC can be achieved through various mechanism such as Strategy Design Pattern, Service Locator Pattern, Factory Pattern and Dependency Injection(DI).

#### What is dependency Injection ?
Dependency Injection is a design pattern which implement IoC to resolve dependencies.

Example:

```java
public class Store{
    private Item item;
    public Store(){
        item = new ItemImpl();
    }
}
```

In the above example, we need to instantiate the `Item` interface within `Store` class itself. It indicates that instantiation of Store class is dependent on instantiation of `Item` interface. So the code is tightly coupled.

By using dependency injection, we can rewrite the example without specifying the implementation of `Item`

```java
public class Store{
    private Item item;
    public Store(Item item){
        this.item = item;
    }
}
```

<br>
[<-- Back to Spring Core](README.md) &nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp; [Next: Spring IoC Container -->](2_spring_ioc_container.md)
<br>