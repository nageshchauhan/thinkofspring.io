[Back to Spring Core](README.md)

## Initializing collection

Java provides mainly three types of collection ie List, Set and Map. Spring supports all these types of collection.

Suppose a class contains collection type of variable, so how can we define collection type to mapped into provided variable.

### Defining List in xml

Suppose our triangle class looks like this:

```java
package com.tos.bean;
import java.util.List;
public class Triangle{
    private List<Point> points;
    
    public List<Point> getPoints() {
		return points;
	}

	public void setPoints(List<Point> points) {
		this.points = points;
	}

	public void draw(){
		System.out.println("Three points of Triangle are");
		for(Point point:points){
			System.out.println("Point ("+point.getX()+","+point.getY()+")");
		}
	}
}
```

The way to define **List** in xml is as follows:

```xml
<bean id="triangle" class="com.tos.bean.Triangle">
	<property name="points">
		<list>
			<ref bean="pointA"/>
			<ref bean="pointB"/>
			<ref bean="pointC"/>
		</list>
	</property>
</bean>

<bean id="pointA" class="com.tos.bean.Point">
	<property name="x" value="0"></property>
	<property name="y" value="0"></property>
</bean>

<bean id="pointB" class="com.tos.bean.Point">
	<property name="x" value="90"></property>
	<property name="y" value="4"></property>
</bean>

<bean id="pointC" class="com.tos.bean.Point">
	<property name="x" value="60"></property>
	<property name="y" value="10"></property>
</bean>
```

Client file to get bean:

```java
ApplicationContext context = new ClassPathXmlApplicationContext("spring.xml");
Triangle triangle = (Triangle) context.getBean("triangle");
triangle.draw();
```

Output would be:

```java
Three points of Triangle are
Point (0,0)
Point (90,4)
Point (60,10)
```

### Defining Set in xml

Suppose our triangle class looks like this:

```java
package com.tos.bean;
import java.util.Set;

public class Triangle{
	
	private Set<Point> points;
	
	public Set<Point> getPoints() {
		return points;
	}

	public void setPoints(Set<Point> points) {
		this.points = points;
	}

	public void draw(){
		System.out.println("Three points of Triangle are");
		for(Point point:points){
			System.out.println("Point ("+point.getX()+","+point.getY()+")");
		}
	}

}
```

The way to define **Set** in xml is as follows:

```xml
<bean id="triangle" class="com.tos.bean.Triangle">
	<property name="points">
		<set>
			<ref bean="pointA"/>
			<ref bean="pointB"/>
			<ref bean="pointC"/>
		</set>
	</property>
</bean>

<bean id="pointA" class="com.tos.bean.Point">
	<property name="x" value="0"></property>
	<property name="y" value="0"></property>
</bean>

<bean id="pointB" class="com.tos.bean.Point">
	<property name="x" value="90"></property>
	<property name="y" value="4"></property>
</bean>

<bean id="pointC" class="com.tos.bean.Point">
	<property name="x" value="60"></property>
	<property name="y" value="10"></property>
</bean>
```

Client file to get bean:

```java
ApplicationContext context = new ClassPathXmlApplicationContext("spring.xml");
Triangle triangle = (Triangle) context.getBean("triangle");
triangle.draw();
```

Output would be:

```java
Three points of Triangle are
Point (0,0)
Point (90,4)
Point (60,10)
```

Java Set can accept list type of xml configuration but it will store only unique values. Similarly Java List can accept set type of xml configuration.

**Note:** If we provide list type of configuration to a Set member variable then duplicate entries will be removed while adding elements to java Set.

Similarly if we provide Set type of configuration to a List member variable then at the time of initialization the xml configuration will return unique elements.


### Defining Map in xml

```java
package com.tos.bean;
import java.util.Map;

public class Triangle{
	
	private Map<String, Point> points;
	
	public Map<String, Point> getPoints() {
		return points;
	}

	public void setPoints(Map<String, Point> points) {
		this.points = points;
	}

	public void draw(){
		System.out.println("Three points of Triangle are");
		for(Map.Entry<String, Point> entry : points.entrySet()){
			System.out.println("Point "+ entry.getKey()+" ("+entry.getValue().getX()+","+entry.getValue().getY()+")");
		}
	}
}
```

The way to define **Map** in xml is as follows:

```xml
<bean id="triangle" class="com.tos.bean.Triangle">
	<property name="points">
		<map>
			<entry key="A" value-ref="pointA"></entry>
			<entry key="B" value-ref="pointB"></entry>
			<entry key="C" value-ref="pointC"></entry>
		</map>
	</property>
</bean>

<bean id="pointA" class="com.tos.bean.Point">
	<property name="x" value="0"></property>
	<property name="y" value="0"></property>
</bean>

<bean id="pointB" class="com.tos.bean.Point">
	<property name="x" value="90"></property>
	<property name="y" value="4"></property>
</bean>

<bean id="pointC" class="com.tos.bean.Point">
	<property name="x" value="60"></property>
	<property name="y" value="10"></property>
</bean>
```

Client file to get bean:

```java
ApplicationContext context = new ClassPathXmlApplicationContext("spring.xml");
Triangle triangle = (Triangle) context.getBean("triangle");
triangle.draw();
```

Output would be:

```java
Three points of Triangle are
Point A (0,0)
Point B (90,4)
Point C (60,10)
```

<br>
[<-- Back to Inner beans and Alias](6_inner_bean_and_alias.md) &nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp; [Next: Bean Autowiring -->](8_bean_autowiring.md)
<br>