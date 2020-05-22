###### Choosing the type of proxy to create

默认的，spring容器为使用<aop:scoped-proxy/>标记的bean创建代理，基于cglib来创建代理。

>**Note**

>cglib代理只能注入到公共的方法调用中！不要在这样的代理上调用非public的方法，他们不会委托给实际的范围的目标对象。

相对的，你可以配置spring容器为这样的bean创建标准的基于接口的JDK，通过在<aop:scoped-proxy/>元素中设置proxy-target-class属性为false。使用基于接口的JDK意味着你不需要在你应用中加入额外的库来影响代理。然而，这也意味着你的范围bean必须实现至少一个接口并且所有相关的被注入的bean必须应用其中一个接口的bean。

<!-- DefaultUserPreferences implements the UserPreferences interface -->
<bean id="userPreferences" class="com.foo.DefaultUserPreferences" scope="session">
    <aop:scoped-proxy proxy-target-class="false"/>
</bean>

<bean id="userManager" class="com.foo.UserManager">
    <property name="userPreferences" ref="userPreferences"/>
</bean>

有关基于类或基于接口的代理的更多信息，见11.6章节“Proxying mechanisms”

