#### Fine-tuning annotation-based autowired with qualifies

@Primary在有多个实例时确定一个主要的类型来注入是有效的方法。当需要在进一步控制选择过程时可以使用spring的@Qualifier注解。你可以使用qualifier的值与特定的参数连接起来，缩小类型的匹配范围以便于每个参数选择特定的bean。在最简单的例子中，可以指定描述的值。

```
public class MovieRecommender {
    @Autowired
    @Qualifier("main")
    private MovieCatalog movieCatalog;
    // ...
}
```

@Qualifier注解也可以独立修饰构造器参数或方法的参数：

```
public class MovieRecommender {
    private MovieCatalog movieCatalog;
    private CustomerPreferenceDao customerPreferenceDao;
    
    @Autowired
    public void prepare(@Qualifier("main")MovieCatalog movieCatalog,
    CustomerPreferenceDao customerPreferenceDao) {
        this.movieCatalog = movieCatalog;
        this.customerPreferenceDao = customerPreferenceDao;
    }
    // ...
}
```

相关的bean的定义如下。qualifier的属性值为main的bean与同名的构造器参数相互连接。

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:context="http://www.springframework.org/schema/context"
xsi:schemaLocation="http://www.springframework.org/schema/beans
https://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>
    
    <bean class="example.SimpleMovieCatalog">
        <qualifier value="main"/>
        <!-- inject any dependencies required by this bean -->
    </bean>
    
    <bean class="example.SimpleMovieCatalog">
        <qualifier value="action"/>
        <!-- inject any dependencies required by this bean -->
    </bean>
    
    <bean id="movieRecommender" class="example.MovieRecommender"/>
</beans>
```

对于一个后备匹配，bean的名字作为默认的qualifier值来处理。你可以使用id为main定义一个bean来代替内置的qualifier元素，也可以实现相同的匹配结果。然而，尽管你可以使用这种规范通过name来引用bean，@Autowired实际上是使用可选语义限定符实现类型的注入。这就意味着，即使是bean的名字的后备，也缩小了类型匹配的范围，而且不用特意指明bean的id。有意义的qualifier的值如main、EMEA或persistent，表达一个依赖于bean的id的组件的字符串，可以自动生成在上面例子中的那个匿名的bean的定义。

qualifier也可以应用于集合，就像上面讨论的那样，例如，Set<MovieCatalog>。在这个例子中，所有匹配的bean都根据定义的qualifier来作为一个集合注入。这意味着qualifier并不一定是唯一的，他们更像是用于过滤的条件。例如，你可以定义多个MovieCatalog的bean使用相同的qualifier值为action，所有的这些都会注入@Qualifier("action")修饰的Set<MovieCatalog>。





