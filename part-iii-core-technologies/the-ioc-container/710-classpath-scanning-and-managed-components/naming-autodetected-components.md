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