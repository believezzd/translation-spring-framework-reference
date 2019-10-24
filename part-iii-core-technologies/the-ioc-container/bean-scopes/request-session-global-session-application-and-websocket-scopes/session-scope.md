考虑下面xml配置的bean的定义:

```
<bean id="userPreferences" class="com.foo.UserPreferences" scope="session"/>
```

pring在单一的http会话中通过使用userPreferences的bean定义来创建UserPreferences的实例。也就是说userPreferences在http的session级别有效。类似request范围的bean可以在初始化时改变状态，session范围的bean在不同的session中是看不见的，因为他们独立于http的session中。当http的session被销毁时，session范围的bean也会被抛弃。

使用注解配置和java的配置，使用@SessionScope注解的bean会被作为session范围的bean来使用。

```
@SessionScope
@Component
public class UserPreferences {

    // ...

}
```