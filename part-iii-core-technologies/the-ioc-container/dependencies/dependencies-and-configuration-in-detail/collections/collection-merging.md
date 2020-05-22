###### Collection merging

spring容器也支持集合的合并。应用开发者可以定义parent-style的list、map、set或props元素，并且也可定义list、map、set和props元素从父集合中继承或覆盖。子集合的值是子集合和父集合的合并，其中子集合会覆盖父集合的内容。

这个章节的合并讨论了有关父子bean的问题。如果读者不清楚父子bean可以先阅读后续的7.7节。

下面的例子描述了集合的合并：

```
<beans>
    <bean id="parent" abstract="true" class="example.ComplexObject">
        <property name="adminEmails">
            <props>
                <prop key="administrator">administrator@example.com</prop>
                <prop key="support">support@example.com</prop>
            </props>
        </property>
    </bean>
    <bean id="child" parent="parent">
        <property name="adminEmails">
            <!-- the merge is specified on the child collection definition -->
<!-- 合并是定义在子集合中的 -->
            <props merge="true">
                <prop key="sales">sales@example.com</prop>
                <prop key="support">support@example.co.uk</prop>
            </props>
        </property>
    </bean>
<beans>
```

注意在props属性中使用了merge=true属性对于adminEmails属性，在子bean定义中。当子bean被解析然后被容器初始化，adminEmails属性的结果是父bean的adminEmails和子bean的adminEmails的合并，如下：

```
administrator=administrator@example.com
sales=sales@example.com
support=support@example.co.uk
```

子属性从父属性中继承了所有的属性内容，并且子bean中support的值覆盖了父bean中同名的值。

合并这个属性对于list、map和set集合类型是相似的。特别是在list元素中，根据list的语义，会维护list的顺序，父bean中内容在所有子bean内容之前。在map、set和properties中没有顺序的概念。因此容器在合并map、set和properties时不会考虑顺序的问题。