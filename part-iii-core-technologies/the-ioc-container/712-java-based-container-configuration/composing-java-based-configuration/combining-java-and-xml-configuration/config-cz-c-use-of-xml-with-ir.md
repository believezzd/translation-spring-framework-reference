###### @Configuration class-centric use of XML with @ImportResource

如果在应用中以@Configuration类作为主要的策略来配置容器，但仍然需要一些xml文件。在这些场景中，简单的使用@ImportResource来引入xml是有用的。这样可以在以java为中心的前提下最低限度的使用xml。

```
@Configuration
@ImportResource("classpath:/com/acme/properties-config.xml")
    public class AppConfig {
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.username}")
    private String username;
    @Value("${jdbc.password}")
    private String password;
    @Bean
    public DataSource dataSource() {
        return new DriverManagerDataSource(url, username, password);
    }
}
```

properties-config.xml: 

```
<beans>
    <context:property-placeholder location="classpath:/com/acme/jdbc.properties"/>
</beans>
```

```
jdbc.properties
jdbc.url=jdbc:hsqldb:hsql://localhost/xdb
jdbc.username=sa
jdbc.password=
```

```
public static void main(String[] args) {
    ApplicationContext ctx = new AnnotationConfigApplicationContext(AppConfig.class);
    TransferService transferService = ctx.getBean(TransferService.class);
    // ...
}
```