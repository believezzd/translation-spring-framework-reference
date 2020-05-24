##### Default profile

default 的profile代表profile将会默认使用。比如：

```
@Configuration
@Profile("default")
public class DefaultDataConfig {
    @Bean
    public DataSource dataSource() {
        return new EmbeddedDatabaseBuilder()
            .setType(EmbeddedDatabaseType.HSQL)
            .addScript("classpath:com/bank/config/sql/schema.sql")
            .build();
    }
}
```

没有profile被激活，但是数据源将被创建，这可以作为一种方式提供一个默认的定义为一个或多个bean。如果有profile被激活，则默认profile将不会被默认激活。

默认profile的名字可以通过Environment的setDefaultProfiles来改变或使用spring.profiles.default属性来定义。