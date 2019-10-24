当你在设置bean属性时可以使用组合或内嵌的属性名，并且保证每个组件的属性名都是非空的。考虑下面的bean定义

```
<bean id="foo" class="foo.Bar">
    <property name="fred.bob.sammy" value="123" />
</bean>
```

foo bean有一个fred属性，fred有一个bob属性，bob哟一个samy属性，并且最后的属性值是123。为了保证可以运行，必须保证每一项都不为null。否则将会抛出空指针异常。