### 8.2 The Resource interface

spring的资源接口希望是比较好的接口对于低级资源的抽象。

```
public interface Resource extends InputStreamSource {
    boolean exists();
    boolean isOpen();
    URL getURL() throws IOException;
    File getFile() throws IOException;
    Resource createRelative(String relativePath) throws IOException;
    String getFilename();
    String getDescription();
}
```

```
public interface InputStreamSource {
    InputStream getInputStream() throws IOException;
}
```

一些Resource接口中重要的方法是：

* getInputStream():定位和打开资源，返回读取资源的输入流。每次调用都会返回一个新的输入流。需要调用和关闭流。

* isOpen():返回一个boolean暗示这个资源是否已经被一个打开的流在处理。如果是true，输入流不能读取多次并且必须读取一次然后关闭以防止资源泄漏。如果是false则所有正常的资源实现，除了InputStreamResource。

* getDescription(): 返回资源的描述，用于错误输出当使用资源时。通常是全限定文件名或资源的实际URL。

其他方法允许你获得一个实际的URL或文件object代表资源（如果潜在的实现是可用的，并且支持这些功能）

资源抽象用于扩展spring自身，作为一个参数类型在许多方法签名中当方法需要资源时。其他方法在spring的一些api中（例如不同ApplicationContext实现的构造器参数），接收一个字符串或简单的形式用于创建Resource用于上下文的实现，或通过前缀在字符串路径中，允许调用特定的资源实现当创建和使用时。

当通过spring大量使用Resource接口时，当作为一个普通的类时也很有用在你的代码中，用于访问资源，即使当你的代码不知道或关心spring的任何部分。这样你的代码和spring结合，只是和小部分类结合，可以用于替代URL并且和其他库相同，为了你要达到的目的。

意识到资源抽象不会功能性的替代是重要的，他只是包装了一些必要的。例如，一个URLResource包装了URL，只要使用包装类就可以工作。

