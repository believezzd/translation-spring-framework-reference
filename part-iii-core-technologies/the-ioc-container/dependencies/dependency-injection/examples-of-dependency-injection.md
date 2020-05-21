##### Examples of dependency injection

下面的例子使用了基于xml的配置元数据用于set方法的注入。下面是一部分spring的xml配置文件声明了一些bean的定义。

```
<bean id="exampleBean" class="examples.ExampleBean">
<!-- setter injection using the nested ref element -->
    <property name="beanOne">
        <ref bean="anotherExampleBean"/>
    </property>
<!-- setter injection using the neater ref attribute -->
    <property name="beanTwo" ref="yetAnotherBean"/>
    <property name="integerProperty" value="1"/>
</bean>

<bean id="anotherExampleBean" class="examples.AnotherBean"/>
<bean id="yetAnotherBean" class="examples.YetAnotherBean"/>
```

```
public class ExampleBean {
    private AnotherBean beanOne;
    private YetAnotherBean beanTwo;
    private int i;
    
    public void setBeanOne(AnotherBean beanOne) {
        this.beanOne = beanOne;
    }
    
    public void setBeanTwo(YetAnotherBean beanTwo) {
        this.beanTwo = beanTwo;
    }
    
    public void setIntegerProperty(int i) {
        this.i = i;
    }
}
```

在前面的例子中，定义在xml文件中的属性和set方法相匹配。下面的例子使用了构造器的依赖注入：

```
<bean id="exampleBean" class="examples.ExampleBean">
    <!-- constructor injection using the nested ref element -->
    <constructor-arg>
        <ref bean="anotherExampleBean"/>
    </constructor-arg>
    <!-- constructor injection using the neater ref attribute -->
    <constructor-arg ref="yetAnotherBean"/>
    <constructor-arg type="int" value="1"/>
</bean>
<bean id="anotherExampleBean" class="examples.AnotherBean"/>
<bean id="yetAnotherBean" class="examples.YetAnotherBean"/>
```

```
public class ExampleBean {
    private AnotherBean beanOne;
    private YetAnotherBean beanTwo;
    private int i;
    
    public ExampleBean(AnotherBean anotherBean, YetAnotherBean yetAnotherBean, int i) {
        this.beanOne = anotherBean;
        this.beanTwo = yetAnotherBean;
        this.i = i;
    }
}
```

在bean定义中定义的构造器参数将会作为ExampleBean构造器的参数使用。

现在考虑这样一个例子，不使用构造器，使用静态工厂方法来返回一个object的实例来调用spring。

```
<bean id="exampleBean" class="examples.ExampleBean" factory-method="createInstance">
    <constructor-arg ref="anotherExampleBean"/>
    <constructor-arg ref="yetAnotherBean"/>
    <constructor-arg value="1"/>
</bean>
<bean id="anotherExampleBean" class="examples.AnotherBean"/>
<bean id="yetAnotherBean" class="examples.YetAnotherBean"/>
```

```
public class ExampleBean {
    // a private constructor
    private ExampleBean(...) {
    ...
    }
    
    // a static factory method; the arguments to this method can be
    // considered the dependencies of the bean that is returned,
    // regardless of how those arguments are actually used.
    public static ExampleBean createInstance (
            AnotherBean anotherBean, YetAnotherBean yetAnotherBean, int i) {
        ExampleBean eb = new ExampleBean (...);
        // some other operations...
        return eb;
    }
}
```

静态工厂方法的参数由<constructor-arg/>元素提供，和使用构造器是差不多的。使用工厂方法返回的类型并不一样要和包含静态方法的类相同，上面的例子是个特例。实例工厂方法（非静态）的使用方法和这个例子差不多（区别只是在class属性中使用了factory-bean），因此我们不在这里在讨论他的细节。