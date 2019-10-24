singleton bean是只有一个被共享的实例被管理，所有通过id或其他形式匹配的请求都会由同一个在spring容器中的bean实例返回。

也就是说，当你定义了一个单例范围的bean，spring的IOC容器只会通过bean的定义明确创建一个实例。这个单例实例保存在这样单例bean的缓存中，并且所有的子请求和通过name的引用都会返回被缓存的实例。

![Singleton Scope](/graph/Figure 7.5.1 Singleton Scope.png)

spring有关单例bean的概念来源于GoF模式那本书中有关单例模式的想法。GoF单例中硬编码了object以至于ClassLoader只能创建类的一个实例。spring的单例是对每个容器一个bean的最好诠释。这意味着如果你在单一的spring容器中定义了一个class，容器会通过bean的定义只创建一个实例。spring的默认bean的范围是单例。如果你希望在xml中配置单例，你可以这样写，例如：

```
<bean id="accountService" class="com.foo.DefaultAccountService"/>

<!-- the following is equivalent, though redundant (singleton scope is the default) -->

<bean id="accountService" class="com.foo.DefaultAccountService" scope="singleton"/>
```