#### ApplicationContextAware and BeanNameAware

当ApplicationContext创建了一个object实例实现了org.springframework.context.ApplicationContextAware接口，那么这个实例提供了ApplicationContext的一个引用。

```
public interface ApplicationContextAware {
    void setApplicationContext(ApplicationContext applicationContext) throws BeansException;
}
```

这样bean可以手动手动编程控制ApplicationContext，通过ApplicationContext接口，或者将其转化为一个子类的实例，例如ConfigurableApplicationContext，提供了额外的功能。一个用法就是获得其他的bean。有时这个做法很有用。然而，通常你应该避免使用它，因为这会使得你的代码和spring耦合并且不符合IOC的风格，当bean作为属性提供的时候。ApplicationContext提供的其他方法访问文件资源、发布应用事件和访问消息资源。这些额外的特性会在7.15章节“ApplicationContext的其他功能”中介绍。

在spring2.5中，自动注入作为另一个获得ApplicationContext引用的替代。传统的构造器和通过类型注入的方式（在7.4.5章节“自动注入组件”）可以提供ApplicationContext类型的依赖作为一个构造器参数或set方法的参数。为了更加便利，包括可以注入的属性和多个参数方法，使用基于注解的自动注入特性。如果你这样做，ApplicationContext可以注入到属性、构造器参数或方法参数被期望ApplicationContext类型的参数如果属性、构造器或方法有一个@Autowired注解。更多信息见7.9.2章节“@Autowired”。

当ApplicationContext创建了一个类实现了org.springframework.beans.factory.BeanNameAware接口，这个类就可以根据name获得另一个bean的实例的定义。

```
public interface BeanNameAware {
    void setBeanName(String name) throws BeansException;
}
```


