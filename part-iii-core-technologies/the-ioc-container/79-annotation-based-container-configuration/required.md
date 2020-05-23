#### @Required

@Required注解应用bean的set方法中，如下

```
public class SimpleMovieLister {
    private MovieFinder movieFinder;
    
    @Required
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }
    // ...
}
```

这个注解指明bean的属性必须在配置是被赋予，在bean定义中注入或自动注入。如果没有被注入会抛出一个bean属性的异常，这样可以避免后续的空指针异常。并且强烈建议你断言类本身在初始化方法中。这样可以强制在引用即便是这个类在容器外部使用。