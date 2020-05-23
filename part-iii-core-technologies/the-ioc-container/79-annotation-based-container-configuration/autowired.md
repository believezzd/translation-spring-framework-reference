#### @Autowired

>**Note**

> 在JSR330中@Inject注解可以用于替代spring的@Autowired注解，如下面的例子。详见7.11。

你可以在构造器上使用@Autowired注解

```
public class MovieRecommender {
    private final CustomerPreferenceDao customerPreferenceDao;
    
    @Autowired
    public MovieRecommender(CustomerPreferenceDao customerPreferenceDao) {
        this.customerPreferenceDao = customerPreferenceDao;
    }
    
    // ...
}
```

>**Note**

>在spirng4.3中，@Autowired可以不在修饰构造器如果目标bean只有一个构造器时。如果有多个构造器，则至少有一个需要被修饰来告诉容器使用哪个构造器。

正如期望的，@Autowired注解也可以用于传统的set方法：

```
public class SimpleMovieLister {
    private MovieFinder movieFinder;
    
    @Autowired
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }
    // ...
}
```

你也可以将这个注解应用于多个参数的方法中：

```
public class MovieRecommender {
    private MovieCatalog movieCatalog;
    private CustomerPreferenceDao customerPreferenceDao;

    @Autowired
    public void prepare(MovieCatalog movieCatalog,
    CustomerPreferenceDao customerPreferenceDao) {
        this.movieCatalog = movieCatalog;
        this.customerPreferenceDao = customerPreferenceDao;
    }
    // ...
}
```

你也可以在属性和构造器中混合使用@Autowired。

```
public class MovieRecommender {
    private final CustomerPreferenceDao customerPreferenceDao;
    @Autowired
    private MovieCatalog movieCatalog;

    @Autowired
    public MovieRecommender(CustomerPreferenceDao customerPreferenceDao) {
        this.customerPreferenceDao = customerPreferenceDao;
    }
    // ...
}
```

>**Tip**

> 确保要注入的目标组件的类型与使用 @Autowired 进行注解注入的类型一致。否则会因为类型未找到导致注入失败。

> XML 配置的 bean，容器会提前知道确切的类型。而使用 @bean 注解的工厂方法，需要确保声明的返回类型具有足够的表达性。对于实现多个接口的组件，考虑在工厂方法上声明最具有特点的返回类型（至少与指向的注入点的类型一致）

还可以获取特定类型的所有 bean，通过设置注解在数组类型的字段或者方法上：

```
public class MovieRecommender {
    @Autowired
    private MovieCatalog[] movieCatalogs;
    // ...
}
```

也可以应用于集合类型：

```
public class MovieRecommender {
    private Set<MovieCatalog> movieCatalogs;
    
    @Autowired
    public void setMovieCatalogs(Set<MovieCatalog> movieCatalogs) {
        this.movieCatalogs = movieCatalogs;
    }
    // ...
}
```

>**Tip**

>你的bean可以实现org.springframework.core.Ordered接口，或使用@Order、@Priority注解，如果你希望数组或list中的元素可以按照一定的顺序排序。

> @Order注释可以在目标类级别声明，也可以在@Bean方法中声明，每个bean定义可能非常独立(在多个定义具有相同的情况下).@Order值可能会影响注射点的优先级，但请注意它们不影响单例启动顺序，单例启动顺序是由依赖关系和@DependsOn声明。

> 注意，javax.annotation.Priority 注解不可以使用在 @Bean 级别上，因为它不能在方法上声明。

> Priority 的语义可以通过组合 @Order与@Primary 的方式在每个类型的单个bean上实现。

甚至Maps也可以被期望的字符串类型的key值注入。map的值可以是被期望的类型的bean，key应该是相对应的bean的名字。

```
public class MovieRecommender {
    private Map<String, MovieCatalog> movieCatalogs;
    @Autowired
    public void setMovieCatalogs(Map<String, MovieCatalog> movieCatalogs) {
        this.movieCatalogs = movieCatalogs;
    }
    // ...
}
```

默认情况在，自动注入在没有一个匹配时会失败，默认的行为是对修饰的方法、构造器和属性的依赖是必须的。下面的行为可以改变这种描述。

```
public class SimpleMovieLister {
    private MovieFinder movieFinder;
    @Autowired(required = false)
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }
    // ...
}
```

>**Note**

>任何给定bean类只有一个构造函数可以使用required声明@Autowired属性设置为true，指示作为Spring bean使用时自动装配的构造函数。此外，如果required属性被设置为true，则可能只有一个构造函数可以使用 @Autowired 进行注解。如果多个非必需的构造函数声明注释，则它们将被考虑作为自动装配的候选人。依赖项被满足最大数目的构造函数会被Spring容器选择。如果没有一个候选者可以满足，那么主/默认构造函数(如果存在)将满足使用。如果一个类一开始只声明一个构造函数，那么它总是会被使用，即使不是带注释的。带注释的构造函数不一定是 public 的。

>推荐使用@Autowired的required属性，而不是@Required注释setter方法。required属性表示自动装配不需要该属性目的。如果属性不能自动获取，则忽略该属性。另一方面，@Required是更强的是，它强制通过容器支持的任何方式设置属性。如果没有值已定义，并引发相应的异常。

或者，您可以使用 Java 8 提供的表达非必需的特定依赖项的性质java.util.Optional:

```
public class SimpleMovieLister {
    @Autowired
    public void setMovieFinder(Optional<MovieFinder> movieFinder) {
        ...
    }
}
```

你也可以使用@Autowired来修饰常用的依赖：BeanFactory、ApplicationContext、Environment、ResourceLoader、ApplicationEventPublisher和MessageSource。这些即可和他们的子接口如ConfigurableApplicationContext、ResourcePatternResolver都可以被自动注入而不需要特殊的设置。

```
public class MovieRecommender {
    @Autowired
    private ApplicationContext context;
    
    public MovieRecommender() {
    }
    // ...
}
```

>**Note**

>@Autowired、@Inject、@Resource和@Value注解通过spring的BeanPostProcessor实现来处理，也就意味着你不能使用这些注解在你自己的BeanPostProcessor或BeanFactoryPostProcessor类型中。这些类型必须通过xml或用@Bean来修饰。

