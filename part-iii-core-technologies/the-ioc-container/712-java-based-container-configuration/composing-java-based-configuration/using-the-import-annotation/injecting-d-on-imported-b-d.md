###### Injecting dependencies on imported @Bean definitions

上面的例子可以工作，但是过于简单。在很多实际的场景中，bean之间是跨配置类互相依赖的。当使用xml时，这不是问题，因为还没有编译器的参与，一个bean可以很简单的引用另一个bean，然后spring的容器会完成初始化操作。当然，当使用@Configuration类时，Java编译器会在配置模型中限制，保证引用的另一个bean符合java语法。

幸运的是解决这个问题也简单。就像我们讨论过的，@Bean方法可以有不定数量的参数依赖。让我们考虑一下更加实际的场景，包含多个@Configuration类，每个又依赖其他的bean：

```
@Configuration
public class ServiceConfig {
    @Bean
    public TransferService transferService(AccountRepository accountRepository) {
        return new TransferServiceImpl(accountRepository);
    }
}

@Configuration
public class RepositoryConfig {
    @Bean
    public AccountRepository accountRepository(DataSource dataSource) {
        return new JdbcAccountRepository(dataSource);
    }
}

@Configuration
@Import({ServiceConfig.class, RepositoryConfig.class})
public class SystemTestConfig {
    @Bean
    public DataSource dataSource() {
        // return new DataSource
    }
}

public static void main(String[] args) {
    ApplicationContext ctx = new AnnotationConfigApplicationContext(SystemTestConfig.class);
    // everything wires up across configuration classes...
    TransferService transferService = ctx.getBean(TransferService.class);
    transferService.transfer(100.00, "A123", "C456");
}
```

有另外一种获取相同结果的方式。注意@Configuration类知识容器中另一个bean。这意味着可以通过@Autowired和@Value注入就像其他的bean一样。

>**Waring**
>保证你注入的依赖是简单的形式。@Configuration类在之前就被处理并且强制依赖注入会导致不可预见的提前初始化。不管何时，依赖于参数注入参考上面的例子。

>注意通过@Bean定义的BeanPostProcessor和BeanFactoryPostProcessor。他们通常被定义为静态的@Bean方法，不会触发容器配置类的初始化。另外，@Autowired和@Value不会对配置类起作用如果很早就创建了bean的实例。

```
@Configuration
public class ServiceConfig {
    @Autowired
    private AccountRepository accountRepository;
    @Bean
    public TransferService transferService() {
        return new TransferServiceImpl(accountRepository);
    }
}

@Configuration
public class RepositoryConfig {
    private final DataSource dataSource;
    @Autowired
    public RepositoryConfig(DataSource dataSource) {
        this.dataSource = dataSource;
    }

    @Bean
    public AccountRepository accountRepository() {
        return new JdbcAccountRepository(dataSource);
    }
}

@Configuration
@Import({ServiceConfig.class, RepositoryConfig.class})
public class SystemTestConfig {
    @Bean
    public DataSource dataSource() {
        // return new DataSource
    }
}

public static void main(String[] args) {
    ApplicationContext ctx = new AnnotationConfigApplicationContext(SystemTestConfig.class);
    // everything wires up across configuration classes...
    TransferService transferService = ctx.getBean(TransferService.class);
    transferService.transfer(100.00, "A123", "C456");
}
```

>**Tip**

>从spring框架4.3开始支持在@Configuration类中的构造器注入。注意如果目标bean值定义了一个构造器，则@Autowired可以省略，上面的例子中，RepositoryConfig的构造器中不需要@Autowired。

在上面的场景中，使用@Autowired会更好并提供预期的模块性，但是决定哪些bean是被自动扫描的依然是困难的。例如，作为一个开发者需要一个ServiceConfig，你如何确切的知道在哪里注入AccountRepository？他并没有明确的在代码中，或许这样会好。记住Spring Tool Suite提供了工具可以生成图表来展示是怎么样注入的，这个或许你会需要。而且JavaIDE可以轻松找到所有的定义和使用AccountRepository类型的位置，并且会快速展示@Bean方法的位置并返回相应的类型。

如此这个考虑是可以被解决的而且你希望可以有直接的导航利用你的IDE从一个@Configuration类到另一个，考虑这样配置类：

```
@Configuration
public class ServiceConfig {
    @Autowired
    private RepositoryConfig repositoryConfig;
    @Bean
    public TransferService transferService() {
        // navigate 'through' the config class to the @Bean method!
        return new TransferServiceImpl(repositoryConfig.accountRepository());
    }
}
```

在上面的情况中，已经明确的声明了AccountRepository的定义。然而，ServiceConfig和RepositoryConfig是绑定的，这是折中的。通过基于接口或基于抽象类的@Configuration类可以减弱这种耦合。考虑下面的例子：

```
@Configuration
public class ServiceConfig {
    @Autowired
    private RepositoryConfig repositoryConfig;
    @Bean
    public TransferService transferService() {
        return new TransferServiceImpl(repositoryConfig.accountRepository());
    }
}

@Configuration
public interface RepositoryConfig {
    @Bean
    AccountRepository accountRepository();
}

@Configuration
public class DefaultRepositoryConfig implements RepositoryConfig {
    @Bean
    public AccountRepository accountRepository() {
        return new JdbcAccountRepository(...);
    }
}

@Configuration
@Import({ServiceConfig.class, DefaultRepositoryConfig.class}) // import the concrete config!
public class SystemTestConfig {
    @Bean
    public DataSource dataSource() {
        // return DataSource
    }
}

public static void main(String[] args) {
    ApplicationContext ctx = new AnnotationConfigApplicationContext(SystemTestConfig.class);
    TransferService transferService = ctx.getBean(TransferService.class);
    transferService.transfer(100.00, "A123", "C456");
}
```

现在ServiceConfig和创建的DefaultRepositoryConfig是松耦合的，并且内置的IDE工具依然是有效的：这样可以帮助开发者获得一个RepositoryConfig实现的类型结构。使用这种方式，导航@Configuration类和他们的依赖变得和通常导航基于接口的代码是没有区别的。

>**Tip**
>如果您想影响某些bean的启动创建顺序，请考虑声明一些bean它们中的@Lazy(在第一次访问时用于创建，而不是在启动时)@DependsOn(取决于)其他bean(确保在当前bean之前创建特定的其他bean后者的直接依赖意味着什么)。
