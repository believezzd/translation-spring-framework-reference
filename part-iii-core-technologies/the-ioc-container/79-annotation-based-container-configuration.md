### 7.9 Annotation-based container configuration

```
对应spring来说，注解配置比xml配置好吗？

基于注解的配置介绍来源于这种方式是否比xml要好。简要回答是看情况。复杂点来说，每一种选择都各有利弊，并且通常取决于开发者决定哪个策略更适合。根据他们的定义，注解提供了很多他们定义中的上下文，来实现简单方便的配置。然而，xml文件可以在不修改源代码的情况下惊醒配置。一些开发者倾向于接近源代码，另外一些则认为注解会破坏pojo并且使得配置难以被控制。

不管哪一种选择，spring都可以使用甚至可以混合使用。可以通过java配置选项，spring也允许非侵入的方式，不触及目标源代码，所有的配置风格在spring tool suite中都支持。
```

xml设置的代替是基于注解的，将会依赖于后续的字节码而不是依赖与定义。不再使用xml来描述bean的定义，开发者将配置移入到类中，通过在类、方法、属性上使用注解来配置组件。在章节“例子：RequiredAnnotationBeanPostProcessor”中使用BeanPostProcessor来连接注解是spring的IOC容器常见的一种扩展方式。例如，spring2.0中介绍了使用@Required注解来声明必须的属性。spirng2.5允许相同的方式来驾驭spring的依赖注入。本质上，@Autowired注解提供了相同的功能在章节7.4.5中描述，“自动注入组件”有更细粒度的控制和更好的适应性。spring2.5也添加了JSR250中的注解，例如@PostConstruct和@PreDestroy。spring3.0中添加了JSR330中的注解（为java的依赖注入）包括javax.inject包中@Inject和@Named。详细内容见7.11。

>**Note**

> 注解注入在 XML 注入之前执行，因此后者的配置会覆盖前者。

通常，你可以注册独立的bean定义，但是他们也可以如下的定义，在基于xml的spring配置中。（注意需要包含context命名空间）：

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
</beans>
```

（潜在的注册了post-processors包括AutowiredAnnotationBeanPostProcessor、CommonAnnotationBeanPostProcessor、PersistenceAnnotationBeanPostProcessor及上面提及的RequiredAnnotationBeanPostProcessor）

>**Note**

> <context:annotation-config/>只查找在相同应用上下文定义的bean的注解。这也意味着，如果你在用于DispatcherServlet的WebApplicationContext上使用了<context:annotation-config/>，他只会检查你控制器中使用的@Autowired，而不是你的服务层。详细内容见22.2章节“DispatcherServlet”。

