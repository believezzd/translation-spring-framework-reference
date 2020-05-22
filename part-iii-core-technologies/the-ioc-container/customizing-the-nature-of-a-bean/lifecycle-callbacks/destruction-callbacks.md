##### Destruction callbacks

实现 org.springframework.beans.factory.DisposableBean 接口可以允许 bean 在销毁的时候进行回调。 DisposableBean 接口的定义了一个唯一方法:

```
void destroy() throws Exception;
```

建议你不使用 DisposableBean 接口因为不必要将你的代码和spring耦合。相反的，使用 @PreDestory 注解或者定义POJO的销毁方法。在基于xml的配置元数据中，你可以使用destroy-method属性定义无返回无参数的方法签名。在java配置中，你可以使用@Bean的initMethod属性，见“Receiving lifecycle callbacks”。例如，下面的例子：

```
<bean id="exampleInitBean" class="examples.ExampleBean" destroy-method="cleanup"/>
```

```
public class ExampleBean {
    public void cleanup() {
        // do some destruction work (like releasing pooled connections)
    }
}
```

实际上是与以下相同的:

```
<bean id="exampleInitBean" class="examples.AnotherExampleBean"/>
```

```
public class AnotherExampleBean implements DisposableBean {
    public void destroy() {
        // do some destruction work (like releasing pooled connections)
    }
}
```

但是不会与 Spring 耦合。

>**Tip**

> <bean> 的 destroy-method 属性可以配置为指定的值（inferred）来指示 Spring 自动检测 pulibc close 或 shutdown 方法在特定的 bean 类型中（任何继承 java.lang.AutoCloseable 或者 java.io.Closeable 接口的类）。这个特殊的值（inferred）可以设置到 <beans> 的 default-destory-method 属性中，来使整个 bean 的配置文件都生效（具体见 “Default initialization and destroy methods”）。注意，这个配置在 Java Config 中是默认的。
