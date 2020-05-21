###### References to other beans(collaborators)

在<constructor-arg/>和<property/>元素中，ref是一个final元素。这里你可以设置来引用容器中里一个被管理的bean。被引用的bean是通过bean的set方法的注入的依赖，并且会在被需要的类之前被初始化。（如果需要的是一个单例的bean，或许已经被容器初始化了）。所有的引用都最终会指向另外一个对象。范围和校验依赖于你是否明确指定了另一个object的id或name，在bean、local或parent属性中。

使用ref来定义引用的bean是最常见的形式，可以引用同一个容器或父容器的任意一个bean，不管有没有在相同的xml文件中配置。bean的value属性和目标bean的id属性是一样的，或是目标bean的name属性。

```
<ref bean="someBean"/>
```

通过parent属性定义目标bean，创建了一个对目前容器的父容器的一个bean的引用。parent属性的value属性和目标bean的id属性是一样的，或是目标bean的name属性，并且目标bean必须在当前容器的父容器中。你使用这个bean引用，主要是当你有一个层次结构的容器，并且你想使用代理包装现有存在于父容器中的bean，并具有相同的名称。

```
<!-- in the parent context -->
<bean id="accountService" class="com.foo.SimpleAccountService">
    <!-- insert dependencies as required as here -->
</bean>
```

```
<!-- in the child (descendant) context -->
<bean id="accountService" <!-- bean name is the same as the parent bean -->    class="org.springframework.aop.framework.ProxyFactoryBean">
    <property name="target">
        <ref parent="accountService"/> <!-- notice how we refer to the parent bean -->
    </property>
    <!-- insert other configuration and dependencies as required here -->
</bean>
```

