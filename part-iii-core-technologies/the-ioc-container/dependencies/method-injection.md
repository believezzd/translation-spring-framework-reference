#### Method injection

在大部分应用的场景中，容器中的大部分bean都是单例的。当一个单例的bean需要和其他的单例bean协作或一个非单例bean需要和另一个非单例bean协作，你通常通过定义另一个bean的属性来处理依赖。当bean的生命周期不同时会产生问题。假设单例bean A需要使用非单例（原型）bean B，或许在在A中的每个方法里调用。容器只会创建一次A，并且只有一次设置属性的机会。容器不可能在每次需要B时提供一个bean的实例。

一个解决方案时放弃使用反向控制。你可以使bean A通过实现ApplicationContextAware接口来感知容器，并且在每次需要的使用通过一个getBean("B")方法来向容器所要一个bean B的实例。下面的例子介绍了这种使用方法

```
// a class that uses a stateful Command-style class to perform some processing
// 一个使用了有状态的命令模式的类来体现一些过程

package fiona.apple;

// Spring-API imports
// spring api的导入
import org.springframework.beans.BeansException;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ApplicationContextAware;

public class CommandManager implements ApplicationContextAware {
 
    private ApplicationContext applicationContext;

    public Object process(Map commandState) {
        // grab a new instance of the appropriate Command
// 抓取一个合适的Command实例
        Command command = createCommand();
        // set the state on the (hopefully brand new) Command instance
// 设置state为新的Command实例
        command.setState(commandState);
        return command.execute();
    }
    protected Command createCommand() {
        // notice the Spring API dependency!
// 通知spring api的依赖
        return this.applicationContext.getBean("command", Command.class);
    } 

    public void setApplicationContext(
            ApplicationContext applicationContext) throws BeansException {
        this.applicationContext = applicationContext;
    }
}
```

前面是例子是不可取的，因为业务代码和spring框架相互融合了。方法注入，是spring IOC容器的高级特性，允许这样的用例来实现干净的逻辑。

>你可以在博客中了解更多有关方法注入的机制。

