##### Shutting down the Spring IoC container gracefully in non-web applications

>**Note**

> 这一章节只适用于非web的应用。基于spring的web应用的ApplicationContext实现在相关web应用停止时会优雅的关闭spring的ioc容器。

如果你在非web应用环境中使用spring的ioc容器，例如，在一个富客户端环境，你注册一个JVM钩子函数保证优雅的关闭并调用所有相关单例对象的销毁方法以便所有的资源都被释放了。当然，你必须首先正确的配置这些销毁的回调方法。

