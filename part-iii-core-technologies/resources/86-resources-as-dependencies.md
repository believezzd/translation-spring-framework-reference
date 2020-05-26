### 8.6 Resources as dependencies

如果bean自身通过某些动态进程来决定和支持资源路径，这样会更有意义，通过使用ResourceLoader接口加载资源。考虑加载一些模板的例子，需要特定的资源依赖于用户的角色。如果资源是静态的，则完全使用ResourceLoader接口是有意义的，bean保留的他需要的资源属性然后将会被注入。

然后注入这些属性是不重要的，所有的应用上下文注册和使用特定的JavaBeans PropertyEditor，可以将字符串路径转化为资源object。因此如果myBean有一个资源的template属性，可以按照如下简单的方式进行配置。

```
<bean id="myBean" class="...">
    <property name="template" value="some/resource/path/myTemplate.txt"/>
</bean>
```

注意资源没有前缀，因此因为应用上下文会被使用作为ResourceLoader，资源将会被加载，通过ClassPathResource、FileSystemResource或ServletContextResource（适合的）依赖于上下文的类型。

如果需要强制使用特定的资源，可以使用前缀。下面的例子展示了如果强制使用ClassPathResource和UrlResource（下面的被用于访问文件系统中的文件）

```
<property name="template" value="classpath:some/resource/path/myTemplate.txt">
<property name="template" value="file:///some/resource/path/myTemplate.txt"/>
```