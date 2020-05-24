#### PropertySource abstraction

spring的环境抽象可以提供查找选型通过配置属性源码的层级。为了更好的说明，考虑下面的例子：

```
ApplicationContext ctx = new GenericApplicationContext();
Environment env = ctx.getEnvironment();
boolean containsFoo = env.containsProperty("foo");
System.out.println("Does my environment contain the 'foo' property? " + containsFoo);
```

在上面的片段中，我们看到了一种高级的方法用于从spring查询foo属性是否定义在当前的环境中。为了回答这个问题，环境提供了从属性中查找的功能。一个PropertySource就是一个简单的抽象对任何key-value对，和spring的StandardEnvironment是由两个PropertySource的object配置的，一个是JVM的系统属性（System.getPropertieses），另一个是系统环境变量（System.getenv）。

>**Note**

>认的PropertySource代表标准环境，用于独立的应用。StandardServletEnvironment由额外的默认属性源码填充包括servlet的配置和servlet的上下文参数。StandardProtleEnvironment类似的访问portlet配置和portlet上下文参数作为属性源码。每一选项都支持JndiPropertySource。详见javadocs。

具体说明，当使用StandardEnvironment时，调用env.containsProperty("foo")将会返回true如果foo系统变量或foo环境变量在运行时可见。

>**Tip**

>查找是层级的。默认的，系统变量会覆盖环境变量，因此如果foo属性在两种set中都存在，则会采用系统变量的值，覆盖环境变量的值。注意，属性不会合并但是可以覆盖。

>对于一个通常的StandardServletEnvironment，下面是全部的层级，按优先级排列从上到下：

>* ServletConfig参数（如果可用，例如DispatcherServlet上下文）
>* ServletContext参数（web.xml中的context-param属性）
>* JNDI环境变量（"java:comp/env/"实体）
>* JVM系统参数("-D"命令行参数)
>* JVM系统环境（操作系统环境变量）

最重要的，这个策略是可以配置的。或许你有一个自定义的属性代码你希望可以被搜索。没有问题，简单实现和实例化你自己的PropertySource并且加入PropertySource的集合为当前的环境：

在上面的代码中，MyPropertySource被加入了搜索的最高优先级。如果他包含foo属性，他将首先被探测并返回覆盖其他PropertySource中的值。MutablePropertySources暴露了一系列方法允许精确的控制属性源码。

