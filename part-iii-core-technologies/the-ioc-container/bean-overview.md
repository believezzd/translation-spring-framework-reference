### 7.3 Bean overview

Spring的IOC容器管理一个或多个bean。这些bean由你提供给容器的配置元数据来创建，例如，在xml中bean的定义。

在容器中，这些bean的定义被作为BeanDefinition的对象，包括以下的元数据（包括其他的信息）：

- 一个有包限制的类名：通常是bean的实际实现类。
- bean行为配置元素，显示bean在容器中的行为（范围、生命周期、回调等等）。
- 指向其他bean的引用，为了使得bean可以工作，这些引用被叫做协作或依赖。
- 其他会在初始化bean时设置的配置项，例如，用在bean上的连接数用于管理连接池或池的大小限制等。

元数据转化为一系列的属性用于完成bean的定义。

*Table 7.1. The bean definition*

Property|Explained in
--|--
class|Section7.3.2,“Instantiating beans”
name|Section7.3.1,“Naming beans”
scope|Section7.5,“Bean scopes”
constructor arguments|Section7.4.1,“Dependency Injection”
properties|Section7.4.1,“Dependency Injection”
autowiring mode|Section7.4.5,“Autowiring collaborators”
lazy-initialization mode|Section7.4.4,“Lazy-initialized beans”
initialization method|the section called“Initialization callbacks”
destruction method|the section called “Destruction callbacks”

除此之外，bean的信息中包括创建一个特殊的bean，ApplicationContext的实现也允许将用户在容器外已经创建的对象注册为bean。他的实现是通过getBeanFactory方法返回的BeanFactory的实现DefaultListableBeanFactory来访问ApplicationContext的BeanFactory。DefaultListableBeanFactory支持通过registerSingleton和registerBeanDefinition来注册bean。然而，通常应用都应该使用元数据来定义bean。

>**Note**

>Bean的元数据和手动添加的单例实例需要尽早注册，为了使容器正确推断它们在自动装配和其他内省的步骤。覆盖已经存在的元数据和已经存在的单例实例在一定程度上是支持的，在运行时注册新的bean（并发的访问工厂）在官方上是不支持的，或许会导致并发访问的异常或不一致的状态在bean的容器中。