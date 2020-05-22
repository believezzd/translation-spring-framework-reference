#### Lazy-initialized beans

默认情况下，ApplicationContext的实现会在初始化的过程中初始化所有的单例bean。通常，提前初始化是好的，因为配置或环境的问题会被直接发现，而不是在几小时或几天以后。当你不需要这样的特性时，你可以阻止单例bean的提前初始化通过设置bean为延迟加载。一个延迟加载bean会通知容器在第一次使用的时候创建实例而不是初始化的时候。

在xml中，该特性由bean元素中的lazy-init属性控制，如下：

```
<bean id="lazy" class="com.foo.ExpensiveToCreateBean" lazy-init="true"/>
<bean name="not.lazy" class="com.foo.AnotherBean"/>
```

当定义了前面这样的设置，ApplicationContext在自身初始化的时候将不会提前初始化延迟加载的bean，反正，没有定义延迟加载的单例bean都会提前初始化。

然而，当一个延迟加载的bean是一个非延迟加载的单例bean的依赖，ApplicationContext还是会在启动时对他进行初始化，因为必须保证单例bean的依赖关系。延迟加载的bean会注入到非延迟加载的单例bean中。

你也可以通过beans中的default-lazy-init属性来控制容器中延迟初始化的等级。

```
<beans default-lazy-init="true">
<!-- no beans will be pre-instantiated... -->
<!-- 没有bean会被提前初始化-->
</beans>
```


