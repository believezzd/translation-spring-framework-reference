##### Inner beans

在<property/>或<constructor-arg/>元素中的bean元素可以叫做内部bean。

```
<bean id="outer" class="...">
<!-- instead of using a reference to a target bean, simply define the target bean inline -->
    <property name="target">
        <bean class="com.example.Person"> <!-- this is the inner bean -->
            <property name="name" value="Fiona Apple"/>
            <property name="age" value="25"/>
        </bean>
    </property>
</bean>
```

一个内部bean的定义不需要id或name，即使指定了，容器也不会使用它作为一个标识符。容器也会忽略scope的标志。内部bean总是匿名的而且和其他外部bean一起创建。内部bean不可能注入到一个组合的bean中，像其他的非内部的bean，而且也不能独立访问内部bean。

在极端的情况下，他可能会破话自定义范围的回调。例如，对于一个单例bean包含的request范围的bean，内部bean会作为容器bean的绑定来处理，但是破坏性回调允许他参与request范围的生命周期。这种情况并不常见，内部bean和他的容器bean共享同一个范围。