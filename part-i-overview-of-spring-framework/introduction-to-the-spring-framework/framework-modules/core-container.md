#### Core Container

核心容器包含spring-core、spring-beans、spring-context、spring-context-support和spring-expression组件。

spring-core和spring-beans模块提供了框架最基本的功能，包括IOC和依赖注入的特性。BeanFactory是一个优秀的工厂模式的具体实现。他使得你不用去编程实现单例模式，并且允许你将配置和依赖管理从你的程序逻辑中分离。

spring-context模块构建在core和beans模块上，他是用框架style的访问object的方式，类似于JNDI的注册。cotext模块继承了beans的优秀特性并添加了对国际化的支持（例如，使用resource bundles）、事件传播、资源加载和方便创建上下文，例如一个servlet容器。context模块也支持Java EE的特性，例如EJB、JMX和基本的远程支持。ApplicationContext接口是context模块的核心点。context-support模块支持一些第三方组件在spring的集成，比如缓存组件(EhCache, Guava, JCache)、邮件组件(JavaMail)、任务计划组件(CommonJ, Quartz)和模板引擎(FreeMarker, JasperReports, Velocity)。

spring-expression模块提供了一种强大的表达式语言用于查询和操作object graph在运行时。他是标准表达式语言的扩展，是JSP 2.1规范的详细实现。该语言支持设置和获得属性值、property assignment、方法声明、访问内容数组、容器和indexers、logical and arithmetic operators、named variables、通过名称在IOC容器中需找bean。他也支持像普通的list aggregations一样list视图。

