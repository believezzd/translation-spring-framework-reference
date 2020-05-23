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

AutowireCandidateResolver 确定自动注入候选者通过：
* 每个bean定义中自动注入的值
* beans元素中任何default-autowire-candidates的格式
* @Qualifier注解存在和使用CustomAutowireConfigurer注册的自定义注解

当多个bean可以匹配，使用如下方式来决定：这些候选的bean中有一个主要属性被设置为true。

