#### Naming autodetected components

当一个组件作为扫描进程的一部分作为探测，他的bean的名字将会通过BeanNameGenerator生成然后被扫描器所知。默认的，任何的spring策略注解（@Component、@Repository、@Service和@Controller）都包含一个name值可以定义相关的bean定义

如果这样的注解不包含name的值（例如通过自定义过滤器发现的），默认的bean的名字生成将返回首字母小写的类全限定名。例如，如果下面的两个组件被探测到，那名字将会是myMoveLister和moveFinderImple：

```
@Service("myMovieLister")
public class SimpleMovieLister {
    // ...
}
```

```
@Repository
public class MovieFinderImpl implements MovieFinder {
    // ...
}
```

>**Note**

> 如果你不希望默认的命名策略，你可以提供自定义的命名策略。首先，实现BeanNameGenerator接口，然后保证包含一个默认无参构造器。然而，在配置扫描器时提供全限定的类名。

```
@Configuration
@ComponentScan(basePackages = "org.example", nameGenerator = MyNameGenerator.class)
public class AppConfig {
    ...
}
```

```
<beans>
    <context:component-scan base-package="org.example"
name-generator="org.example.MyNameGenerator" />
</beans>
```

作为通用的规则，考虑使用注解定义那么淡其他组件或许定义明显的引用。另一方面，自动生成的名字是可以胜任需求的，当容器配置bean时。

