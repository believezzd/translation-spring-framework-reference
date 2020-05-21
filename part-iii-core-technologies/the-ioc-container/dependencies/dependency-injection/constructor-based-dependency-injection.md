##### Constructor-based dependency injection

基于构造器的注入是通过容器调用带有参数的构造方法来实现的，每个参数代表一个依赖。调用一个有参数的静态工厂方法来构建bean和这个有些类似，而且和静态工厂方法对待参数的形式类似。下面的例子展示了一个类使用构造器的依赖注入。注意这个类并没有什么特殊，只是一个POJO而且和容器的任何接口、类和注解没有依赖关系。

```
public class SimpleMovieLister {
    // the SimpleMovieLister has a dependency on a MovieFinder
    private MovieFinder movieFinder;
    
    // a constructor so that the Spring container can inject a MovieFinder
    public SimpleMovieLister(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }
    
    // business logic that actually uses the injected MovieFinder is omitted...
}
```