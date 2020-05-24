##### Declaring a bean

使用@Bean注解方法简单定义一个bean。你可以通过方法的返回值来使用ApplicationContext来注册一个bean的定义。默认的，bean的名字会和方法名相同。下面就是一个简单的@Bean方法的例子：

```
@Configuration
public class AppConfig {
    @Bean
    public TransferServiceImpl transferService() {
    return new TransferServiceImpl();
    }
}
```

上面的配置和下面的xml中的定义是一样的效果：

```
<beans>
    <bean id="transferService" class="com.acme.TransferServiceImpl"/>
</beans>
```

两种方式都会使得transferService这个名字在ApplicationContext中有效，并且绑定TransferServiceImpl的返回对象：

```
transferService -> com.acme.TransferServiceImpl
```

你也可以在使用 @Bean 注解的方法上，返回接口类型：

```
@Configuration
public class AppConfig {
    @Bean
    public TransferService transferService() {
        return new TransferServiceImpl();
    }
}
```

但是，这将提前类型预测的可见性限制为指定的接口类型然后，使用容器只知道的完整类型(TransferServiceImpl)一旦受影响的单例bean被实例化。实例化非惰性单例bean根据它们的声明顺序，因此您可能会看到不同类型的匹配结果当另一个组件试图通过未声明的类型(如@Autowired)匹配时TransferServiceImpl只会在“transferService”bean被调用后才会解析实例化)。

>**Tip**

>如果您始终通过声明的服务接口引用您的类型，那么您的@Bean将返回类型可以安全地加入设计决策。但是，对于实现多个接口的组件或对于可能由其实现类型引用的组件，声明大多数特定的返回类型都是可能的(至少与引用注入点所要求的一样具体)您的bean)。
