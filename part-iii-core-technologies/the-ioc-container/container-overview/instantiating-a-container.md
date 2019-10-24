实例化一个spirng IOC容器是简单的。提供给ApplicationContext构造器的实际的资源字符串允许容器可以在本地文件系统或java classpath或其他中定位配置元数据。

```
ApplicationContext context =
    new ClassPathXmlApplicationContext(new String[] {"services.xml", "daos.xml"});
```

>Note
>学习到现在，你或许想更深入知道spring的资源抽象，可以看第8章节的描述，提供了方便的方法从定义的URI表达式中读取输入流。另外，使用在构造器的使用说明可以看8.7章节，“应用上下文和资源路径”。

面的例子展示了服务层object（services.xml）配置文件：

```
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- services -->
    <bean id="petStore" class="org.springframework.samples.jpetstore.services.PetStoreServiceImpl">
        <property name="accountDao" ref="accountDao"/>
        <property name="itemDao" ref="itemDao"/>
        <!-- additional collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions for services go here -->

</beans>
```

下面的例子展示了数据访问objcet的daos.xml文件:

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="accountDao"
        class="org.springframework.samples.jpetstore.dao.jpa.JpaAccountDao">
        <!-- additional collaborators and configuration for this bean go here -->
    </bean>

    <bean id="itemDao" class="org.springframework.samples.jpetstore.dao.jpa.JpaItemDao">
        <!-- additional collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions for data access objects go here -->

</beans>
```

在上面的例子中，服务层包含PetStoreServiceImpl类和两个数据访问object，类型是JpaAccountDao和JpaItemDao（基于独立的JPA ORM标准）。name元素指向JavaBean属性的名字，ref元素指向另一个bean的定义。id和ref元素之间的关联表达了相关object的合作。配置object依赖的详情见依赖。