##### Lookup mehtod injection

Lookup method injection 是容器提供的一种可以重写被容器管理的 bean 的方法的的能力，返回容器中另一个命名的bean。lookup通常用在原型的bean上如上面描述的场景中。spring框架实现这种方法注入通过使用由cglib库生成的二进制代码，来动态生成子类覆盖了这个方法。

>**Note**

>为了这样的子类可以工作，spring的bean容器中包含的类不能是final的，并且方法也不可以是final的。

>单元测试一个有抽象方法的类需要你提供一个子类并实现抽象的方法。
>组件扫描下的具体类也需要具体方法。

>一个更进一步的限制对于lookup方法不会正常工作在工厂方法上，特别是在配置class时的@Bean方法，因为容器在这种情况下不会创建实例也不能在创建运行时子类。

看前面代码片段中CommandManager的类，你看见spring容器动态的实现覆盖了createCommand方法。你的CommandManager类可以不需要任何spring的依赖，你可以看下面改进的例子：

```
package fiona.apple;

// no more Spring imports!
public abstract class CommandManager {

    public Object process(Object commandState) {
        // grab a new instance of the appropriate Command interface
        Command command = createCommand();
        // set the state on the (hopefully brand new) Command instance
        command.setState(commandState);
        return command.execute();
    }

    // okay... but where is the implementation of this method?
    protected abstract Command createCommand();
}
```

在客户端的class包含需要被注入的方法（在这个例子中是CommandManager），需要被注入的方法需要满足下面这样的形式:

```
<public|protected> [abstract] <return-type> theMethodName(no-arguments);
```

如果方法是抽象的，可以动态生成子类来实现这个方法。另外，动态生成的子类覆盖了原有类中的创建方法。例如：

```
<!-- a stateful bean deployed as a prototype (non-singleton) -->
<bean id="myCommand" class="fiona.apple.AsyncCommand" scope="prototype">
    <!-- inject dependencies here as required -->
</bean>

<!-- commandProcessor uses statefulCommandHelper -->
<bean id="commandManager" class="fiona.apple.CommandManager">
    <lookup-method name="createCommand" bean="myCommand"/>
</bean>
```

commandManager的bean定义调用了自己的createCommand方法当他需要myCommand的bean时。你必须将myCommand的bean开发成原型模式，如果需要的话。如果是单例模式的话，那么每次返回的都将是同一个实例。

另外，如果是基于注解的配置模式，你可以在lookup方法上定义@Lookup注解：

```
public abstract class CommandManager {

    public Object process(Object commandState) {
        Command command = createCommand();
        command.setState(commandState);
        return command.execute();
    }
    
    @Lookup("myCommand")
    protected abstract Command createCommand();
}
```

或者更常见的是，你也可以根据lookup方法的返回类型来查找匹配的bean：

```
public abstract class CommandManager {

    public Object process(Object commandState) {
        MyCommand command = createCommand();
        command.setState(commandState);
        return command.execute();
    }

    @Lookup
    protected abstract MyCommand createCommand();
}
```

注意你可以实现lookup方法通过创建一个子类实现，以配合spring的组件扫描默认会忽略抽象类。这种限制不适用于明确注册bean或明确导入bean。

>Note
>另一种可以访问不同生命周期的方法是ObjectFactory/ Provider注入。见章节“Scoped beans as dependencies”
>感兴趣的读者也可以查找ServiceLocatorFactoryBean（在org.springframework.beans.factory.config包中）的用法。