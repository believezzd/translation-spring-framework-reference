#### Constructing application contexts

一个应用上下文构造器（对于特定的应用上下文类型）通常需要一个字符串或字符串数组作为本地资源的路径例如xml文件用于定义上下文。

当路径没有前缀，特定的资源类型由路径来构建用于加载bean的定义，依赖于特定的应用上下文。例如，如果你如下一样创建了ClassPathXmlApplicationContext。

```
ApplicationContext ctx = new ClassPathXmlApplicationContext("conf/appContext.xml");
```

bean的定义将会从classpath中加载，将会使用ClassPathResource。但是如果你如下创建了FileSystemXmlApplicationContext：

```
ApplicationContext ctx =
new FileSystemXmlApplicationContext("conf/appContext.xml");
```

bean的定义将会从文件系统加载，取决于当前的工作路径。

注意使用特定了classpath前缀或标准的URL前缀在路径上将会覆盖定义创建的默认的资源类型。因此这个FileSystemXmlApplicationContext…

```
ApplicationContext ctx =
new FileSystemXmlApplicationContext("classpath:conf/appContext.xml");
```

实际会从classpath中加载bean的定义。然而，他依然是FileSystemXmlApplicationContext。如果使用了ResourceLoader，任何没有前缀的路径都会被作为是文件系统路径。



