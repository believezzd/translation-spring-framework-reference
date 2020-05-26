### 8.5 The ResourceLoaderAware interface

ResourceLoaderAware接口是一个特定的标注接口，定义object由ResourceLoader引用来提供。

```
public interface ResourceLoaderAware {
    void setResourceLoader(ResourceLoader resourceLoader);
}
```

当一个类实现了ResourceLoaderAware并且部署在应用上下文中（作为一个spring管理的bean），将会被应用上下文识别为ResourceLoaderAware。应用上下文将会调用setResourceLoader(ResourceLoader)，将其作为参数（记住，所有的应用上下文在spring中都实现了ResourceLoader接口）。

当然，每个应用上下文都是ResourceLoader，bean可以实现ApplicationContextAware接口然后使用提供的应用上下文直接来加载资源，但是通常情况，最好使用特定的ResourceLoader接口如果有这种需求。代码将和资源加载接口耦合，可以考虑为一个实用的接口，不是整个spirng应用上下文接口。

在spring2.5中，你可以依赖资源加载器的自动注入代替实现ResourceLoaderAware接口。传统的构造器和通过类型注入的模式（在7.4.5节描述，自动加载处理）可以支持提供依赖ResourceLoader的类型作为一个构造器参数或set方法的参数。更加方便的（包括自动注入属性和多个参数方法），考虑使用新的基于注解的自动注入特性。在这种情况，ResourceLoader将会自动注入到属性、构造器参数或方法参数需要ResourceLoader类型的属性、构造器或方法在@Autowired注解。详见7.9.2“@Autowired”。