#### Basic concepts: @Bean and @Configuration

对spring的新java配置提供支持的是@Configuration注解类和@Bean注解类。

@Bean注解用于指定配置和初始化由spring的ioc容器管理的实例。@Bean注解和beans元素中的bean元素有相同的作用。你可以在@Component中使用@Bean注解方法，然而，最常用的是@Configuration修饰bean。

使用@Configuration修饰类指明主要目的是作为一个bean的定义。更进一步，@Configuration类允许内部bean依赖通过在同一个类中调用其他的@Bean方法。最简单的例子如下：

上面的AppConfig类相当于下面的xml文件片段：

```
<beans>
    <bean id="myService" class="com.acme.services.MyServiceImpl"/>
</beans>
```

>**Full @Configuration vs 'lite' @Bean mode?**

>当一个@Bean的方法定义在一个没有@Configuration修饰的类中时，他们将会以简化的模式被处理。Bean方法在@Component中声明或者即使在一个普通的旧类中也会被认为是“lite”，具有不同的主要目的类和@Bean方法只是一种附加功能。例如，服务组件是否可以通过每个容器上的附加@Bean方法向容器公开管理视图适用的组件类。在这种情况下，@Bean方法是一种简单的通用方法工厂方法的机制。

>与全@Configuration不同，简化的@Bean方法不能定义内部bean的依赖。相反，它们对包含它们的组件的内部状态和可选的参数进行操作好让他们宣布。因此，这样的@Bean方法不应该调用其他@Bean方法;每个这样的方法实际上只是一个特定bean引用的工厂方法，没有任何方法特殊的运行时语义。这里的积极副作用是不需要进行CGLIB子类化在运行时应用，因此在类设计方面没有限制(即包含类)可能是最终的等等)。

>在常见的场景中，@Bean方法是在@Configuration类中声明的，确保总是使用“full”模式，因此会得到交叉方法引用重定向到容器的生命周期管理。这将阻止相同的@Bean方法通过一个常规的Java调用意外地被调用，这有助于减少可能出现的细微错误在“精简”模式下运行时很难追踪。

@Bean注解和@Configuration注解将在后续的章节中介绍。首先，我们介绍一下使用基于java的配置创建spring容器的不同方法。




