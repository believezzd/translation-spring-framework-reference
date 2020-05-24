##### Injecting inter-bean dependencies

当一个@Bean依赖另一个bean，表达依赖的简单方法就是有一个bean方法调用另一个方法：

```
@Configuration
public class AppConfig {
    @Bean
    public Foo foo() {
        return new Foo(bar());
    }
    @Bean
    public Bar bar() {
        return new Bar();
    }
}
```

上面的例子中，foo的bean接收了一个bar的应用通过构造器注入。

>**Note**

>这种方法定义内部bean依赖只使用与@Configuration类中的@Bean方法。你不可以在普通的@Component类中定义内部bean依赖。