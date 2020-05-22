#### Other Aware interfaces

除了上面讨论的ApplicationContextAware和BeanNameAware，spring提供了一系列的Aware接口运行bean来暗示容器他们需要一些特殊的组件依赖。最重要的Aware接口在下面被总结，作为通用的规则，名字暗示了注入的类型

*Table 7.4 Aware interfaces*

Name | Injected Depend | Explained in...
-- | :--: |--
ApplicationContextAware | Declaring ApplicationContext | the section called “ApplicationContextAware and BeanNameAware”
ApplicationEventPublisherAEwvaenrte | b | c
BeanClassLoaderAware | b | c
BeanFactoryAware | b | c
BeanNameAware | b | c
BootstrapContextAware | b | c
LoadTimeWeaverAware | b | c
MessageSourceAware | b | c
NotificationPublisherAwareSpring | Spring JMX notification publisher | Section 31.7, “Notifications” PortletConfigAware | Current PortletConfig the container runs in. Valid only in a web-aware Spring ApplicationContext | Chapter 25, Portlet MVC Framework
PortletContextAware | Current PortletContext the container runs in. Valid only in a web-aware Spring
ApplicationContext | Chapter 25, Portlet MVC Framework
ResourceLoaderAware | Configured loader for low-level access to resources | Chapter 8, Resources
ServletConfigAware | Current ServletConfig the container runs in. Valid only in a web-aware Spring ApplicationContext | Chapter 22, Web MVC framework
ServletContextAware | Current ServletContext thecontainer runs in. Valid only in a web-aware Spring ApplicationContext | Chapter 22, Web MVC framework






