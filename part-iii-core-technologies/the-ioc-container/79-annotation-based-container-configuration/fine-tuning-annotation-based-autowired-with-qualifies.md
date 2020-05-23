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


