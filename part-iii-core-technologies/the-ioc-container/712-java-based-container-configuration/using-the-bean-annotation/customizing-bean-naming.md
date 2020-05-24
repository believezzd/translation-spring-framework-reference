##### Customizing bean naming

默认，使用@Bean的方法名和bean的名字相同。但是使用name属性可以导致名字被覆盖。

```
@Configuration
public class AppConfig {
@Bean(name = "myFoo")
    public Foo foo() {
        return new Foo();
    }
}
```