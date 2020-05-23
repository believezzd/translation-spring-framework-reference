#### @Resource

spring也支持使用JSR250的@Resource注解修饰field和set方法来实现注入。这对于JavaEE5和6来说是很常见的，例如，在JSF1.2管理bean或JAX-WS2.0。spring也支持这种形式对于spring管理的object。

@Resource注解有一个name属性，默认使用bean的名字来实现注入。换句话说，就像下面的例子一样，按照名字匹配实现注入。

```
public class SimpleMovieLister {
    private MovieFinder movieFinder;
    @Resource(name="myMovieFinder")
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }
}
```

如果没有明确指定name属性，默认的name从修饰的field或set方法中来判断。如果是field，就使用field的name，如果是方法，就使用bean的property的那么。因此下面的例子中会使用“moveFinder”为名的bean进行注入：

```
public class SimpleMovieLister {
    private MovieFinder movieFinder;
    @Resource
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }
}
```

>**Note**

>提供了name的注解将通过bean的name来处理，通过受CommonAnnotationBeanPostProcessor控制的pplicationContext。如果你配置的spring的SimpleJndiBeanFactory话也可以使用JNDI来处理name。然而，建议你使用默认行为和简单使用spring的JNDI查找能力来包含间接的级别。

如果不使用name属性的@Resource注解的用法和@Autowired的用法相似，@Resource查找主要的类型匹配代替特定name的bean并解决常见的依赖：BeanFactory、ApplicationContext、ResourceLoader、ApplicationEventPublisher和MessageSource接口。

```
public class MovieRecommender {

    @Resource
    private CustomerPreferenceDao customerPreferenceDao;
    @Resource
    private ApplicationContext context;
    
    public MovieRecommender() {
    }
    // ...
}
```





