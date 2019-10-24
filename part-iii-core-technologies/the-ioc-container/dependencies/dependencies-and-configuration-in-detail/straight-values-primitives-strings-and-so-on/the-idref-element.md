idref元素提供了一个简单的错误检查的方式来传递id（字符串值，不是一个引用）到容器的<constructor-arg/>或<property/>元素中。

```
<bean id="theTargetBean" class="..."/>

<bean id="theClientBean" class="...">
    <property name="targetName">
        <idref bean="theTargetBean" />
    </property>
</bean>
```

上面的bean的定义在运行时和下面的片段没有什么区别。

```
<bean id="theTargetBean" class="..." />
<bean id="client" class="...">
    <property name="targetName" value="theTargetBean" />
</bean>
```

第一种形式比第二种形式要好，因为使用idref标签可以使得容器在开发时就检查注入的bean是否真实存在。在第二种方式中，没有为客户端的bean提供对值的校验机制。拼写错误只能在客户端bean被实际初始化时才被发现（可能会导致致命的问题）。如果客户端的bean是一个原型模式，这个拼写错误或许只能在容器已经被部署后才被发现。

>Note
>idref元素的local属性在4.0的beans xsd中不在被支持。升级到4.0时请改用idref bean属性。

（在spring2.0之前），idref元素的值对于在ProxyFactoryBean中AOP的拦截器的定义是有好处的。使用idref元素可以使得你在定义拦截器名的时候不会漏掉拦截器的id。