当你使用构造器创建一个bean，所有的正常类在Spring中都是可用且兼容的。这样，class不需要实现任何接口或需要特定的编码风格。只要正常定义类就可以。然而，依赖IOC的情况，你需要为其提供一个默认（或空）的构造器。

spring的ioc容器可以管理你需要让他管理的任何类，并不只限于管理JavaBean。大部分spring的用户为容器中的JavaBean提供一个默认无参构造器、set和get方法。你也可以在容器中包含exotic non-bean-style的类。例如，你需要使用一个连接池并且很明显这个和JavaBean没有关系，但是spring也可以管理。

使用基于xml的配置元数据，你可以这样定义你的bean类如下：

```
<bean id="exampleBean" class="examples.ExampleBean"/>
<bean name="anotherExample" class="examples.ExampleBeanTwo"/>
```

如果想了解向构造器提供参数的机理或在构造之后设置实例属性，详见Injecting Dependencies。