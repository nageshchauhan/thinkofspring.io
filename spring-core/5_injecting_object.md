[Back to Spring Core] (README.md)

## Injecting Object


In the previous section we had injected object using constructor and setter injection. The object which we injected were of primitive and String type.
In this section will try to inject object of a class type. Suppose we have class Point as follows:

```java
package com.tos.bean;
public class Point {

	private int x;
	private int y;

	public int getX() {
		return x;
	}
	public void setX(int x) {
		this.x = x;
	}

	public int getY() {
		return y;
	}
	public void setY(int y) {
		this.y = y;
	}
}
```

And in the triangle class, we will define three points and provide its getters and setters, also we will override the definition of draw() method as below.


```java
package com.tos.bean;
public class Triangle{
    private Point pointA;
	private Point pointB;
	private Point pointC;

	public Point getPointA() {
		return pointA;
	}

	public void setPointA(Point pointA) {
		this.pointA = pointA;
	}

	public Point getPointB() {
		return pointB;
	}

	public void setPointB(Point pointB) {
		this.pointB = pointB;
	}
	
	public Point getPointC() {
		return pointC;
	}

	public void setPointC(Point pointC) {
		this.pointC = pointC;
	}
	
	public void draw(){
		System.out.println("Point A ("+getPointA().getX()+","+getPointA().getY()+")");
		System.out.println("Point B ("+getPointB().getX()+","+getPointB().getY()+")");
		System.out.println("Point C ("+getPointC().getX()+","+getPointC().getY()+")");
	}
}
```

Here, to get triangle bean, we need to provide value for all points member variable otherwise we will get null value. So what we can do is, define all three point separately as a bean in xml as follows:

```xml
<bean id="pointA" class="com.tos.bean.Point">
	<property name="x" value="0" />
	<property name="y" value="0" />
</bean>
	
<bean id="pointB" class="com.tos.bean.Point">
	<property name="x" value="90" />
	<property name="y" value="4" />
</bean>
	
<bean id="pointC" class="com.tos.bean.Point">
	<property name="x" value="60" />
	<property name="y" value="10" />
</bean>
```

Now we can refer these beans in the blueprint of Triangle bean as follows:

```xml
<bean id="triangle" class="com.tos.bean.Triangle">
	<property name="pointA" ref="pointA" />
	<property name="pointB" ref="pointB" />
	<property name="pointC" ref="pointC" />
</bean>
```

Instead of using value field to provide actual value, here we are using `ref` attribute to refer another bean. This way we can reuse beans. If we try to get triangle bean in our client application like this 

```java
ApplicationContext context = new ClassPathXmlApplicationContext("DrawingApp.xml");
Triangle triangle = (Triangle) context.getBean("triangle");
triangle.draw();
```

The output of above code would be:

```java
Point A (0,0)
Point B (90,4)
Point C (60,10)
```

**Note:** Here we have nested beans only one level, similarly we can nest bean in any number of level.

<br>
[<-- Back to Constructor injection](4_constructor_injection.md) &nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp; [Next: Inner beans and Alias -->](6_inner_bean_and_alias.md)
<br>