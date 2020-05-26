##### Ant-style Patterns

当路径包含Ant风格的模式，如下

```
/WEB-INF/*-context.xml
    com/mycompany/**/applicationContext.xml
    file:C:/some/path/*-context.xml
    classpath:com/mycompany/**/applicationContext.xml
```

解决方法有点复杂但是定义过程试图支持通配符。他从路径中生产一个资源取决于最后一个非通配符的片段并且从中获得一个URL。如果这个URL不是jar：URL或包含特定的变量（例如zip：在WebLogic、wsjar在WebSphere中等等），从中获得一个java.io.File并且用于处理解决通配符通过该文件系统。防止是jar的URL，处理器从中获得java.net.JarURLConnection或手动解析jar的URL然后从其中解析通配符。

```

```