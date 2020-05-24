#### Dependency Injection With @Inject and @Named

代替了@Autowired，@javax.injiect.Inject可以像如下的方式一样使用：

```
import javax.inject.Inject;

public class SimpleMovieLister {

    private MovieFinder movieFinder;

    @Inject
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }
    public void listMovies() {
        this.movieFinder.findMovies(...);
        ...
    }
}
```

像@Autowired注解一样，@Inject也可以修饰field级别、方法级别和构造器参数级别。更进一步，你可以定义你自己的注入点作为Provider，允许依赖访问小范围或延迟访问其他bean通过Provider.get()方法。作为上面例子的变化：

```
import javax.inject.Inject;
import javax.inject.Provider;

public class SimpleMovieLister {

    private Provider<MovieFinder> movieFinder;
    @Inject
    public void setMovieFinder(Provider<MovieFinder> movieFinder) {
        this.movieFinder = movieFinder;
    }
    public void listMovies() {
        this.movieFinder.get().findMovies(...);
        ...
    }
}
```

如果你想要使用限定依赖的名称，那么可以使用 @Named 注解如下：

```
import javax.inject.Inject;
import javax.inject.Named;

public class SimpleMovieLister {

    private MovieFinder movieFinder;
    
    @Inject
    public void setMovieFinder(@Named("main") MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }
    // ...
}
```

与 @Autowired 相似，@Inject 可以与 java.util.Optional 或者 @Nullable 一起使用。因为 @Inject 没有 required 属性，因此这么用是更合适的。

```
public class SimpleMovieLister {
    @Inject
    public void setMovieFinder(Optional<MovieFinder> movieFinder) {
        ...
    }
}
```

```
public class SimpleMovieLister {
    @Inject
    public void setMovieFinder(@Nullable MovieFinder movieFinder) {
        ...
    }
}
```



