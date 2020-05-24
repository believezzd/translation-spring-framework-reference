##### @Profile

@Profile注解允许你指出一个组件可以被注册，当一个或多个profile被激活时。使用我们上面的例子，我们可以重写数据源的配置如下：

```
@Configuration
@Profile("development")
public class StandaloneDataConfig {
    @Bean
    public DataSource dataSource() {
        return new EmbeddedDatabaseBuilder()
            .setType(EmbeddedDatabaseType.HSQL)
            .addScript("classpath:com/bank/config/sql/schema.sql")
            .addScript("classpath:com/bank/config/sql/test-data.sql")
            .build();
    }
}
```

```
@Configuration
@Profile("production")
public class JndiDataConfig {
    @Bean(destroyMethod="")
    public DataSource dataSource() throws Exception {
        Context ctx = new InitialContext();
        return (DataSource) ctx.lookup("java:comp/env/jdbc/datasource");
    }
}
```

>**Note**

>如之前所提到的，使用@Bean方法的时候，你通常可以选择使用编程查找JNDI：使用spring的JndiTemplate/JndiLocatorDelegate或者直接使用 JNDI InitialContext ，而不是使用JndiObjectFactoryBean变量，它会将会强制要求你返回返回FactoryBean类型。

@Profile也可以被当做元注解用于创建自定义组合注解。下面的例子定义了一个自定义的@Production注解可以替换@Profile("production")：

```
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Profile("production")
public @interface Production {
}
```

>**Tip**

>如果@Configuration类标记为@Profile，则所有@Bean方法和@Import与该类关联的注释将被绕过，除非有一个或多个指定的概要文件是活跃的。如果@Component或@Configuration类被标记为@Profile({"p1"，，除非配置文件'p1'和/或'p2'已被注册/处理，否则该类将不会被注册/处理激活。如果给定的配置文件以NOT操作符(!)为前缀，那么带注释的元素将会如果配置文件不活动，则注册。例如，给定@Profile({"p1"， "!p2"})，如果配置文件“p1”是活动的，或者配置文件“p2”不是活动的，就会发生注册。

@Profile也可以被定义在方法级别，包括只有一个特定bean的configuration类中：

```
@Configuration
public class AppConfig {

    @Bean("dataSource")
    @Profile("development")
    public DataSource standaloneDataSource() {
        return new EmbeddedDatabaseBuilder()
            .setType(EmbeddedDatabaseType.HSQL)
            .addScript("classpath:com/bank/config/sql/schema.sql")
            .addScript("classpath:com/bank/config/sql/test-data.sql")
            .build();
    }

    @Bean("dataSource")
    @Profile("production")
    public DataSource jndiDataSource() throws Exception {
        Context ctx = new InitialContext();
        return (DataSource) ctx.lookup("java:comp/env/jdbc/datasource");
    }
}
```

>**Note**

>对于@Bean方法上的@Profile，可以应用一个特殊的场景:在重载的情况下具有相同Java方法名的@Bean方法(类似于构造函数重载)@Profile条件需要在所有重载方法上一致声明。如果条件不一致，只有重载中第一个声明的条件不一致重要的方法。因此，@Profile不能用于选择带有特定论点签名超过另一个;解决所有工厂之间的方法相同bean在创建时遵循Spring的构造函数解析算法

>如果您想定义具有不同概要条件的可选bean，请使用不同的Java方法名通过@Bean name属性指向相同的bean名，如上面的例子。如果参数签名都是相同的(例如，所有变量都没有参数)这是在一个有效的Java类中表示这种安排的唯一方法第一个位置(因为特定名称和参数签名只能有一个方法)。


