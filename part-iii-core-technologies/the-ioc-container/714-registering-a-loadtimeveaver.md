### 7.14 Registering a LoadTimeVeaver

Spring使用的LoadTimeWeaver用于动态转换类当类加载到java虚拟机的时候。

允许加载时注入可以在@Configuration类上使用@EnableLoadTimeWeaving注解：

```
@Configuration
@EnableLoadTimeWeaving
public class AppConfig {
}
```

也可以在xml配种使用context:load-time-weaver元素：

```
<beans>
    <context:load-time-weaver/>
</beans>
```

一旦为ApplicationContext配置了这个选项。任何一个bean可以实现LoadTimeWeaverAware接口，他们接受一个load-time weaver的实例。在组合使用spring的JPA支持时很有用当load-time weaving可以对JPA类是必要的。详见LocalContainerEntityManagerFactoryBean的javadocs。关于AspectJ load-time weaving，见11.8.4节，“在spring框架中使用AspectJ实现Load-time weaving”。

