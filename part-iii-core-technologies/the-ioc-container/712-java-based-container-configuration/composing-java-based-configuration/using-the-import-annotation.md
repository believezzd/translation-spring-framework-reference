##### Using the @Import annotation

在spring的xml中import元素用于模块化配置，@Import注解允许从其他配置类中加载bean的定义：

```
@Configuration
public class ConfigA {
    @Bean
    public A a() {
        return new A();
    }
}

@Configuration
@Import(ConfigA.class)
public class ConfigB {
    @Bean
    public B b() {
        return new B();
    }
}
```

现在，不需要定义ConfigA和ConfigB类当实例化context，只需要提供ConfigB就可以

```
public static void main(String[] args) {
    ApplicationContext ctx = new AnnotationConfigApplicationContext(ConfigB.class);
    // now both beans A and B will be available...
    A a = ctx.getBean(A.class);
    B b = ctx.getBean(B.class);
}
```

这种方法简化了容器的实例化，只需要处理一个类，不在要求开发者在配置时使用多个@Configuration类。

>**Tip**

>在spring框架4.2中，@Import也支持引用普通的组件类，模拟了AnnotationConfigApplicationContext的注册方法。如果你想避免组件扫描时将会有很大的帮助，使用一些配置类作为实体点定义所有你的组件。