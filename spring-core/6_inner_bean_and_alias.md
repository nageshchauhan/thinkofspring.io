[Back to Spring Core](README.md)

## Inner Beans and Alias

### Inner Beans

In the previous section, we injected another bean as a property in a bean. 

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

<bean id="triangle" class="com.tos.bean.Triangle">
	<property name="pointA" ref="pointA" />
	<property name="pointB" ref="pointB" />
	<property name="pointC" ref="pointC" />
</bean>
```
Here we will check how can we define inner bean. In the previous section we had created separate point beans and in the triangle bean, those beans were getting referred. If the point bean is no where getting referred then we can specify that point bean in Triangle bean itself as follows:

```xml
<bean id="triangle" class="com.tos.bean.Triangle">
	<property name="pointA">
       <bean class="com.tos.bean.Point">
        	<property name="x" value="0" />
        	<property name="y" value="0" />
    	</bean>
   </property>
   
	<property name="pointB">
        <bean class="com.tos.bean.Point">
        	<property name="x" value="90" />
        	<property name="y" value="4" />
        </bean>
	</property>
	<property name="pointC">
        <beanclass="com.tos.bean.Point">
            <property name="x" value="60" />
            <property name="y" value="10" />
        </bean>
	</property>
</bean>
```

These two way of creating bean will result in same object with same values. This is called Inner bean.

### Alias

There are mainly two way to provide alias for a bean.

#### Method: 1

```xml
<bean id="triangle" class="com.tos.bean.Triangle"></bean>
<alias name="triangle" alias="triangle-alias"/>
```

Here we have used alias tag to define alias for triangle bean. So in our application, instead of using `triangle` bean id, we can ref alias name to get bean. We can define any number of alias for given bean.

#### Method: 2

```xml
<bean id="zeroPoint" class="com.tos.spring.Point" name="zero-point"></bean>
```

Here `name="zero-point"` is an alias name provided at the time of bean definition.

**Note:** We can refer alias name to get bean or it can be referred at the time of bean injection.

<br>
[<-- Back to Injecting Object](5_injecting_object.md) &nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp; [Next: Initializing collection -->](7_initializing_collection.md)
<br>