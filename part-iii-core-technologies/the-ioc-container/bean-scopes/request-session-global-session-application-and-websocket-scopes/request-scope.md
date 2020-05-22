##### Request scope

考虑下面这个xml配置的bean定义：

```
<bean id="loginAction" class="com.foo.LoginAction" scope="request"/>
```

Spring容器在每次http请求时通过使用loginAction的定义创建一个新的LoginAction的实例。这就意味着loginAction的bean是http请求级别的。你可以根据需要创建多个bean的实例，并改变内部的状态，因为通过相同的loginAction的bean定义创建的实例看不到其他实例的状态改变，他们对一个request来说是独立的。如果一个请求处理完成，那么request范围的bean将会被销毁。

当使用注解配置或java配置时，@RequestScope注解修饰的bean将会被作为一个request范围的bean来使用。


```
@RequestScope
@Component
public class LoginAction {

    // ...

}
```