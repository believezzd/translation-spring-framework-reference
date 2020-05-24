##### Enabling component scanning with scan(String…)

为了允许组件扫描，只需要在类上声明@Configuration如下：

```
@Configuration
@ComponentScan(basePackages = "com.acme")
public class AppConfig {
    ...
}
```

>**Tip**

>有经验的spring用户会更加熟悉在spring的context中定义命名空间：

>```
><beans>
>    <context:component-scan base-package="com.acme"/>
></beans>
>```

在上面的例子中，com.acme包将会被扫描，需要@Component修饰的类，并且这些类会被注册为bean的定义在容器中。AnnotationConfigApplicationContext允许使用scan方法来实现相同的组件扫描功能。

```
public static void main(String[] args) {
    AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext();
    ctx.scan("com.acme");
    ctx.refresh();
    MyService myService = ctx.getBean(MyService.class);
}
```

>**Note**

> 记住@Configuration类是@Component的元注解，也就暗示着支持组件扫描。在上面的例子中，假设AppConfig被定义在com.acme包中（或下面的子包），都会在调用scan时被扫描，并且在refresh时所有的@Bean方法都会被注册为bean定义。



