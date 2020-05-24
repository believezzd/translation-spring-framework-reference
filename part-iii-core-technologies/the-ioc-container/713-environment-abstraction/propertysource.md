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