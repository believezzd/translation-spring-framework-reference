#### Automatically detecting classes and registering bean definitions

spring可以自动探测模板类并且使用ApplicationContext来注册相应的bean定义。例如，下面的两个类就是可以被探测到的。

```
@Service
public class SimpleMovieLister {
    private MovieFinder movieFinder;
    @Autowired
    public SimpleMovieLister(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }
}
```

```
@Repository
public class JpaMovieFinder implements MovieFinder {
    // implementation elided for clarity
}
```

需要被自动探测的类需要注册相应的bean，你需要在@Configuration类上添加@ComponentScan注解，然后基本包路径就是为两个类的通用父包。（相应的，你可以使用逗号、分行和空格来分割父包的列表）

```
@Configuration
@ComponentScan(basePackages = "org.example")
public class AppConfig {
    ...
}
```

>**Note**

> 为了方便，上面的定义也可以用注解的属性来定义，如ComponentScan("org.example")

或者使用xml作为代替

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:context="http://www.springframework.org/schema/context"
xsi:schemaLocation="http://www.springframework.org/schema/beans
https://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
https://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan base-package="org.example"/>
</beans>
```

>**Tip**

> 使用<context:component-scan>暗示着允许<context:annotation-config>的功能。所以当使用<context:component-scan>时可以忽略<context:annotation-config>。

>**Note**

>用于扫描的包路径必须要以相对路径来表示。当你使用Ant打包JAR时，保证你不需要激活JAR task的仅文件开关。类路径在一些环境的安全路径中不会暴露，例如基于1.7.0_45或以上版本的单独app（需要在你的清单文件设置“信赖库”，见http://stackoverflow.com/questions/19394570/java-jre-7u45-breaks-classloader-getresources）

更进一步的，AutowiredAnnotationBeanPostProcessor和CommonAnnotationBeanPostProcessor都隐含的包含了当你使用组件扫描元素时。这就意味着这两个组件已经被扫描注入了，不需要在xml中提供bean的配置。

>**Note**

> 你可以关闭AutowiredAnnotationBeanPostProcessor和CommonAnnotationBeanPostProcessor的自动注册通过使用annotation-config属性，并指定属性值为false。







