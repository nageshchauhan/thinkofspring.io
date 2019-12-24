# Spring AOP

## Pointcuts and wildcard expression

As we know Spring only support method execution join points for spring beans. So followings are the pointcut expression which are matching the execution of method on Spring beans. A pointcut declaration has four parts:

1. Matching method signature pattern
2. Matching type signature pattern
3. Matching bean name pattern
4. Combining pointcut expression

### Supported Pointcut designator by Spring AOP

AspectJ framework support many designators but Spring AOP supports only some of them as below:

* **execution** - pointcut expression for matching method execution join points
* **within** - pointcut expression for matching to join points within certain types
* **this** - pointcut expression for matching to join points where the bean reference is an instance of the given type
* **target** - pointcut expression for matching to join points where the target object is an instance of the given type
* **args** - pointcut expression for matching to join points where the arguments are instances of the given types
* **@target** - pointcut expression for matching to join points where the class of the executing object has an annotation of the given type
* **@args** - pointcut expression for matching to join points where the runtime type of the actual arguments passed have annotation of the given type
* **@within** - pointcut expression for matching to join points within types that have the given annotation.
* **@annotation** - pointcut expression for matching to join points where the subject of the join point has the given annotation


### Declaring pointcut expression

### 1. Matching method signature patterns

* Matching all methods in `ShapeService` with 
`public` access specifier, 
return type as `void`, 
method name start with `get` and 
which accepts no argument.

```java
@Pointcut("execution(public void com.thinkofspring.service.ShapeService.get*())")
public void publicMethodOfShapeService(){}
```

* Matching all methods in `ShapeService` with 
`public` access specifier, 
return type as `void` and 
which accepts no argument.

```java
@Pointcut("execution(public void com.thinkofspring.service.ShapeService.*())")
public void publicMethodOfShapeService(){}
```

* Matching all methods in `ShapeService` with 
`public` access specifier, 
return type as `Object` and 
which accepts no argument.


```java
@Pointcut("execution(public Object com.thinkofspring.service.ShapeServce.*())")
public void publicMethodOfShapeService(){}

@Pointcut("execution(public java.lang.List com.thinkofspring.service.ShapeService.*())")
public void publicMethodOfShapeService(){}
```
**Note:** It is good practice to define return type with fully qualified name(e.g. package.name.ClassName) except for void. Because in pointcut expression pre-defined classes which do not need to import are acceptable but which needs to be import for that fully qualified name is mandatory.

* Matching all methods in `ShapeService` with
`public` access specifier
any return type and 
which accepts any parameter (no or any parameters)

```java
@Pointcut("execution(public * com.thinkofspring.service.ShapeServce.*(..))")
public void publicMethodWithAnyParamAnyReturnType(){}
```

* Matching any method defined in service package. first * indicate any return type, second any class under service package and third any method present in service package.

```java
@Pointcut("execution(* com.thinkofspring.service.*.*(..))")
public void anyMethodInServicePackage(){}
```

* Matching any method defined in the service package or it’s sub-packages. two dots after service package means it’s include sub-packages as well and * after two dots means any class.

```java
@Pointcut("execution(* com.thinkofspring.service..*.*(..))")
public void anyMethodInServicePackageAndSubPackage(){}
```

* Matching any draw method under service package which take one parameter of Shape

```java
@Pointcut("execution(* com.thinkofspring.service.*.draw(com.thinkofspring.model.Shape shape))")
public void anyDrawOfServicePackageWithShapeParameter(){}
```

* Matching any method under service package which accepts first parameter as `Shape` and no or any parameter.

```java
@Pointcut("execution(* com.thinkofspring.service.*.*(com.thinkofspring.model.Shape shape, ..))")
public void anyMethodOfServicePackageWithShapeAndAnyParameter(){}
```

Matching all methods which returns either Shape or its implemented/extended classes with any parameter.

```java
@Pointcut("execution(com.thinkofspring.model.Shape+ com.thinkofspring.service.*.*(..))")
public void allMethodWithShapeOrImplementedClass(){}
```

### 2. Matching type signature patterns

* Matching all methods defined in classes inside package `com.thinkofspring.service`
  
```java
@Pointcut("within(com.thinkofspring.service.*)")
public void allMethodInServicePackage(){}
```

**Note:** `execution(* com.thinkofspring.service.*.*(..))` is equivalent to `within(com.thinkofspring.service.*)`

* Matching all methods defined in classes inside package `com.thinkofspring.service` and its sub-packages. Two dots are for sub-packages.

```java
@Pointcut("within(com.thinkofspring.service..*)")
public void allMethodsInServicePackageAndSubPackages();
```

* Matching all methods defined in ShapeService class.

```java
@Pointcut("within(com.thinkofspring.service.ShapeService)")
public void allMethodOfShapeService(){}
```

* Matching all methods within all implementing classes of ShapeService interface. Use + (plus) sign to match all implementations of an interface

```java
@Pointcut("within(com.thinkofspring.service.ShapeService+)")
public void public void allMethodsOfShapeServiceImpl();
```

* Matching all methods where the proxy implements the ShapeService interface

```java
@Pointcut("this(com.thinkofspring.service.ShapeService)")
public void allMethodsProxyImplmentShapeService();
```

* Matching all methods where the target object implements the ShapeService interface

```java
@Pointcut("target(com.doj.app.service.ShapeService)")
public void allMethodsTargetObjectImplmentShapeService();
```

### 3. Matching bean name patterns

Matching Bean Name Patterns is only supported in Spring AOP – and not in native AspectJ weaving.

* Matching all methods on a Spring bean named shapeService

```java
@Pointcut("bean(shapeService)")
public void allMethodsOfShapeServiceBean();
```

* Matching all methods on Spring beans having names that match the wildcard expression *Service

```java
@Pointcut("bean(*Service)")
public void allMethodsOfBeanNameAsAnyService();
```

### 4. Combining pointcut expressions

Pointcut expressions can be combined using ‘&&’, ‘||’ and ‘!’

Matching any public methods within service module

```java
@Pointcut("execution(public * *(..))")
private void anyPublicOperation() {}

@Pointcut("within(com.thinkofspring.service..*)")
private void inService() {}

@Pointcut("anyPublicOperation() && inService()")
private void serviceOperation() {}
```

[#Imp] **What is the difference between concern and cross-cutting concern in Spring AOP?** <br>
**Concern is behavior which we want to have in a module of an application.** Concern may be defined as a functionality we want to implement to solve a specific business problem. E.g. in any eCommerce application different concerns (or modules) may be inventory management, shipping management, user management etc.

**Cross-cutting concern is a concern which is applicable throughout the application (or more than one module).** e.g. logging , security and data transfer are the concerns which are needed in almost every module of an application, hence they are termed as cross-cutting concerns.