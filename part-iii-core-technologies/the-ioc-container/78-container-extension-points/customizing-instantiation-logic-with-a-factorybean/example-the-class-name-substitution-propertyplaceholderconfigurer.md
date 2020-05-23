##### Example: the Class name substitution PropertyPlaceholderConfigurer

你使用PropertyPlaceholderConfigurer来外部属性值从一个bean定义中在其他的文件使用标准的java属性形式。这样做可以在部署应用是自定义特定环境的属性，例如数据库地址和密码，而不再需要修改复杂的xml定义文件或容器的文件。

考虑下面基于xml配置元数据的片段，定义了一个使用占位符的DataSource。这个例子中属性配置是在一个外置的properties文件中的。在运行时，PropertyPlaceholderConfigurer将会替换DataSource的部分属性。需要被替换的属性都类似于${属性名}这样的形式，仿照了Ant、log4j和JSP EL的风格。

```
<bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
    <property name="locations" value="classpath:com/foo/jdbc.properties"/>
</bean>
<bean id="dataSource" destroy-method="close"
class="org.apache.commons.dbcp.BasicDataSource">
    <property name="driverClassName" value="${jdbc.driverClassName}"/>
    <property name="url" value="${jdbc.url}"/>
    <property name="username" value="${jdbc.username}"/>
    <property name="password" value="${jdbc.password}"/>
</bean>
```

实际的值来源于另外一个标准的Java的属性文件格式：

```
jdbc.driverClassName=org.hsqldb.jdbcDriver
jdbc.url=jdbc:hsqldb:hsql://production:9002
jdbc.username=sa
jdbc.password=root
```

在运行时，${jdbc.username}将会由sa来替换，其他的也会根据key的匹配来进行替换。PropertyPlaceholderConfigurer会检查bean定义中属性的定义。另外，占位符的前缀和后缀是可以自定义的。

在spring2.5的context的命名空间中，允许使用一个专有的配置属性来设置占位符属性。一个或多个属性值可以通过逗号来分割，提供给location的属性值。

```
<context:property-placeholder location="classpath:com/foo/jdbc.properties"/>
```

PropertyPlaceholderConfigurer的功能并不只限于加载你定义的属性文件。默认情况下，他会检测java系统属性如果配置的属性在properties中无法找到时。你可以自定义查找的属性通过设置systemPropertiesMode属性并指定以下某个值。

* never(0): 不检测系统属性
* fallback(1): 如果配置文件中不存在则检查系统属性，这是默认值
* override(2): 先检查系统属性，然后检查自定义配置文件。并且允许系统属性可以覆盖其他属性。

参考PropertyPlaceholderConfigurer的javadocs来获取更多信息。

>**Tip**

> 你可以使用PropertyPlaceholderConfigurer来替代类名，当你在运行时需要获得一个特殊的实现时这个功能很有用。例如：

>```
><bean >class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
>    <property name="locations">
>        <value>classpath:com/foo/strategy.properties</value>
>    </property>
>    <property name="properties">
>        <value>custom.strategy.class=com.foo.DefaultStrategy</value>
>    </property>
></bean>
>
><bean id="serviceStrategy" class="${custom.strategy.class}"/>
>```

> 如果在运行时不能指定一个有效的类，对于一个非延迟加载的bean 在 preInstantiateSingletons() 阶段，当它被创建的时候，会导致失败。


