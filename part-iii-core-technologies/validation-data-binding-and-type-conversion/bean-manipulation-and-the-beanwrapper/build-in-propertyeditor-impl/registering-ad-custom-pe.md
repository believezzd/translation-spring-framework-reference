##### Registering additional custom PropertyEditors

当设置一个bean的属性为字符串时，spring的ioc容器会使用基本的JavaBean属性编辑器来将字符串转化为复杂的属性类型。spring会事先注册一些自定义的属性编辑器（例如，将一个类名表达为一个字符串设置到实际的class的object中）。此外，java的标砖JavaBeans属性编辑器的查询策略允许一个类的属性编辑器简单被命名并并在同一个包中定义，以便可以自动被发现。

如果需要注册其他的自定义属性编辑器则有一些策略可用。其中最手动的方法不是很方便也不被推荐使用，就是简单的使用ConfigurableBeanFactory接口的registerCustomEditor方法，假设你有一个BeanFactory的引用。另外，稍微有些方便的策略是使用特定bean工厂后处理器名为CustomEditorConfigurer。尽管bean工厂后处理器可以用于BeanFactory的实现，CustomEditorConfigurer有一个内置属性的设置，因此强烈推荐可以使用在ApplicationContext中，可以和其他bean一样以相似的方式部署并可以自动被探测和应用。

注意所有的bean工厂和应用上下文自动使用一定数量的内置属性编辑器，尽管他们使用的一些叫BeanWrapper用于处理属性转换。BeanWrapper注册的标准的属性编辑器在前一节中已经列出。此外应用上下文也可以覆盖和添加额外的编辑器用于处理资源的查找在管理以适应特定的应用上下文类型。

标准的JavaBeans属性编辑器实例可以用于将属性值表达为字符串用于实际复杂的属性类型中。bean的工厂后处理器，CustomEditorConfigurer可以方便用于支持应用上下文中的属性编辑器实例。

考虑一个用户类ExoticType和另一个类DependsOnExoticType需要ExoticType作为属性：

```
package example;
public class ExoticType {
    private String name;
    public ExoticType(String name) {
        this.name = name;
    }
}

public class DependsOnExoticType {
    private ExoticType type;
    public void setType(ExoticType type) {
        this.type = type;
    }
}
```

当被正确的设置好，我们希望指定属性的类型是字符串，PropertyEditor在后面会将其转化为ExoticType实例。

```
<bean id="sample" class="example.DependsOnExoticType">
    <property name="type" value="aNameForExoticType"/>
</bean>
```

PropertyEditor实现看上去是这样的：

```
// converts string representation to ExoticType object
package example;
public class ExoticTypeEditor extends PropertyEditorSupport {
    public void setAsText(String text) {
        setValue(new ExoticType(text.toUpperCase()));
    }
}
```

最后，我们使用应用上下文的CustomEditorConfigurer来注册新的属性编辑器，可以根据需要来使用：

```
<bean class="org.springframework.beans.factory.config.CustomEditorConfigurer">
    <property name="customEditors">
        <map>
            <entry key="example.ExoticType" value="example.ExoticTypeEditor"/>
        </map>
    </property>
</bean>
```

