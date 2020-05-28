###### Using PropertyEditorRegistrars

spirng容器中其他注册属性编辑器的策略是创建和使用PropertyEditorRegistrar。当你需要在不同的环境中使用相同的一些属性编辑器时还是很有用的接口：创建一个相关的注册者然后在每个类中重复使用。PropertyEditorRegistrars和PropertyEditorRegistry接口同时使用，一个接口的实现是spring的BeanWrapper（和DataBinder）。PropertyEditorRegistrars当和CustomEditorConfigurer一起使用时是很方便的（在这里介绍），暴露了一个属性名为setPropertyEditorRegistrars：PropertyEditorRegistrars和CustomEditorConfigurer结合使用可以简单的在DataBinder和spring的MVC控制之间共享。进一步说，他避免了自定义编辑器对于同步的使用：PropertyEditorRegistrar试图对每个bean创建PropertyEditor的实例。

使用PropertyEditorRegistrar或许是最好的方法在这个例子中。首先，你需要创建你自己的PropertyEditorRegistrar实现：

```
package com.foo.editors.spring;
public final class CustomPropertyEditorRegistrar implements PropertyEditorRegistrar {
    public void registerCustomEditors(PropertyEditorRegistry registry) {
        // it is expected that new PropertyEditor instances are created
        registry.registerCustomEditor(ExoticType.class, new ExoticTypeEditor());
        // you could register as many custom property editors as are required here...
    }
}
```

也可参考org.springframework.beans.support.ResourceEditorRegistrar作为一个PropertyEditorRegistrar实现的例子。注意实现中的registerCustomEditors方法创建每个属性编辑器的新实例

下面我们配置一个CustomEditorConfigurer并且注入一个我们自己的CustomPropertyEditorRegistrar实例：

```
<bean class="org.springframework.beans.factory.config.CustomEditorConfigurer">
<property name="propertyEditorRegistrars">
    <list>
        <ref bean="customPropertyEditorRegistrar"/>
    </list>
    </property>
</bean>
<bean id="customPropertyEditorRegistrar"
class="com.foo.editors.spring.CustomPropertyEditorRegistrar"/>
```

最后，暂时脱离一下本节，在你使用spring的mvc的web框架时，一起使用PropertyEditorRegistrars和数据绑定控制器会更加方便。下面的例子中使用PropertyEditorRegistrar并实现了initBinder方法：

```
public final class RegisterUserController extends SimpleFormController {
    private final PropertyEditorRegistrar customPropertyEditorRegistrar;
    
    public RegisterUserController(PropertyEditorRegistrar propertyEditorRegistrar) {
        this.customPropertyEditorRegistrar = propertyEditorRegistrar;
    }
    
    protected void initBinder(HttpServletRequest request, ServletRequestDataBinder binder) throws Exception {
        this.customPropertyEditorRegistrar.registerCustomEditors(binder);
    }
    
    // other methods to do with registering a User
}
```

这种使用PropertyEditor注册的风格可以简化代码（initBinder的实现只是一部分），允许通用的PropertyEditor注册代码在一个类中然后根据需要在许多控制器间共享。



