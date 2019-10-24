ApplicationContext是一个接口用于保持高级的工厂可以提供注册不同bean和他们的依赖的能力。使用方法T getBean(String name, Class<T> requiredType)你可以取回你bean的实例对象。

ApplicationContext允许你通过下面的方式读取bean的定义和访问他们：

```
// create and configure beans
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");

// retrieve configured instance
PetStoreService service = context.getBean("petStore", PetStoreService.class);

// use configured instance
List<String> userList = service.getUsernameList();
```

使用Groovy配置，引导看起来非常类似，只是一个不同的上下文实现类groovy-aware(但也理解XML bean定义):

```
ApplicationContext context = new GenericGroovyApplicationContext("services.groovy", "daos.groovy");
```

最灵活的变体是结合了reader委托的GenericApplicationContext。使用XmlBeanDefinitionReader处理XML文件:

```
GenericApplicationContext context = new GenericApplicationContext();
new XmlBeanDefinitionReader(context).loadBeanDefinitions("services.xml", "daos.xml");
context.refresh();
```

或者结合解析Groove类型文件的GroovyBeanDefinitionReader：

```
GenericApplicationContext context = new GenericApplicationContext();
new GroovyBeanDefinitionReader(context).loadBeanDefinitions("services.groovy", "daos.groovy");
context.refresh();
```

如果需要，同一个ApplicationContext可以混合使用读取器代理来读取不同的配置源。

可以使用getBean方法来获取beans的实例。ApplicationContext有不同的方法来获取beans，但是理想情况下，应用程序代码不应该使用。实际上，应用程序代码不应该调用getBean方法，也不要依赖于Spring的接口。例如，Spring整合web框架提供的依赖注入功能，允许你在特定bean上声明依赖(例如autowired注解)
















