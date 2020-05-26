### 8.4 The ResourceLoader

ResourceLoader接口意味着可以返回资源的实例。

```
public interface ResourceLoader {
    Resource getResource(String location);
}
```

所有应用上下文实现ResourceLoader接口，所以所有应用上下文都一个使用来获得的资源的实例。

当你调用getResource在特定的应用上下文，并且路径没有含有特定的前缀，你将会得到一个资源类型适合于特定的应用上下文。例如，例如下面的代码片段将会执行得到一个ClassPathXmlApplicationContext实例。

```
Resource template = ctx.getResource("some/resource/path/myTemplate.txt");
```

将会返回的是ClassPathResource：如果相同的方法被执行通过FileSystemXmlApplicationContext实例，你会得到一个FileSystemResource。对于一个web应用上下文，你会得到一个ServletContextResource。

通用，你可以加载资源以适当的方式通过特定的应用上下文。

另一方面，你也可以强制使用ClassPathResource，不管应用上下文的类型，定义特定的classpath前缀：

```
Resource template = ctx.getResource("classpath:some/resource/path/myTemplate.txt");
```

相似的，你也可以强制使用UrlResource通过使用任意标准的java.net.URL前缀：

```
Resource template = ctx.getResource("file:///some/resource/path/myTemplate.txt");
Resource template = ctx.getResource("https://myhost.com/resource/path/myTemplate.txt");
```

下面的表格总结了将字符串转化为资源的策略

Prefix | Example | Explanation
-- | -- | --
classpath: | classpath:com/myapp/config.xml | Loaded from the classpath.
file: | file:///data/config.xml | Loaded as a URL, from the filesystem.
http: | https://myserver/logo.png | Loaded as a URL.
(none) | /data/config.xml | Depends on the underlying ApplicationContext.




