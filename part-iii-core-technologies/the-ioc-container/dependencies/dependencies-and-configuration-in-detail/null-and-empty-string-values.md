spring处理空的参数和处理空的字符串是一样的。下面基于xml的配置元数据支持对email属性设置空的字符串（""）

```
<bean class="ExampleBean">
    <property name="email" value=""/>
</bean>
```

前面的例子和下面的java代码是一样的:

```
exampleBean.setEmail("")
```

<null/>元素用于处理null值，如下:

```
<bean class="ExampleBean">
    <property name="email">
        <null/>
    </property>
</bean>
```

前面的例子和下面的java代码是一样的:

```
exampleBean.setEmail(null)
```

