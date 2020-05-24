##### Activating a profile

现在我们更新我们配置，我们依然需要告诉spring哪个profile是激活的。如果我们现在直接启动应用，我们会看到NoSuchBeanDefinitionException异常被抛出，因为容器没有找到spring的bean和数据源。

激活一个profile有几种方法，但是最直接的方法是通过Environment的API编程，通过ApplicationContext：

```
AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext();
ctx.getEnvironment().setActiveProfiles("development");
ctx.register(SomeConfig.class, StandaloneDataConfig.class, JndiDataConfig.class);
ctx.refresh();
```

额外的，profile可以通过spring.profiles.active属性进行激活，可以作为系统环境变量、JVM系统属性、servlet上下文属性在web.xml中或作为一个实体在JNDI中（见7.13.3节“PropertySource抽象”）。在集成测试中，激活profile可以通过定义@ActiveProfiles注解在spring-test模块中（见章节“使用环境profile的上下文配置”）。

注意profiles不是二选一的命题，可以一次激活多个profiles。使用编程的方式，简单的在setActiveProfiles方法中提供多个profile名字就可以，该方法接受多个字符串参数：

```
ctx.getEnvironment().setActiveProfiles("profile1", "profile2");
```

相应的，spring.profiles.active也可以接受逗号分隔的profile名字的列表：

```
-Dspring.profiles.active="profile1,profile2"
```

