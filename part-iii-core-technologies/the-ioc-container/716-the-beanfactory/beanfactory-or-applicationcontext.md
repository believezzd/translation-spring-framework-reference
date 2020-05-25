#### BeanFactory or ApplicationContext

使用ApplicationContext，除非你有很好的理由不这样做。使用 GenericApplicationContext 及其子类 AnnotationConfigApplicationContext 作为自定义引导的常用实现。这些是Spring的主要入口点用于所有常见目的的核心容器:加载配置文件，触发类路径扫描，以编程方式注册bean定义和带注释的

因为ApplicationContext包括所有BeanFactory的功能，通常建议使用BeanFactory除了一些情况例如在嵌入式应用运行在资源匮乏的设备上，其中内存的消耗是很重要的，而且几个kilobytes就会导致不同的特殊情况。然而对于大部分典型的企业应用和系统中，ApplicationContext是可以使用的。spring使用了BeanPostProcessor扩展点（用于代理或其他的）。如果你只是使用普通的BeanFactory，一些事务和AOP将不会有效至少一些额外的操作是必要的。这种情况是困扰的因为没有实际的错误在配置中。

对于许多扩展的容器特性，如注释处理和AOP代理，BeanPostProcessor扩展点是必不可少的。如果你只使用普通字体DefaultListableBeanFactory，这样的后处理器在默认情况下不会被检测和激活。这种情况可能令人困惑，因为您的bean配置实际上没有任何错误;它是相反，在这样的场景中，容器需要通过额外的设置完全引导。

下面的表展示的BeanFactory和ApplicationContext接口和实现的特性：

*Table 7.9. Feature Matrix*

Feature | BeanFactory | ApplicationContext
-- | -- | -- 
Bean instantiation/wiring | Yes | Yes
Integrated lifecycle management | No | Yes
Automatic BeanPostProcessor registration | No | Yes
Automatic BeanFactoryPostProcessor registration | No | Yes
Convenient MessageSource access (for internalization) | No | Yes
Built-in ApplicationEvent publication mechanism | No | Yes

明确注册一个 bean 的 post-processor 的实现通过 DefaultListableBeanFactory，你需要通过调用像 addBeanPostProcessor: 

```
DefaultListableBeanFactory factory = new DefaultListableBeanFactory();

// populate the factory with bean definitions
// now register any needed BeanPostProcessor instances
factory.addBeanPostProcessor(new AutowiredAnnotationBeanPostProcessor());
factory.addBeanPostProcessor(new MyBeanPostProcessor());

// now start using the factory
```

应用 BeanFactoryPostProcessor 到一个普通的 DefaultListableBeanFactory 上，你需要调用它的 postProcessBeanFactory 方法：

```
DefaultListableBeanFactory factory = new DefaultListableBeanFactory();
XmlBeanDefinitionReader reader = new XmlBeanDefinitionReader(factory);
reader.loadBeanDefinitions(new FileSystemResource("beans.xml"));

// bring in some property values from a Properties file
PropertyPlaceholderConfigurer cfg = new PropertyPlaceholderConfigurer();
cfg.setLocation(new FileSystemResource("jdbc.properties"));

// now actually do the replacement
cfg.postProcessBeanFactory(factory);
```

在两个例子中，明确的注册步骤是不方便的，也是一个原因为什么不同的ApplicationContext实现比普通的BeanFactory实现要好在不同spring-backed应用，尤其是使用BeanFactoryPostProcessors和BeanPostProcessors。这些策略实现是重要的例如属性替换和AOP。

>**Note**

> 一个注释configapplicationcontext拥有所有通用的注释后处理器注册的开箱即用，并可能带来额外的处理器下面的覆盖通过配置注释，如@EnableTransactionManagement。在抽象层次上在Spring的基于注解的配置模型中，bean后处理器的概念变成了一个纯粹的内部容器细节。


