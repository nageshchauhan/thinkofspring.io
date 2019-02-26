[Back to Spring Core] (README.md)

## ApplicationContext and Property initialization


### ApplicationContext

The ApplicationContext container works same as BeanFactory container in terms of performance, functionality and other features. But the main advantage of using ApplicationContext is, it has more features than BeanFactory. Those features are like Internationalization(i18n) support, Event notification, AOP etc.


Example:

#### DrawingApp.xml

```xml
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <bean id="triangle" class="com.tos.bean.Triangle"></bean>
</beans>    
```

#### Triangle.java

```java
package com.tos.bean;
public class Triangle{
    public void draw(){
        System.out.println("Triangle drawn");
    }
}
```
#### DrawingShape.java

```java 
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class DrawingShape {

	public static void main(String[] args) {

        ApplicationContext context = new ClassPathXmlApplicationContext("DrawingApp.xml");
		Triangle triangle = (Triangle) context.getBean("triangle");
		triangle.draw();
	}
}
```

Output:

```java
Triangle drawn
```

In the above example, we have used `ClassPathXmlApplicationContext`  representation of `ApplicationContext`.

Working of `BeanFactory` and `ApplicationContext` is totally same because `ApplicationContext` extends `BeanFactory` so it provide all the function which is there in `BeanFactory`.


### Instantiating object with member variable

As shown in above Triangle class, we are introducing one member variable as `type` and also generating its getter and setter, it will look like this

```java
package com.tos.bean;
public class Triangle{
	private String type;
	
	public String getType() {
		return type;
	}
	public void setType(String type) {
		this.type = type;
	}
	public void draw(){
		System.out.println(getType()+" Triangle drawn");
	}
}
```

The changes which we need to perform in xml configuration is like this:

```xml
<bean id="triangle" class="com.tos.bean.Triangle">
    <property name="type" value="Equilateral"/>
</bean>
```

Here we have introduced another tag called property which has to attribute. The name attribute has the name of the variable declared in Triangle class, whereas the value attribute provide value for it.

So wherever we try to get a bean of type Triangle, it will instantiate triangle object and set the type value with specified in xml config and give back to requester.

**Note:** Here the xml file provide value to the `type` variable by using setter of that field ie to provide value for an object through setter we have to use property tag in bean definition.

If we get bean of triangle class through ApplicationContext and call `draw()` method of it then the output will be

```java
Equilateral Triangle drawn
```

**Note:** We can set any number of property through xml configuration.