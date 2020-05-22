##### Initail web configuration

为了支持request、session、全局session、application和WebSocket范围，一些初始化的配置需要用来定义你的bean。（这些初始化的配置不需要基本的范围支持，如单例和原型）

如何完成这些初始化配置取决于你具体的Servlet环境。

如果你使用spring的web mvc来使用具有范围的bean，实际上，对于由spring的DispatcherServlet和DispatcherPortlet来处理的一个请求，那么不需要什么特殊的配置。DispatcherServlet和DispatcherPortlet已经暴露了所有相关的状态。

如果你使用servlet 2.5版本的web容器，请求是在spring的DispatcherServlet外部处理的（例如，你使用JSF或Struts），你需要注册org.springframework.web.context.request.RequestContextListener ServletRequestListener。对于Servlet3.0以上的版本，这个操作可以通过WebApplicationInitializer接口来实现或者兼容老的容器，在你web应用的web.xml文件中添加如下的配置：

```
<web-app>
    ...
    <listener>
        <listener-class>        
            org.springframework.web.context.request.RequestContextListener
        </listener-class>
    </listener>
    ...
</web-app>
```

相对的，如果你的监听器初始化有问题，考虑使用spring的RequestContextFilter。过滤器的匹配根据web应用来配置，因此你可以需要适当更改。

```
<web-app>
    ...
    <filter>
        <filter-name>requestContextFilter</filter-name>
        <filter-class>org.springframework.web.filter.RequestContextFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>requestContextFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    ...
</web-app>
```

DispatcherServlet、RequestContextListener和RequestContextFilter基本都是在做相同的事情，绑定http请求对象到处理请求线程上。这使得request和session范围的bean可以来到调用链上。