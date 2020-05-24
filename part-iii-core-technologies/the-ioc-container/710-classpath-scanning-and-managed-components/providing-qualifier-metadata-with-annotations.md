#### Providing qualifier metadata with annotations

在7.9.4中讨论的@Qualifier注解，“使用qualifier微调基于注解的自动注入”。那节中的例子描述了@Qualifier注解的使用和自定义qualifier注解来实现微调控制当你处理自动注入时。因为这些例子是基于xml的bean定义，qualifier元数据通过bean定义来提供，使用在xml中的qualifier元素或元子元素。当扫描类路径用于自动探测组件，你在备选类中提供了qualifier元数据在类型级别。下面的三个例子讨论了这种技术：

```
@Component
@Qualifier("Action")
public class ActionMovieCatalog implements MovieCatalog {
    // ...
}
```

```
@Component
@Genre("Action")
public class ActionMovieCatalog implements MovieCatalog {
    // ...
}
```

```
@Component
@Offline
public class CachingMovieCatalog implements MovieCatalog {
    // ...
}
```

>**Note**

>作为基于注解的替代，记住注解元数据是和类定义本身相互关联的，使用xml允许多个相同类型的bean提供不同在他们的元数据中，因为元数据提供单个实例而不是单个类。
