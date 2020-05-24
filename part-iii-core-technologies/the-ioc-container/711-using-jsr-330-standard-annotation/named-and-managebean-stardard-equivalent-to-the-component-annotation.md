#### @Named and @ManageBean: stardard equivalent to the @Component annotation

代替@Component，@javax.inject.Named或javax.annotation.ManageBean可以像如下这样使用：

```
import javax.inject.Inject;
import javax.inject.Named;

@Named("movieListener") // @ManagedBean("movieListener") could be used as well
public class SimpleMovieLister {
    private MovieFinder movieFinder;
    
    @Inject
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }
    // ...
}
```

通常使用@Component而不指定名在组件中。@Named也可以这样使用：

```
import javax.inject.Inject;
import javax.inject.Named;

@Named
public class SimpleMovieLister {
    private MovieFinder movieFinder;
    @Inject
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }
    // ...
}
```

当使用@Named或@ManageBean，最好和使用spring注解一样使用组件扫描。

```
@Configuration
@ComponentScan(basePackages = "org.example")
public class AppConfig {
    ...
}
```

>**Note**

> 与@Component对于，JSR330的@Named和JSR250的@ManageBean是不能组合使用的。请使用spring的策略模型用于构建自定义组件注解。



