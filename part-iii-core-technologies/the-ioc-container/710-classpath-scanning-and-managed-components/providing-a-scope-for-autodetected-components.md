#### Providing a scope for autodetected components

作为spring管理的通用的组件，大部分被探测的组件是单例的。然而，有时你需要不同的范围则需要使用@Scope注解。简单的使用这个注解提供范围的名字。

```
@Scope("prototype")
@Repository
public class MovieFinderImpl implements MovieFinder {
    // ...
}
```

>**Note**

> @Scope 注解只在具体的 bean 类(带有注解的组件)或者工厂方法(带有@Bean的方法上)内省。相反的，基于XML的配置，没有概念类级别的继承和继承层次结构是不相关的元数据的目的。

关于web特殊的范围，见7.5.4节“Request、session、global session、application和WebSocket范围”。就像之前的范围注解，你可以组合生成自己的范围注解使用Spring的原注解，例如使用原注解 @Scope("Prototype")来注解自定义注解，可能也需要声明一个自定义的范围代理模式。

>**Note**

>对于组件处理提供自定义的范围处理，实现ScopeMetadataResolver接口，并且抱着有一个无参的构造方法。然后在配置扫描器的时候提供全限定的类名

```
@Configuration
@ComponentScan(basePackages = "org.example", scopeResolver = MyScopeResolver.class)
public class AppConfig {
    ...
}
```

```
<beans>
    <context:component-scan base-package="org.example" scope-resolver="org.example.MyScopeResolver"/>
</beans>
```

当使用一个非单例的范围，需要为这个范围的object生成代理。这个理由描述在章节“范围bean作为依赖”。因为这个原因，一个被代理的属性在组件扫描元素中可见。这三个可取值为no、interface和targetClass。例如，下面的配置将实现标准的JDK动态代理：

```
@Configuration
@ComponentScan(basePackages = "org.example", scopedProxy = ScopedProxyMode.INTERFACES)
public class AppConfig {
    ...
}
```

```
<beans>
    <context:component-scan base-package="org.example" scoped-proxy="interfaces"/>
</beans>
```

