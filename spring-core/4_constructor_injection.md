[Back to Spring Core] (README.md)

## Constructor injection


In the previous section we had initialized object using property tag for its member variable. The property tag uses setter of that variable to assign value. Here we are going to create constructor and assign values to member variable by using constructor.


```java
package com.tos.bean;
public class Triangle{
	private String type;
	
	public Triangle(String type){
		System.out.println("Constructor of triangle called");
		this.type=type;
	}
	
	public String getType() {
		return type;
	}

	public void setType(String type) {
		System.out.println("setter of type called");
		this.type = type;
	}
	
	public void draw(){
		System.out.println(getType()+" Triangle drawn");
	}
}
```

Here, we have created constructor with parameter type as String. The getter of type field and constructor contains syso statement, which will tell us which line got executed during initialization of triangle class.

In the xml file, instead of using property tag of bean, we will use constructor-arg tag.

```xml
<bean id="triangle" class="com.tos.bean.Triangle">
    <constructor-arg value="Equilateral" />
</bean>
```

**Note:** Here we do not need to provide name attribute, since constructor accepts only one parameter.

Now if we get bean in our client application and run the `draw()` method then the output would be:

```java
Constructor of triangle called
Equilateral Triangle drawn
```

Suppose, we are providing value of type member variable of class Triangle by setter and constructor both way. Then which value the `draw()` method is going to refer?

```xml
<bean id="triangle" class="com.tos.bean.Triangle">
    <property name="type" value="Isosceles"/>
    <constructor-arg value="Equilateral" />
</bean>
```

The output would be:

```java
Constructor of triangle called
setter of type called
Isosceles Triangle drawn
```
Because the constructor call is first followed by setter call. So the setter statement will override the value.


Since in this example we have only one member variable, let's introduce another variable `height` of type `int` and generate its getter setter and update the constructor to accept height as well. The triangle class will look like this:

```java
package com.tos.bean;
public class Triangle{
	private String type;
	private int height;
	
	public Triangle(String type, int height){
		this.type=type;
		this.height = height;
	}
	
	public void draw(){
		System.out.println(getType()+" Triangle drawn of height "+getHeight());
	}
	//getters and setters are omitted
}
```

Updating the xml configuration to pass one more constructor value

```xml
<bean id="triangle" class="com.tos.bean.Triangle">
    <constructor-arg value="Equilateral" />
    <constructor-arg value="20" />
</bean>
```

Now when we execute our client then the spring is going to search for the constructor which accept two argument in Triangle class and instantiate the bean. In this case the output would be:

```java
Equilateral Triangle drawn of height 20
```

As you can notice in the constructor-arg statement in xml configuration, the value provided for both argument are within double quote `""`. In java values provided in double quote is refer as String. The spring intelligently converts the values into required type. But sometime it leads to a potential problem. For example if we reverse the order of constructor-arg in xml file like

```xml
<bean id="triangle" class="com.tos.bean.Triangle">
    <constructor-arg value="20" />
    <constructor-arg value="Equilateral" />
</bean>
```
The spring is going to assign value `20` to type variable and `Equilateral` to height variable, but the height variable is of type int, so it will try to convert `Equilateral` String to int, and it will fail by throwing `NumberFormatException`.

So here we have to use another attribute called `type` of constructor-arg field.
The type field accept which type of given value. In our case, value 20 is type int whereas value `Equilateral` is of type String. After re-writing the xml configuration with type attribute looks like:

```xml
<bean id="triangle" class="com.tos.bean.Triangle">
    <constructor-arg type="int" value="20" />
    <constructor-arg type="java.lang.String" value="Equilateral" />
</bean>
```

When we get this bean, then spring will try to search for constructor with int and string or vice versa because we have not defined the order of parameters. In this case the output would be:

```java
Equilateral Triangle drawn of height 20
```

**Note:** For primitive type, its name is sufficient whereas for non-primitive type we have to provide fully qualified name.


Suppose the xml configuration is same as above and the Triangle class we have added one more constructor with String and int type as shown below:

```java
package com.tos.bean;
public class Triangle{
	private String type;
	private int height;
	
	public Triangle(String type, int height){
		System.out.println("string,int constructor called");
		this.type=type;
		this.height = height;
	}
	public Triangle(int height, String type){
		System.out.println("int,string constructor called");
		this.type=type;
		this.height = height;
	}
	
	public void draw(){
		System.out.println(getType()+" Triangle drawn of height "+getHeight());
	}
	//Getters and Setters are omitted.
}
```

Here two constructors are present, if we try to get triangle bean as per above configuration in xml then which constructor will get called ?
And the answer is first constructor, because spring will search for constructor which accept int and String type argument irrespective of parameters order. The execution will start from top so first constructor will be eligible. The output would be:

```java
string,int constructor called
Equilateral Triangle drawn of height 20
```

To overcome this confusion, its better to provide the order of parameter in xml configuration by using `index` attribute in `constructor-arg` tag.
After updating with index attribute the xml will look like:

```xml
<bean id="triangle" class="com.tos.bean.Triangle">
    <constructor-arg type="int" value="20" index="0" />
    <constructor-arg type="java.lang.String" value="Equilateral" index="1" />
</bean>
```

Now if we try to get bean then the spring will search for constructor which accept int and String type argument and the second constructor would be eligible for this. So the output would be:

```java
int,string constructor called
Equilateral Triangle drawn of height 20
```
**Note:** The index number start from 0.