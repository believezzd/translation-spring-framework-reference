#### Configuration metadata

通过前面的图展示，spring IOC容器处理一系列配置的元数据，这些元数据代表了你的应用是如何告诉spring容器来初始化实例、配置和集合应用的object。

配置元数据是以一种比较简单的XML格式，也是本章节会介绍的用于阐述spring IOC容器中关键的信息和特性的方式。

>Note
>配置元数据不仅仅只能通过xml的方式进行书写。Ioc容器本身已经与配置元数据书写方式解耦。最近许多开发者选择使用基于java的方式进行为应用进行配置。

Spring容器的其他配置方式，见
- 基于注解的配置：spring2.5支持注解配置元数据。

- 基于java的配置：从spring3.0开始，许多spring JavaConfig工程中的特性已经加入了spring框架的核心包。以方便你在应用中通过java来定义bean而不是使用xml文件。如果需要使用这些新特性，见@Configuration、@Bean、@Import和@DependsOn注解。

spring的配置中至少有一个或多个bean的定义需要容器来管理。基于xml的配置元数据显示bean需要使用<bean>元素，并且顶层元素是<beans>。java配置通常使用在@Configuration的类中定义@Bean修饰的方法。

bean的定义取决组成你应用的实际object。通常你会定于服务层object、数据访问object（DAOs）、类似于Struts Action实例和类似于Hibernate SessionFactory、JMS队列等的基础object。通常不需要配置细粒度的领域对象在容器中，因为这通常是数据访问层和业务逻辑来负责这些领域对象的创建与加载。然而你可以使用AspectJ来控制没有被IOC容器包含的外部bean。见在spring中使用AspectJ来依赖注入object。

面的例子展示了基于xml配置元数据的基本框架：

```
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
        
    <bean id="..." class="...">
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <bean id="..." class="...">
        <!-- collaborators and configuration for this bean go here -->
    </bean>
    
    <!-- more bean definitions go here -->
    
</beans>
```

字符串类型的id属性是你用来唯一定义bean的标识。class属性定义了bean的类型并且需要使用全限定的类名。在xml并没有说明如何引用外部bean。详见依赖。


















