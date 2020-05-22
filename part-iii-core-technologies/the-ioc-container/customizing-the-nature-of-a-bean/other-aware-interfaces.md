#### Other Aware interfaces

除了上面讨论的ApplicationContextAware和BeanNameAware，spring提供了一系列的Aware接口运行bean来暗示容器他们需要一些特殊的组件依赖。最重要的Aware接口在下面被总结，作为通用的规则，名字暗示了注入的类型

*Table 7.4 Aware interfaces*

Name | Injected Depend | Explained in...
-- | :--: |--
ApplicationContextAware | Declaring ApplicationContext | the section called “ApplicationContextAware and BeanNameAware”
ApplicationEventPublisherAware | event publisher of the enclosing ApplicationContext | Section 7.15, “Additional capabilities of the ApplicationContext”
BeanClassLoaderAware | Class loader used to load the bean classes. | the section called “Instantiating beans”
BeanFactoryAware | Declaring BeanFactory | the section called “ApplicationContextAware and BeanNameAware”
BeanNameAware | Name of the declaring | the section called “ApplicationContextAware and BeanNameAware”
BootstrapContextAware | Resource adapter BootstrapContext the container runs in. Typically available only in JCA aware ApplicationContexts | Chapter 32, JCA CCI
LoadTimeWeaverAware | Defined weaver for processing class definition at load time | the section called “Load-time weaving with AspectJ in the Spring Framework”
MessageSourceAware | Configured strategy for resolving messages (with support for parametrization and internationalization) | Section 7.15, “Additional capabilities of the ApplicationContext”
NotificationPublisherAware | Spring JMX notification publisher | Section 31.7, “Notifications” 
PortletConfigAware | Current PortletConfig the container runs in. Valid only in a web-aware Spring ApplicationContext | Chapter 25, Portlet MVC Framework
PortletContextAware | Current PortletContext the container runs in. Valid only in a web-aware Spring ApplicationContext | Chapter 25, Portlet MVC Framework
ResourceLoaderAware | Configured loader for low-level access to resources | Chapter 8, Resources
ServletConfigAware | Current ServletConfig the container runs in. Valid only in a web-aware Spring ApplicationContext | Chapter 22, Web MVC framework
ServletContextAware | Current ServletContext thecontainer runs in. Valid only in a web-aware Spring ApplicationContext | Chapter 22, Web MVC framework

再次注意，使用这些接口会使得你的代码和Spring API耦合，并且不符合反向控制的方式。建议基础组件需要编程访问容器时使用。




