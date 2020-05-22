##### Scoped beans as dependencies

spring的IOC容器不只管理你bean的实例化，也包括注入依赖。如果你希望将一个request的bean注入到另一个长期存活的bean，你可以选择使用AOP代理来替代范围的bean。你需要注入一个暴露相同接口的代理object作为范围的object，可以从相关范围（例如http请求中）检索实际的目标object并且对目标object方法进行委托。

>**Note**

>你也可以使用<aop:scoped-proxy/>在单例bean的定义之间，获得他的引用通过可以序列化的中间代理并且通过反序列化重新获得目标的bean实例。
>当在原型bean中定义<aop:scoped-proxy/>时，每个对共享代理的方法调用将会指向新创建的目标实例然后在被转发。
>并且，范围代理并不是唯一的生命期安全的访问shorter范围bean的方法。你也可以简单定义你的注入点（例如构造器或set方法注入或自动注入）作为ObjectFactory<MyTargetBean>，允许调用getObject来获得当前实例在你需要的任何时间——而不再需要保持一个实例或分开存储一个实例。
>在JSP-330中这种叫做提供者，使用Provider<MyTargetBean>定义并且和get方法调用相关联用于每次尝试的获取操作。详见JSR-330中的说明。

下面的配置虽然只有一行，对展示后面的原理很重要。

```
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">
        
    <!-- an HTTP Session-scoped bean exposed as a proxy -->
    <bean id="userPreferences" class="com.foo.UserPreferences" scope="session">
        <!-- instructs the container to proxy the surrounding bean -->
        <aop:scoped-proxy/>
    </bean>

    <!-- a singleton-scoped bean injected with a proxy to the above bean -->
    <bean id="userService" class="com.foo.SimpleUserService">
        <!-- a reference to the proxied userPreferences bean -->
        <property name="userPreferences" ref="userPreferences"/>
    </bean>
</beans>
```

你在范围bean之间插入了一个<aop:scoped-proxy/>子元素创建了一个这样的代理（详见“Choosing the type of proxy to create”和41章节，基于XML Schema的配置）。为什么在定义request、session、globalSession和自定义范围级别的bean时需要<aop:scoped-proxy/>元素呢？我们来检查一下下面这个单例的bean的定义对比一下你需要为上述的范围做什么（注意下面的userPreferences的bean定义是不完整的）

```
<bean id="userPreferences" class="com.foo.UserPreferences" scope="session"/>

<bean id="userManager" class="com.foo.UserManager">
    <property name="userPreferences" ref="userPreferences"/>
</bean>
```

在上面的例子中，单例bean的userManager依赖于session范围的bean是userPreferences。这里的重点是userManager的bean是单例的，这意味着这个bean在每个容器中只会初始化一次，并且它的依赖（对于userPreferences的bean来说也只有一次）也只会注入一次。这意味着userManager的bean将操作相同的userPreferences的object，就是原始被注入的那一个。

这并不是你想要的方式，当一个短生命的范围bean注入一个长生命周的范围bean中，例如一个单例bean需要一个session范围的bean时。相比之下，你的userManager在每个特定的http的session的生命周期中需要一个userPreferences对象。容器创建一个object老宝来相同的接口作为UserPreferences类（理想上是UserPreferences实例）可以抓取UserPreferences对象在一些范围中（http请求、session等）。容器将代理对象注入到userManager的bean中，但是引用不会意识到实际引用的是一个代理。在这个例子中，当UserManager实例调用UserPreferences的方法是，他实际调用了代理的方法。代理抓取实际UserPreferences对象从session中，然后委托给实际的UserPreferences对象来实现。

下面是你需要的正确完整的配置，当注入请求、session、全局session范围的bean到一个其他对象中：

```
<bean id="userPreferences" class="com.foo.UserPreferences" scope="session">
    <aop:scoped-proxy/>
</bean>

<bean id="userManager" class="com.foo.UserManager">
    <property name="userPreferences" ref="userPreferences"/>
</bean>
```


