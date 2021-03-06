#### Fine-tuning annotation-based autowiring with @Primary

基于类型的注入可能会出现多种选择，因此建议更加细微的控制选择的过程。一种方式就是使用@Primary注解。@Primary指定一个特殊的bean对于多个候选值应该给出对某个单值的偏爱。如果只有一个primarybean存在，那将会被注入值。

让我们假设下面的配置定义了firstMovieCatalog作为主的MovieCatalog。

```
@Configuration
public class MovieConfiguration {
    @Bean
    @Primary
    public MovieCatalog firstMovieCatalog() { ... }
    
    @Bean
    public MovieCatalog secondMovieCatalog() { ... }
    // ...
    }
```

在上面的配置中，下面的MovieRecommender将会自动注入firstMovieCatalog。

```
public class MovieRecommender {
    @Autowired
    private MovieCatalog movieCatalog;
    // ...
}
```

相关的bean定义如下

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
    
    <bean class="example.SimpleMovieCatalog" primary="true">
        <!-- inject any dependencies required by this bean -->
    </bean>
    
    <bean class="example.SimpleMovieCatalog">
        <!-- inject any dependencies required by this bean -->
    </bean>
    
    <bean id="movieRecommender" class="example.MovieRecommender"/>
    </beans>
```










