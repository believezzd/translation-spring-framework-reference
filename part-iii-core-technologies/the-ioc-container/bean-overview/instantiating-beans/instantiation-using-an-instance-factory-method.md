和使用静态工厂方法初始化类似，使用实例化工厂方法调用一个已经存在的bean的非静态方法从构造器中创建一个新的bean。使用这样的特性，需要将class属性设置为空，并且在factory-bean属性中定义当前容器或父容器中包含创建object的方法的类。然后在factory-method的属性中设置工厂方法的名字。

```
<!-- the factory bean, which contains a method called createInstance() -->
<bean id="serviceLocator" class="examples.DefaultServiceLocator">
    <!-- inject any dependencies required by this locator bean -->
</bean>

<!-- the bean to be created via the factory bean -->
<bean id="clientService" factory-bean="serviceLocator" factory-method="createClientServiceInstance"/>
```

```
public class DefaultServiceLocator {
    private static ClientService clientService = new ClientServiceImpl();
    
    private DefaultServiceLocator() {}
    
    public ClientService createClientServiceInstance() {
        return clientService;
    }
}
```

一个工厂类也可以有多个工厂方法:

```
<bean id="serviceLocator" class="examples.DefaultServiceLocator">
<!-- inject any dependencies required by this locator bean -->
</bean>

<bean id="clientService" factory-bean="serviceLocator" factory-method="createClientServiceInstance"/>

<bean id="accountService" factory-bean="serviceLocator" factory-method="createAccountServiceInstance"/>
```

```
public class DefaultServiceLocator {
    private static ClientService clientService = new ClientServiceImpl();
    
    private static AccountService accountService = new AccountServiceImpl();
    
    private DefaultServiceLocator() {}
    
    public ClientService createClientServiceInstance() {
        return clientService;
    }
    
    public AccountService createAccountServiceInstance() {
        return accountService;
    }
}
```

这个例子说明工厂bean自身也可以通过依赖注入来管理和配置。详见Dependencies and configuration in detail。

>Note
>在spring文档中，factory bean是spring容器中可以通过实例或者静态工厂方法创建对象的bean。FactoryBean（注意大小写）就是spring的特殊的FactoryBean。