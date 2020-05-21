##### Setter-based dependency injection

基于setter方法的依赖注入，是容器首先调用无参数的构造方法或无参数的静态工厂方法实例bean之后调用setter方法来最终完成bean的初始化。

下面的例子就只使用了基于setter的依赖注入。这个类时符合java的。这是一个POJO并且没有依赖容器特定的接口、基准类或注解。

```
public class SimpleMovieLister {
    // the SimpleMovieLister has a dependency on the MovieFinder
    private MovieFinder movieFinder;
    
    // a setter method so that the Spring container can inject a MovieFinder
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }
    
    // business logic that actually uses the injected MovieFinder is omitted...
}
```

ApplicationContext对于他管理的bean支持构造器注入和setter方法注入。基于setter的依赖注入也支持和构造器注入一同使用。你使用BeanDefinition的形式来配置依赖，配合PropertyEditor一同使用来转化属性从一种形式到另一种。然而，大部分spring的用户不会直接这样操作这些类（例如：编程的方式）而是使用xml定义bean、或使用注解修饰组件（例如：修饰类的@Component或@Controller等等）、或基于java的@Configuration类中的@Bean方法。这些资源会在内部转换为BeanDefinition的实体被spring的IOC容器全部加载。

>基于构造器还是基于setter方法更好？

>虽然可以混合使用了构造器注入和setter方法的注入，但是更好的方式是，将构造器注入作为必选依赖、setter方法或配置方法作为可选项依赖。在set方法上可以使用@Required注解会使得这个属性成为必要的依赖。

>spring的小组成员建议使用构造器注入，因为这样可以使得组件成为不可变object，并且可以确保依赖不为null。而且构造器注入返回给客户端代码的总是一个已经初始化好的状态。附注，如果构造器的参数过多也不是很好，这样一个class的压力会很大，建议将属性拆分重构。

>建议如果可以提供默认值的话可以考虑使用set方法的注入。另外，在使用依赖的时候必要的非空检查是需要的。一种使用set方法注入的好处就是可以在晚些的时候注入属性。JMX的Bean就是在编译时使用set方法注入的好例子。

>使用依赖注入的方式依赖于特殊的类。有时，处理第三方的类时，如何处理由你来选择。例如，一个第三方类并未提供set方法，那么构造器注入就是唯一可用的依赖注入方法。