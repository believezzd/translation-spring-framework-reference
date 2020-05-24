##### Further information about how Java-based configuration works internally

下面的例子展示了一个@Bean注解的方法被调用了两次

```
@Configuration
public class AppConfig {
    @Bean
    public ClientService clientService1() {
        ClientServiceImpl clientService = new ClientServiceImpl();
        clientService.setClientDao(clientDao());
        return clientService;
    }
    @Bean
    public ClientService clientService2() {
        ClientServiceImpl clientService = new ClientServiceImpl();
        clientService.setClientDao(clientDao());
        return clientService;
    }
    @Bean
    public ClientDao clientDao() {
        return new ClientDaoImpl();
    }
}
```

clientDao方法在clientService1和clientService2中各被调用了一次。这个方法用于创建ClientDaoImpl的实例并返回，你正常情况需要两个实例（每个service一个实例）。这种定义是有问题的，在spring中，默认的bean实例化之后是单例的。这就有趣的地方，所有@Configuration类在启动时都会使用CGLIB创建子类。在子类中，子方法首先检查容器是否缓存了bean在调用父方法创建一个新的实例之前。注意在spring3.2中，不需要在类路径中添加CGLIB，因为CGLIB类已经被重新打包放在org.springframework.cglib中并且包括在spring-core的jar中。

>**Note**
>由于你的bean的范围，行为也会有些不同。我们在这里讨论的是单例的bean。

>**Tip**
>由于CGLIB在启动时动态的添加特性，会导致一些限制，特别如果配置的类不是final的时候。然而，由于4.3节，构造器允许在配置类中使用，包括使用@Autowired或一个没有默认构造器的定义用于依赖注入。
>如果你想避免CGLIB强加的限制，考虑在没有@Configuration类中定义@Bean方法，例如，使用普通@Component类作为替代。在@Bean方法中跨方法调用不会拦截，因此你必须避免在构造中依赖注入或方法级别进行依赖注入。
