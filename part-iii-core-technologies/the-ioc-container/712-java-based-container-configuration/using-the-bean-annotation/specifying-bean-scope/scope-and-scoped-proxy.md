###### @Scope and scoped-proxy

spring提供了一种方便的方式通范围代理定义范围依赖。最简单的方法创建这样的代理是使用xml配置的<aop:scoped-proxy/>元素。使用@Scope注解配置bean也提供了相同的proxyMode属性。默认值是没有代理（ScopedProxyMode.NO），但是你可以定义为ScopeProxyMode.TARGET_CLASS或ScopeProxyMode.INTERFACES。

如果你从xml的参考文档看到范围代理的例子，他看起来是这样的：

```
// an HTTP Session-scoped bean exposed as a proxy
@Bean
@SessionScope
public UserPreferences userPreferences() {
    return new UserPreferences();
}
@Bean
public Service userService() {
    UserService service = new SimpleUserService();
    // a reference to the proxied userPreferences bean
    service.setUserPreferences(userPreferences());
    return service;
}
```