[Back to Spring Core] (README.md)

## Spring IoC Container


Spring container is collection of objects, and the IoC container is responsible for instantiate, configure and assemble the objects. The IoC container gets information from XML file. Spring container is responsible for managing the life cycle of object.

In our application we can create object manually and can be created through Spring IoC container. But the difference in creating object manually and through spring container is that, lifecycle of manually created objects are maintained by developer whereas lifecycle of spring container created objects are maintained by spring itself.<br><br>

There are two types of Spring IoC Container:

1. BeanFactory (`org.springframework.beans.factory.BeanFactory`)
2. ApplicationContext (`org.springframework.context.ApplicationContext`)

The BeanFactory is factory which is responsible for creating object based on the provided blueprint(configuration file) in xml file. So whenever client request for an object the BeanFactory looks into the configuration of requested object and instantiate based on the blueprint and returns back.

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
import org.springframework.beans.factory.BeanFactory;
import org.springframework.beans.factory.xml.XmlBeanFactory;
import org.springframework.core.io.FileSystemResource;

public class DrawingShape {

	public static void main(String[] args) {

		BeanFactory factory = new XmlBeanFactory(new FileSystemResource("DrawingApp.xml"));
		Triangle triangle = (Triangle) factory.getBean("triangle");
		triangle.draw();
	}
}
```

Output:

```java
Triangle drawn
```

In the above example, we have used `XmlBeanFactory`  representation of `BeanFactory`. It reads the configuration from specified file resource. Beans namespace is used here (in DrawingApp.xml file), is to support bean related tags. In xml file, we have declared a bean with id as triangle and the class name with using fully qualified name. So whenever in our application, if we request for bean triangle then Spring container will look for the configuration in the xml file and provide an object as per the specified configuration.

If we request a bean by using spring container then the lifecycle of such bean is maintained by Spring factory itself. The same bean we can manually create but such bean's lifecycle needs to be maintained manually by the programmer.

####Example:

```java
BeanFactory factory = new XmlBeanFactory(new FileSystemResource("DrawingApp.xml"));

Triangle triangle = (Triangle) factory.getBean("triangle");
triangle.draw();

Triangle myTriangle = new Triangle();
myTriangle.draw();
```

Here, two objects of triangle class present. The one provided by spring factory and another is created by programmer. Both will work as same in functionality wise, but `triangle` object is maintained by spring factory and `myTriangle` has to be maintained by programmer.

<br>
[<-- Back to Introduction](1_introduction.md) &nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp; [Next: ApplicationContext and Property initialization -->](3_app_context_and_prop_init.md)
<br>