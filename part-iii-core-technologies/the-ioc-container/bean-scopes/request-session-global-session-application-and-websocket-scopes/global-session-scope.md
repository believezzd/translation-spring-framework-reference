##### Global session scope

考虑下面的bean定义:

```
<bean id="userPreferences" class="com.foo.UserPreferences" scope="globalSession"/>
```

全局的session范围类似于标准的http的session范围（上面描述过），并且只在portlet-based的web应用上下文中有效。portlet规范定义全局session的用处在共享所有的portlet来创建一个单一的portlet的web应用。定义为globalSession范围的bean在global portlet Session中被绑定。

如果你写一个标准的基于servlet的web应用，并且你定义了一个或过个bean使用了globalSession范围，那么使用标准的http的session范围也不会有问题。