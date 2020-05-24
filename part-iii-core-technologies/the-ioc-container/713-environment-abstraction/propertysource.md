#### @PropertySource

@PropertySource注解提供了一个简单的方式用于向spring的环境中添加属性源码。

提供一个文件app.properties文件包含key-value对如：testbean.name=myTestBean，下面的@Configuration类使用了@PropertySource注解以如下的方式调用了testBean.getName方法，将会返回myTestBean。

```
@Configuration
@PropertySource("classpath:/com/myco/app.properties")
public class AppConfig {
    @Autowired
    Environment env;
    
    @Bean
    public TestBean testBean() {
        TestBean testBean = new TestBean();
        testBean.setName(env.getProperty("testbean.name"));
        return testBean;
    }
}
```

任何在@PropertySource资源中的占位符可以被属性源码在注册时被处理，如下：

```
@Configuration
@PropertySource("classpath:/com/${my.placeholder:default/path}/app.properties")
public class AppConfig {

    @Autowired
    Environment env;
    
    @Bean
    public TestBean testBean() {
        TestBean testBean = new TestBean();
        testBean.setName(env.getProperty("testbean.name"));
        return testBean;
    }
}
```

假设"my.placeholder"代表其中一个已经被注册的属性，例如系统参数或环境变量，那占位符将替换为相应的值。如果没有，则"default/path"将被作为默认值。如果灭有默认值可以处理，将会抛出非法参数的异常。

>Note

>@PropertySource注释根据Java 8约定是可重复的。然而,所有此类@PropertySource注释需要在同一级别声明:直接在配置类或作为同一自定义注释中的元注释。混合的直接不建议使用注释和元注释，因为直接注释会有效覆盖元注释。
