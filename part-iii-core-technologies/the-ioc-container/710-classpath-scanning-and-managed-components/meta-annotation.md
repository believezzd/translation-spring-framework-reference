#### Meta-annotation

所有spring提供的注解都可以作为你代码中的元注解。一个元注解可以很简单的运用在另一个注解上。例如，@Service注解就在元注解@Component上。

```
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component // Spring will see this and treat @Service in the same way as @Component
public @interface Service {
    // ....
}
```

元注解也可以合并用于创建组合注解。例如，spring mvc中的@RestController注解就是@Controller和@ResponseBody的组合。

额外的，组合注解可以允许自定义元注解的属性。如果你希望使用元注解的部分属性时这个功能很有用。例如，spring的@SessionScope注解硬编码了session的范围名但是仍然允许自定义proxyMode。

```
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Scope(WebApplicationContext.SCOPE_SESSION)
public @interface SessionScope {
    /**
    * Alias for {@link Scope#proxyMode}.
    * <p>Defaults to {@link ScopedProxyMode#TARGET_CLASS}.
    */
    @AliasFor(annotation = Scope.class)
    ScopedProxyMode proxyMode() default ScopedProxyMode.TARGET_CLASS;
}
```

@SessionScope 可以不用声明 proxyMode 来使用：

```
@Service
@SessionScope
public class SessionScopedService {
    // ...
}
```

或者覆盖proxyMode的值如下

```
@Service
@SessionScope(proxyMode = ScopedProxyMode.INTERFACES)
public class SessionScopedUserService implements UserService {
    // ...
}
```

详见spring注解编程模型




