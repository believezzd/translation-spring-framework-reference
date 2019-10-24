对于核心容器有如下的增强:

- Spring现在在注入bean的时候，将泛型作为一种修饰符的形式。例如，你在使用Spring Data Repository时，你可以简单的这样使用：@Autowired Repository<Customer> customerRepository
- 如果你使用spring支持的元注解，你可以自定义注解，来暴露源注解的特定属性。
- 对象在注入到list和数组时可以指定顺序。同时支持@Order注解和Ordered接口。
- @Lazy注解也可以向@Bean一样用于injection points上。
- @Description注解的引入是为了让开发者使用Java-based配置。
- 一个通用的模型用于conditionally filtering beans已经被支持通过@Conditional注解。这个和@Profile类似，但是支持用户在开发中进行自定义。
- 基于CGLIB的代理类不在需要一个默认的构造器。作为spring框架内置的库——objenesis应经允许前面的功能可以实现。由于这个策略，在调用代理对象时不在需要构造器了。
- 可管理的时区支持在spring中已经存在了。举例来说，LocaleContext。