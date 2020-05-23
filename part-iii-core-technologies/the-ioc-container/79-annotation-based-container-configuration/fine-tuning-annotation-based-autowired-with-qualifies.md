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

qualifier也可以应用于类型集合，就像上面讨论的那样，例如，Set<MovieCatalog>。在这个例子中，所有匹配的bean都根据定义的qualifier来作为一个集合注入。这意味着qualifier并不一定是唯一的，他们更像是用于过滤的条件。例如，你可以定义多个MovieCatalog的bean使用相同的qualifier值为action，所有的这些都会注入@Qualifier("action")修饰的Set<MovieCatalog>。

>**Tip**

> 让限定符值根据目标bean名称进行选择，在类型匹配候选项中，在注入点甚至不需要@Qualifier注解。如果没有其他解决办法指示符(例如限定词或主标记)，对于非唯一依赖情况，Spring将将注入点名(即字段名或参数名)与目标bean名相匹配并选择同名的候选人(如果有的话)。

你可以创建自定义的qualifier注解。简单的定义一个注解并在定义中使用@Qualifier注解。

```
@Target({ElementType.FIELD, ElementType.PARAMETER})
@Retention(RetentionPolicy.RUNTIME)
@Qualifier
public @interface Genre {
    String value();
}
```

然后你使用自定义的qualifier注解来自动注入属性和参数：

```
public class MovieRecommender {

    @Autowired
    @Genre("Action")
    private MovieCatalog actionCatalog;
    
    private MovieCatalog comedyCatalog;
    
    @Autowired
    public void setComedyCatalog(@Genre("Comedy") MovieCatalog comedyCatalog) {
        this.comedyCatalog = comedyCatalog;
    }
    // ...
}
```

下一步，提供候选bean的定义信息。在bean元素下使用qualifier子元素定义类型和参数来匹配你自定义的注解。类型匹配注解类名的全限定名。或者，作为规范在不存在name冲突是，你可以使用类名。在下面的例子中描述了这两种方式。

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
        <qualifier type="Genre" value="Action"/>
        <!-- inject any dependencies required by this bean -->
    </bean>
    <bean class="example.SimpleMovieCatalog">
        <qualifier type="example.Genre" value="Comedy"/>
        <!-- inject any dependencies required by this bean -->
    </bean>
    
    <bean id="movieRecommender" class="example.MovieRecommender"/>
</beans>
```

在7.10节中，“类路径扫描和管理组件”，你会看到基于注解的来提供xml中的qualifier元数据。见7.10.8节，“使用注解提供qualifier元数据”。

在一些例子中，可以运行使用注解而不指定value。注解可以服务于通用的目标会更加有用，而且可以跨不同类型的依赖。例如，你可以提供线下的catalog并且可以在没有Internet的时候可用。首先定义简单的注解：

```
@Target({ElementType.FIELD, ElementType.PARAMETER})
@Retention(RetentionPolicy.RUNTIME)
@Qualifier
public @interface Offline {
}
```

然后用注解修饰field或property用于自动注入：

```
public class MovieRecommender {
    @Autowired
    @Offline
    private MovieCatalog offlineCatalog;
    // ...
}
```

现在bean的定义中只需要声明qualifier的类型就可以：

```
<bean class="example.SimpleMovieCatalog">
    <qualifier type="Offline"/>
    <!-- inject any dependencies required by this bean -->
</bean>
```

你也可以自定义qualifier注解来接受名字属性来代替简单的value属性。如果多个属性被定义在field或参数上，则一个bean定义必须满足所有的属性值才会被考虑自动注入。例如，考虑下面的这个注解的例子：

```
@Target({ElementType.FIELD, ElementType.PARAMETER})
@Retention(RetentionPolicy.RUNTIME)
@Qualifier
public @interface MovieQualifier {
    String genre();
    Format format();
}
```

Format这个枚举是这样定义的：

```
public enum Format {
    VHS, DVD, BLURAY
}
```

需要被自动注入的自定义qualifier必须包含两个属性：genre和format。

```
public class MovieRecommender {
    @Autowired
    @MovieQualifier(format=Format.VHS, genre="Action")
    private MovieCatalog actionVhsCatalog;
    @Autowired
    @MovieQualifier(format=Format.VHS, genre="Comedy")
    private MovieCatalog comedyVhsCatalog;
    @Autowired
    @MovieQualifier(format=Format.DVD, genre="Action")
    private MovieCatalog actionDvdCatalog;
    @Autowired
    @MovieQualifier(format=Format.BLURAY, genre="Comedy")
    private MovieCatalog comedyBluRayCatalog;
    // ...
}
```

最后，bean定义应该包含匹配的限定符值。这个例子还演示了可以使用bean元属性来代替 <qualifier/> 子元素。如果可以，qualifier和他的属性应该优先，但是自动注入策略考虑meta标签中提供的值，如果没有这样的qualifier被提供，例如下面例子中两个bean的定义。

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
        <qualifier type="MovieQualifier">
            <attribute key="format" value="VHS"/>
            <attribute key="genre" value="Action"/>
        </qualifier>
        <!-- inject any dependencies required by this bean -->
    </bean>
    
    <bean class="example.SimpleMovieCatalog">
        <qualifier type="MovieQualifier">
            <attribute key="format" value="VHS"/>
            <attribute key="genre" value="Comedy"/>
        </qualifier>
        <!-- inject any dependencies required by this bean -->
    </bean>
    
    <bean class="example.SimpleMovieCatalog">
        <meta key="format" value="DVD"/>
        <meta key="genre" value="Action"/>
        <!-- inject any dependencies required by this bean -->
    </bean>
    
    <bean class="example.SimpleMovieCatalog">
        <meta key="format" value="BLURAY"/>
        <meta key="genre" value="Comedy"/>
        <!-- inject any dependencies required by this bean -->
    </bean>
</beans>
```





