#### Customizing instantiation logic with FactoryBean

实现org.springframework.beans.factory.FactoryBean接口的对象自己成了工厂。

FactoryBean接口是spring的ioc容器的初始化逻辑中一个可插拔的部分。如果你用java实现了更好的的初始化逻辑代码，来代替冗长的xml配置，你可以创建你自己的FactoryBean，在类中实现复杂的初始化逻辑，然后将你的FactoryBean通过插件加载到容器中。

FactoryBean接口提供了三个方法：

* Object getObject(): 返回公共创建的object的实例。这个实例可以被共享，取决于工厂返回的是一个单例还是原型。

* boolean isSingleton(): 返回true如果这个FactoryBean实例是单例，不是返回false。

* Class getObjectType(): 返回getObject方法返回的实例的类型，如果类型是未知的则返回空。

FactoryBean概念和接口在spring框架的很多地方被使用。在spring中就有多于50个的实现。

当你需要从容器中获得一个实际的FactoryBean实例而不是bean产生的，在bean的id前使用&符号，然后调用ApplicationContext的genBean方法就可以。例如一个给定的bean的名字为myBean，调用容器的getBean("myBean")来返回创建的他的FactoryBean，调用getBean("&myBean")可以获得FactoryBean实例本身。