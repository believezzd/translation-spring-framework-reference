##### Example: the PropertyOverrideConfigurer

PropertyOverrideConfigurer，类似于PropertyPlaceholderConfigurer，但是与后者相比，前者对于bean属性可以有缺省值或者根本没有值。如果起覆盖作用的 Properties文件没有某个bean属性的内容，那么缺省的上下文定义将被使用。

注意bean工厂的定义并不会意识到被覆盖，所以仅仅察看XML定义文件并不能立刻明显地知道覆盖配置是否被使用了。在有多个PorpertyOverrideConfigurer对用一个bean属性定义了不同的值的时候，最后一个将取胜（取决于覆盖的机制）

属性配置文件如下：

```
beanName.property=value
```

例如：

```
dataSource.driverClassName=com.mysql.jdbc.Driver
dataSource.url=jdbc:mysql:mydb
```

这个例子文件中的定义可以叫dataSource，包含driver和url属性。

组合属性名也是支持的，每一个部分是final的且非空（推测已经被构造器初始化）在这个例子中

```
foo.fred.bob.sammy=123
```

foo的fred属性的bob属性的sammy属性被设置成为123

>**Note**

> 定义的值通常是字面值，他们不能转化为bean的引用。这个规定也适用于在xml中bean定义的原始值的引用

使用spring2.5的context命名空间，你一个可以这样配置属性的覆盖。

```
<context:property-override location="classpath:override.properties"/>
```



