##### Receiving lifecycle callbacks

任何定义@Bean注解的类都支持生命周期方法回调，使用JSR250中的@PostConstruct和@PreDestroy，详见JSR250注解说明。

支持所有常规的spring生命周期回调方法。如果一个bean实现了InitializingBean、DisposableBean或Lifecycle，其相应的方法也会被容器调用。

也支持*Aware接口，例如BeanFactoryAware、BeanNameAware、MessageSourceAware、ApplicationContextAware等等。

@Bean注解支持定义初始化方法或销毁回调方法，很像spring的xml中bean元素中的init-method和destroy-method属性。

```
public class Foo {
    public void init() {
        // initialization logic
    }
}

public class Bar {
    public void cleanup() {
        // destruction logic
    }
}
@Configuration
public class AppConfig {
    @Bean(initMethod = "init")
    public Foo foo() {
        return new Foo();
    }
    @Bean(destroyMethod = "cleanup")
    public Bar bar() {
        return new Bar();
    }
}
```

>**Note**

>默认情况下，使用java配置的bean定义有一个公共的关闭方法在销毁时被回调。如果有公共的销毁回调方法，但是你不希望在容器关闭时调用，那么可以使用@Bean(destroyMethod="")来使得你的bean不调用默认关闭方法。

>你或许希望将其设置为默认，比如一个资源你通过JNDI获得并且它的生命周期由外部管理。特殊情况下，保证你只处理数据源，在JavaEE应用服务器中可能会有问题。

>```
>@Bean(destroyMethod="")
>public DataSource dataSource() throws NamingException {
>    return (DataSource) jndiTemplate.lookup("MyDS");
>}
>```

> 在@Bean方法中，你可以使用JNDI查找：或使用spring的JndiTemplate/JndiLocatorDelegate来辅助或直接使用JNDI InitialContext，但是JndiObjectFactoryBean不会强制你定义工厂bean的返回类型代替目标类型，使得交叉引用其他的@Bean方法来代替源代码中提供的。

当然，在上面的Foo中，和在构造中直接调用init方法是一样的。

```
@Configuration
public class AppConfig {
    @Bean
    public Foo foo() {
        Foo foo = new Foo();
        foo.init();
        return foo;
    }
    // ...
}
```

>**Tip**

>当你直接使用java代码时，你可以对你的object做任何事情并且不需要依赖容器的生命周期。

