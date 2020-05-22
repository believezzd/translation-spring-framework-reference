和“使用p命名空间的xml简写”类似，c命名空间是spring3.1的新特性，允许使用你不的属性用于配置构造器参数而不是使用constructor-arg元素。

现在回顾一下“基于构造器注入”的那个使用c命名空间的例子：

```
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"    xmlns:c="http://www.springframework.org/schema/c"    xsi:schemaLocation="http://www.springframework.org/schema/beans        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="bar" class="x.y.Bar"/>
    <bean id="baz" class="x.y.Baz"/>

    <!-- traditional declaration -->
    <bean id="foo" class="x.y.Foo">
        <constructor-arg ref="bar"/>
        <constructor-arg ref="baz"/>
        <constructor-arg value="foo@bar.com"/>
    </bean>
    <!-- c-namespace declaration -->
    <bean id="foo" class="x.y.Foo" c:bar-ref="bar" c:baz-ref="baz" c:email="foo@bar.com"/>
</beans>
```

c命名空间使用相同的规范和p命名空间（后面使用ref作为bean的引用）来通过名字设置构造器的参数。并且他们不再xsd的schema中，而在spring的核心代码中。

极少数情况下，构造器的参数名不是可见的（二进制代码没有调试信息的编译），这样可以使用index来标识。

```
<!-- c-namespace index declaration -->
<bean id="foo" class="x.y.Foo" c:_0-ref="bar" c:_1-ref="baz"/>
```

>**Note**

>遵循xml的语法，index的表示前需要_作为xml的属性名而不能以数字开头（尽管某些ide支持这种写法）

在实际中，构造器注入在匹配参数相当有效除非确实很需要他，我们推荐使用name符号在你的配置中。