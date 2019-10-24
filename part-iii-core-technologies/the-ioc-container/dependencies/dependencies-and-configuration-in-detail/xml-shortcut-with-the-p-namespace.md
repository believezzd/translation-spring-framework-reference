p命名空间允许你使用bean的元素属性，而不是使用内置的property元素来描述组合bean中属性值。

通过xml的schema定义，spring允许使用命名空间支持外部的定义。这章节中讨论的bean的配置形式定义在XML schema文档中。然而，p命名空间不在xsd文件中而在spring的核心中。

下面的例子是两个xml片段，具有相同的功能：第一个使用了标准的xml格式，第二个使用的p命名空间。


```
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"    xmlns:p="http://www.springframework.org/schema/p"    xsi:schemaLocation="http://www.springframework.org/schema/beans        http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <bean name="classic" class="com.example.ExampleBean">
        <property name="email" value="foo@bar.com"/>
    </bean>
    
    <bean name="p-namespace" class="com.example.ExampleBean"
        p:email="foo@bar.com"/>
</beans>
```

上面例子展示了在bean定义中一个属性在p命名空间中叫email。这告诉spring包括一个属性的定义。前面提到的，p命名空间没有schema的定义，因此你可以设置attribute的名字作为property名字。

下面的例子包括另外2个bean的定义且同时指向另一个bean。

```
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"    xmlns:p="http://www.springframework.org/schema/p"    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean name="john-classic" class="com.example.Person">
        <property name="name" value="John Doe"/>
        <property name="spouse" ref="jane"/>
    </bean>
    
    <bean name="john-modern" class="com.example.Person"
        p:name="John Doe"
        p:spouse-ref="jane"/>
        
    <bean name="jane" class="com.example.Person">
        <property name="name" value="Jane Doe"/>
    </bean>
</beans>
```

你看到了，这个例子包含了不只一个元素使用p命名空间，但是也使用了特殊的形式来定义属性的引用。第一个bean的定义使用了<property name="spouse" ref="jane"/>来创建一个到john bean的指向到bean jane，第二个bean使用p:spouse-ref="jane"作为一个属性来做相同的事情。在这个例子中spouse是一个属性名，-ref的部分指示了他不是一个直接值而是一个对其他bean的引用。

p命名空间并不是标准的xml形式。例如，比如定义属性引用的使用属性名后加-ref的，也不是标准的xml形式。我们建议你和你的小组成员讨论，来避免在xml文档中同时使用所有三个途径。
