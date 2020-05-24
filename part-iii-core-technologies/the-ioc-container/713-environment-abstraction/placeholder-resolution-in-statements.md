#### Placeholder resolution in statements

历史上说，在元素中占位符的值可以通过JVM系统参数或环境变量来处理。不再是这样的。因为环境抽象已经集成在容器，可以更加容器的处理占位符。这意味着你可以配置处理方式根据你的需要：改变查找的优先级或移除他们、添加你自己的属性源码来混合使用。

具体的，下面的声明可以作为一个自定义的属性来定义，并且在容器中有效：

```
<beans>
    <import resource="com/bank/service/${customer}-config.xml"/>
</beans>
```