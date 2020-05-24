##### Conditionally include @Configuration classes or @Bean methods

基于一些结构系统的状态通过条件开启或关闭一个@Configuration类或独立的@Bean方法是很有用的。一个通用的例子就是使用@Profile注解来激活bean通过该特定的profile被允许在spring的环境中（见7.13.1节，“bean定义profiles”）

@Profile注解实现使用了更加方便的注解叫@Conditional。@Conditional注解暗示特定org.springframework.context.annotation.Condition的实现应该在@Bean被注册之前。

实现Condition接口只要简单的提供matches方法并返回一个true或false。例如，下面是使用@Profile的Condition实现。

```
@Override
public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
    if (context.getEnvironment() != null) {
        // Read the @Profile annotation attributes
        MultiValueMap<String, Object> attrs =
        metadata.getAllAnnotationAttributes(Profile.class.getName());
        if (attrs != null) {
            for (Object value : attrs.get("value")) {
                if (context.getEnvironment().acceptsProfiles(((String[]) value))) {
                    return true;
                }
            }
            return false;
        }
    }
    return true;
}
```

参考@Conditional的javadocs获得更多信息