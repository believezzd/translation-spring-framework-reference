##### Bean aliasing

在7.3.1章节中讨论的，命名bean，在有时可能会给单一的一个bean提供多个name，也就是bean的别名。@Bean的name属性支持一个字符串数组用于实现bean的别名。

```
@Configuration
public class AppConfig {
@Bean({"dataSource", "subsystemA-dataSource", "subsystemB-dataSource"})
public DataSource dataSource() {
    // instantiate, configure and return DataSource bean...
}
}
```