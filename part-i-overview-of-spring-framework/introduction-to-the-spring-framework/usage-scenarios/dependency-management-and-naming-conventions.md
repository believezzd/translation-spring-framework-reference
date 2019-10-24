#### Dependency Management and Naming Conventions

依赖管理和依赖注入是不同的。为了使用这些优秀的特性（比如依赖注入），你需要将所有相关的jar放入你的classpath中，并且在编译和运行时使用这些jar文件。这些依赖并不是虚拟的，而是在文件系统中实际的资源。依赖管理包括确认这些资源的位置、存储这些资源并且将他们加入classpath。依赖可以直接的（比如你的应用在运行时依赖spring），或者是间接的（比如你的应用依赖commons-dbcp，而commons-dbcp依赖于commons-pool）。间接依赖也就是我们所知道的传递，并且这样的依赖是很难定义和管理的。

如果你使用spring或许你会将你需要的jar复制一份。为了使操作简单，spring在打包时就尽可能的将依赖分开，例如，如果你不需要web应用的话你就不需要spring-web模块。在这份文档中为了方便引用，我们使用了名字的简略，比如spring-*或spring-*.jar，用*来代表模块的简写。（例如spring-core、spring-webmvc、spring-jms等）。而你实际用到的jar文件可能还会含有版本号（例如spring-core-4.3.25.RELEASE.jar）。

每一份发布的spring框架都会发布在如下的地方。

- Maven中心仓库，是Maven查询的默认仓库，并且使用时不需要任何特殊的配置。spring依赖的很多通用的库文件在Maven中心仓库中也能找到并且对spring来说会有很多的选择，如果使用Maven作为依赖管理，这样比较方便。这些在Maven仓库中的jar的名字都有这样的形式spring-*-版本号.jar并且Maven的groupId是org.springframework.
- Spring使用的公开的Maven仓库。为了方便找到GA版本，仓库也包括了spring的开发版本和milestones版本。这些jar在Maven参考中的命名也相似，因此Maven仓库也是获得开发版本的spring及其他相关有用的库文件的地方。这个仓库中也包含一系列的zip文件包括spring的所有jar以方便下载。

因此首先你要决定如何管理你的依赖，我们强烈推荐使用自动化管理系统例如Maven, Gradle or Ivy，但是你也可以下载和手动管理。

你可以看到下面的spring的产品。如果需要查看详细的模块描述，请看[2.2 Framework Modules](/part-i-overview-of-spring-framework/introduction-to-the-spring-framework/framework-modules.md)。

*Table 2.1. Spring Framework Artifacts*

Group|ArtifactId|Description
--|:--:|--
org.springframework|spring-aop|Proxy-based AOP support
org.springframework|spring-aspects|AspectJ based aspects
org.springframework|spring-beans|Beans support, including Groovy
org.springframework|spring-context|Application context runtime, including scheduling and remoting abstractions
org.springframework|spring-context-support|Support classes for integrating common third-party libraries into a Spring application context
org.springframework|spring-core|Core utilities, used by many other Spring modules
org.springframework|spring-expression|Spring Expression Language (SpEL)
org.springframework|spring-instrument|Instrumentation agent for JVM bootstrapping
org.springframework|spring-instrument-tomcat|Instrumentation agent for Tomcat
org.springframework|spring-jdbc|JDBC support package, including DataSource setup and JDBC access support 
org.springframework|spring-jms|JMS support package, including helper classes to send/receive JMS messages
org.springframework|spring-messaging|Support for messaging architectures and protocols
org.springframework|spring-orm|Object/Relational Mapping, including JPA and Hibernate support
org.springframework|spring-oxm|Object/XML Mapping
org.springframework|spring-test|Support for unit testing and integration testing Spring components
org.springframework|spring-tx|Transaction infrastructure, including DAO support and JCA integration
org.springframework|spring-web|Foundational web support, including web client and webbased remoting
org.springframework|spring-webmvc|HTTP-based Model-View-Controller and REST endpoints for Servlet stacks
org.springframework|spring-webmvc-portlet|MVC implementation to be used in a Portlet environment
org.springframework|spring-websocket|WebSocket and SockJS infrastructure, including STOMP messaging support
