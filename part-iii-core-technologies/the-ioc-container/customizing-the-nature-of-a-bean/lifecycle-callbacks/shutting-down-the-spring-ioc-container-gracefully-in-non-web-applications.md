##### Shutting down the Spring IoC container gracefully in non-web applications

>**Note**

> 这一章节只适用于非web的应用。基于spring的web应用的ApplicationContext实现在相关web应用停止时会优雅的关闭spring的ioc容器。

如果你在非web应用环境中使用spring的ioc容器，例如，在一个富客户端环境，你注册一个JVM钩子函数保证优雅的关闭并调用所有相关单例对象的销毁方法以便所有的资源都被释放了。当然，你必须首先正确的配置这些销毁的回调方法。

注册一个关闭钩子，你需要调用registerShutdownHook方法定义在ConfigurableApplicationContext的接口中。

```
import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public final class Boot {
    public static void main(final String[] args) throws Exception {
        ConfigurableApplicationContext ctx = new ClassPathXmlApplicationContext("beans.xml");
        // add a shutdown hook for the above context...
        
        ctx.registerShutdownHook();
        // app runs here...
        // main method exits, hook is called prior to the app shutting down...
    }
}
```