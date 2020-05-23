#### @PostConstruct and @PreDestroy

CommonAnnotationBeanPostProcessor并不只会处理@Resource注解，也会处理JSR250中生命周期注解。在spring2.5中，支持这些注解并且会在初始化和销毁时进行回调。如果CommonAnnotationBeanPostProcessor在spring的ApplicationContext中注册了，那么在相应的时间点对应的方法会被调用。在下面的例子中，cache将会在开始时创建，然后在销毁时被清除。

```
public class CachingMovieLister {
    @PostConstruct
    public void populateMovieCache() {
        // populates the movie cache upon initialization...
    }
    
    @PreDestroy
    public void clearMovieCache() {
        // clears the movie cache upon destruction...
    }
}
```