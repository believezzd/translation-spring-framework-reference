### 7.7 Bean definition inheritance

一个bean的定义包含了很多配置信息，包括构造器参数、参数值和容器特定的信息例如初始化方法、静态工厂方法名等等。子bean定义继承来自父bean的定义。子bean定义可以根据需要覆盖一些值或添加其他的值。使用子bean和父bean的定义可以减少很多键盘输入。更加有效，可以作为一种模板。

如果你编程使用ApplicationContext接口，子bean定义由ChildBeanDefinition类代表。大部分用户不需要在这个级别编程来替代ClassPathXmlApplicationContext中定义的bean定义信息。当你使用基于XML的配置元数据时，你可以通过使用parent属性来定义子bean定义，尤其是父bean作为这个值的属性值。

```
<bean id="inheritedTestBean" abstract="true"
class="org.springframework.beans.TestBean">
    <property name="name" value="parent"/>
    <property name="age" value="1"/>
</bean>

<bean id="inheritsWithDifferentClass"
class="org.springframework.beans.DerivedTestBean"
parent="inheritedTestBean" init-method="initialize">
    <property name="name" value="override"/>
    <!-- the age property value of 1 will be inherited from parent -->
</bean>
```

如果子bean不定义class属性，则使用其父bean的class定义，当然也可以覆盖父bean的定义。在下面的例子中，子bean的类必须和父bean相匹配，也就是必须可以接收父bean的属性值。

子bean可以继承范围、构造器参数值、属性值和覆盖父类的方法通过添加新的属性值。任何范围、初始化方法、销毁方法和静态工厂方法设置都可以从父bean的设置中覆盖。

剩下的设置则从子bean定义中获得：依赖、自动注入模式、依赖检查、单例、延迟加载。

前面的例子明确声明了父bean的定义是抽象的，因为使用了抽象的属性定义。如果父bean定义没有定义class属性，需要将bean定义为抽象，如下：

```
<bean id="inheritedTestBeanWithoutClass" abstract="true">
    <property name="name" value="parent"/>
    <property name="age" value="1"/>
</bean>

<bean id="inheritsWithClass" class="org.springframework.beans.DerivedTestBean"
parent="inheritedTestBeanWithoutClass" init-method="initialize">
    <property name="name" value="override"/>
    <!-- age will inherit the value of 1 from the parent bean definition-->
</bean>
```

上面的父bean是不可以被实例化的因为他不完整并且也明确定义了他是抽象的。当一个像这样定义的抽象，通常作为一个纯粹的模板bean定义作为一个父bean来服务子bean。如果试图使用这样的父bean，或作为另一个bean属性的指向或者使用getBean方法使用父bean的id会返回错误。同样，容器内部的preInstantiateSingletons方法会忽略抽象bean的定义。