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