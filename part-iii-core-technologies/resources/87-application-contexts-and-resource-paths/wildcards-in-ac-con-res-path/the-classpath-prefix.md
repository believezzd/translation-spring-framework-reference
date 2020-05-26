##### The classpath*: prefix

当构建一个基于xml的应用上下文，一个位置字符串可以使用特定的classpath*:前缀：

```
ApplicationContext ctx =
new ClassPathXmlApplicationContext("classpath*:conf/appContext.xml");
```

特定的前缀定义了所有的classpath资源匹配给定的名字（内部必须通过调用ClassLoader.getResources），并且合并到最后应用上下文定义中。

>**Note**
>通配符classpath依赖于类加载器中的getResources方法。作为大部分应用服务器现在支持他们自己的类加载器实现，行为是有区别的尤其是处理jar文件。一个简单的测试如果classpath*可以工作用于从jar中加载文件通过使用classpath: getClass().getClassLoader().getResources("<someFileInsideTheJar>")。尝试这样的测试需要同名但是替换两个不同的位置。为了防止返回不同的结果，检查应用服务器文档设置是否会影响类加载器的行为。

classpath*前缀也可以组合通过PathMatcher模式在剩余的路径中，例如classpath*:META-INF/*-beans.xml。这个例子中，解决方法是很简单的：调用ClassLoader.getResources()用于最后一个无通配符的路径片段来获得所有匹配的资源在类加载器中，然后相同的PathMatcher解决策略用于通配符子路径。