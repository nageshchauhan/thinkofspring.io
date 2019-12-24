[Back to Spring Core](README.md)

## Bean Autowiring

Autowiring is a feature provided by Spring framework that helps in reduction of configuration in xml. 

There are 5 modes of autowiring spring provides:<br>
1. no <br>
2. byName <br>
3. byType <br>
4. autowiring by constructor  <br>
5. autodetect <br>


### 1. no autowiring
This option is default in spring framework and it means that autowiring is Off. We have to explicitly set the dependencies using `<property>` tag in bean definition.

### 2. Autowiring byName
This option enables the dependency injection based on bean names. When autowiring a property in bean, property name is used for searching a matching bean definition in configuration file. If such bean is found, it is injected in property. If no such bean is found, a error is raised.

#### Example: 

**Point.java**

```java
package com.tos.spring;

public class Point {

	private int x;
	private int y;
	//omitted getters and setters of x and y
}
```

**Triangle.java**

```java
package com.tos.spring;

public class Triangle{

	private Point pointA;
	private Point pointB;
	private Point pointC;
    //omitted getters and setters of pointA, pointB and pointC

    public void draw(){
        System.out.println("Drawing triangle with points:");
		System.out.println("pointA("+this.getPointA().getX()+","+this.getPointA().getY()+")");
		System.out.println("pointB("+this.getPointB().getX()+","+this.getPointB().getY()+")");
		System.out.println("pointC("+this.getPointC().getX()+","+this.getPointC().getY()+")");
    }
}
```

**bean.xml**

```xml
...
<beans>
    <bean id="triangle" class="com.tos.spring.Triangle" autowire="byName">
    </bean>
	
    <bean id="pointA" class="com.tos.spring.Point">
    	<property name="x" value="0"></property>
    	<property name="y" value="0"></property>
    </bean>
	
    <bean id="pointB" class="com.tos.spring.Point">
    	<property name="x" value="90"></property>
    	<property name="y" value="4"></property>
    </bean>
    	
    <bean id="pointC" class="com.tos.spring.Point">
    	<property name="x" value="60"></property>
    	<property name="y" value="10"></property>
    </bean>
</beans>
...
```
Client file for fetching bean

```java
package com.tos.spring;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class DrawingShape {

    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
        Triangle triangle = (Triangle) context.getBean("triangle");
        triangle.draw();
    }
}
```

**Output:**

```java
Drawing triangle with points:
pointA(0,0)
pointB(90,4)
pointC(60,10)
```

In the above example, Triangle class have three member variables and in the configuration file, we have defined bean with named triangle, which doesn't have any property in config. But the triangle bean have an attribute called autowire whose value is byName.

Basically autowire byName will search for the bean name matching with the member variable in java. Once it find, it used to map that bean with the corresponding member variable.

In case it fails to find the bean with that name then it will assign default value to that member variable.


### 3. Autowiring byType

This option enables the dependency injection based on bean types. When autowiring a property in bean, property’s class type is used for searching a matching bean definition in configuration file. If such bean is found, it is injected in property.

Consider the above example, if we replace `autowire="byName"` with `autowire="byType"` in bean definition of triangle, then spring will try to search the matching bean type with class name. But in our case there are three beans of type Point.

So while initializing bean it will throw exception saying expected one bean of type Point class but found several (print several bean name). **It is a thumb rule that if we use autowire by type then there must be only one bean of that type.**

If we comment any of two bean definition from xml file then it will assign same bean to all three member variables (pointA, pointB, pointC) of Triangle class.

### 4. Autowiring by constructor

Autowiring by constructor is similar to byType, but applies to constructor arguments. In autowire enabled bean, it will look for class type of constructor arguments, and then do a autowire bytype on all constructor arguments. Please note that if there isn’t exactly one bean of the constructor argument type in the container, a fatal error is raised.

