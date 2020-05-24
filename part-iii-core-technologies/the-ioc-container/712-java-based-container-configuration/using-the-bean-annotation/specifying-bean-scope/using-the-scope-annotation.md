###### Using the @Scope annotation

你使用@Bean定义bean的时候应该定义一个范围。你可以使用bean范围章节中任意一个标准的范围。

你使用@Bean定义bean的时候应该定义一个范围。你可以使用bean范围章节中任意一个标准的范围。

```
@Configuration
public class MyConfiguration {
    @Bean
    @Scope("prototype")
    public Encryptor encryptor() {
        // ...
    }
}
```