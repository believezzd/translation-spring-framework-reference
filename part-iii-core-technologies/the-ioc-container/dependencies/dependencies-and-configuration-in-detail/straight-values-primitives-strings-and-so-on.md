<property/>元素定义的内容或构造器参数定义的内容都是方便阅读的string形式。spring的转换服务将这些值转化为实际中需要的实际类型。

```
<bean id="myDataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
    <!-- results in a setDriverClassName(String) call -->
    <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql://localhost:3306/mydb"/>
    <property name="username" value="root"/>
    <property name="password" value="masterkaoli"/>
</bean>
```

下面的例子使用了p的命名空间用于简化xml的配置

```
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <bean id="myDataSource" class="org.apache.commons.dbcp.BasicDataSource"
        destroy-method="close"
        p:driverClassName="com.mysql.jdbc.Driver"
        p:url="jdbc:mysql://localhost:3306/mydb"
        p:username="root"
        p:password="masterkaoli"/>
</beans>
```

前面的例子更加简洁，打字错误会在运行时出现而不是编码的时候，除非你使用类似于IntelliJ IDEA或STS的IDE工具来支持创建bean定义的自动属性不全。这样的IDE是强烈推荐的。

你也可以这样定义一个java.util.Properties实例：

```
<bean id="mappings"
class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
    <!-- typed as a java.util.Properties -->
    <property name="properties">
        <value>
        jdbc.driver.className=com.mysql.jdbc.Driver
        jdbc.url=jdbc:mysql://localhost:3306/mydb
        </value>
    </property>
</bean>
```

spring容器通过使用JavaBeans PropertyEditor策略将<value/>元素中的文本内容加载到java.util.Properties的实例中。这是一个捷径，spring的小组特别喜欢使用这种内置的value元素。



