#### CustomAutowireConfigurer

CustomAutowireConfigurer是一个BeanFactoryPostProcessor，允许你注册你自定义的qualifier注解类型，尽管他们没有使用spring的@Qualifier注解修饰。

```
<bean id="customAutowireConfigurer"
class="org.springframework.beans.factory.annotation.CustomAutowireConfigurer">
    <property name="customQualifierTypes">
        <set>
            <value>example.CustomQualifier</value>
        </set>
    </property>
</bean>
```