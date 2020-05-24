##### Bean description

有时提供一个文字版的bean的描述是有帮助的。当bean被暴露时这很有效（例如通过JMX）。

使用@Description注解在@Bean中可以添加描述，如下这样使用：

```
@Configuration
public class AppConfig {
    @Bean
    @Description("Provides a basic example of a bean")
    public Foo foo() {
        return new Foo();
    }
}
```