##### Initialization callbacks

org.springframework.beans.factory.InitializingBean接口允许bean来实现初始化工作在所有必要的属性被容器设置之后。InitializingBean接口中只定义了一个方法：

```
void afterPropertiesSet() throws Exception;
```

建议你不使用InitializingBean接口因为不必要将你的代码和spring耦合。相反的，使用@PostConstruct注解或者定义POJO的初始化方法。在基于xml的配置元数据中，你可以使用init-method属性定义无返回无参数的方法签名。在java配置中，你可以使用@Bean的initMethod属性，见“Receiving lifecycle callbacks”。例如，下面的例子：

```
<bean id="exampleInitBean" class="examples.ExampleBean" init-method="init"/>
```

```
public class ExampleBean {
    public void init() {
        // do some initialization work
    }
}
```

与下面这个完全相同：

```
<bean id="exampleInitBean" class="examples.AnotherExampleBean"/>
```

```
public class AnotherExampleBean implements InitializingBean {

    public void afterPropertiesSet() {
        // do some initialization work
    }
}
```

但是不会与Spring耦合。