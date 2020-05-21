##### Aliasing a bean outside the bean definition

在定义bean的时候，你可以提供多个name，通过id指定唯一的一个name，然后通过name指定多个name。这些name可以别名为相同的bean，并且在很多场景中有用，比如允许每一个应用通过别名引用这个由组件自己定义的这个依赖。

指定bean的别名是不够的。然而有时候需要为在各处定义的bean引入别名。在一个大型的系统中，配置被切分通过子系统，每个子系统有其自己的object的定义。在基于xml的元数据配置中，你可以使用<alias/>标签来完成别名的定义。

```
<alias name="fromName" alias="toName"/>
```

在这种情况下，在同一个容器中叫fromName，但是在使用了别名之后，toName指向了bean。

例如，子系统A的配置元数据指向数据源通过subsystemA-dataSource。子系统B的配置元数据指向数据源通过subsystemB-dataSource。主应用集成两个子系统，并且通过myApp-dataSource来指向DataSource。为了让这三个source指向同一个配置元数据，你可以如下所示定义配置元属性。

```
<alias name="subsystemA-dataSource" alias="subsystemB-dataSource"/>
<alias name="subsystemA-dataSource" alias="myApp-dataSource" />
```

现在主应用可以指向DataSources通过一个唯一的名字，并且保证不会产生冲突，（更加有效地创建命名空间）指向相同的bean。

>java配置
>如果你使用代码配置，@Bean注解可以被使用用于提供别名，详见7.12.3章节，使用@Bean注解。