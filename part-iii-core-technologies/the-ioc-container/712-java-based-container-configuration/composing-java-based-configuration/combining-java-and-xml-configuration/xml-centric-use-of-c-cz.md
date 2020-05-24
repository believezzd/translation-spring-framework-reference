###### XML-centric use of @Configuration classes

使用xml启动容器然后包含@Configuration类是一种方式。例如，在一个已存在的代码库使用spring的xml文件，将会更加简单创建基于需要的@Configuration类和包括已存在的xml文件。下面你会找到在以xml为中心的情况下使用@Configuration类。

记住@Configuration类只是在容器中的bean的定义。在这个例子中，我们创建@Configuration类名为AppConfig和包括他的system-test-config.xml文件作为bean的定义。因为扫描的功能被打开了，容器会发现@Configuration注解并会处理定义在AppConfig@Bean方法。

```
@Configuration
public class AppConfig {
    @Autowired
    private DataSource dataSource;
    @Bean
    public AccountRepository accountRepository() {
        return new JdbcAccountRepository(dataSource);
    }
    @Bean
    public TransferService transferService() {
        return new TransferService(accountRepository());
    }
}
```

system-test-config.xml:

```
<beans>
    <!-- enable processing of annotations such as @Autowired and @Configuration -->
    <context:annotation-config/>
    <context:property-placeholder location="classpath:/com/acme/jdbc.properties"/>
    <bean class="com.acme.AppConfig"/>
    <bean class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>
</beans>
```

jdbc.properties:

```
jdbc.url=jdbc:hsqldb:hsql://localhost/xdb
jdbc.username=sa
jdbc.password=
```

```
public static void main(String[] args) {
    ApplicationContext ctx = new ClassPathXmlApplicationContext("classpath:/com/acme/system-testconfig.
    xml");
    TransferService transferService = ctx.getBean(TransferService.class);
    // ...
}
```

>**Note**

>在上面的system-test-config.xml中，bean AppConfig并没有定义id元素。这是可以被接受的，而且没有必要定义因为没有bean会引用到他，而且不应该让容器通过名字引用到他。同样也包括DataSource的bean——只是通过类型注入，因此不需要明确定义bean的id。

因为@Configuration是@Component的一个元注解，基于@Configuration类会自动被组件扫描发现。和上面的场景一样，我们重新定义了system-test-config.xml文件来使用组件扫描。注意在这种情况下，我们不需要明确定义<context:annotation-config/>，因为<context:component-scan/>也有相同的功能。

system-test-config.xml:

```
<beans>
    <!-- picks up and registers AppConfig as a bean definition -->
    <context:component-scan base-package="com.acme"/>
    <context:property-placeholder location="classpath:/com/acme/jdbc.properties"/>
    <bean class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>
</beans>
```
