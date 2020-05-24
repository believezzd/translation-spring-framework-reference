##### Bean dependencies

一个@Bean修饰的方法可以有多个参数依赖用于构建bean。例如TransferService需要一个AccountRepository作为依赖通过方法参数：

```
@Configuration
public class AppConfig {
    @Bean
    public TransferService transferService(AccountRepository accountRepository) {
        return new TransferServiceImpl(accountRepository);
    }
}
```

这个方法和构造依赖注入是相似的，详见7.4.1章节。