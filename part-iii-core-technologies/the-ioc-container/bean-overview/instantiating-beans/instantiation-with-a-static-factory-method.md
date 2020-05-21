##### Instantiating with static factory method

当你使用静态工厂方法创建一个bean时，你使用class属性定义包含静态工厂方法的类和一个属性叫factory-method来定义工厂方法自身的名字。你应该可以调用这个方法（使用后续描述的可选参数）并且返回一个live的object，这个object和用构造器创建的没有什么区别。这样的用法通常是为了调用静态工厂在legacy code中。

下面的bean定义了这个bean将会被工厂方法来创建。这个定义没有定义返回object的类型，只有这个类包含工厂方法。在这个例子中，createInstance方法必须是静态方法。

```
<bean id="clientService" class="examples.ClientService" factory-method="createInstance"/>
```

```
public class ClientService {
    private static ClientService clientService = new ClientService();
    
    private ClientService() {}
    
    public static ClientService createInstance() {
        return clientService;
    }
}
```

如果想了解向工厂方法提供参数的机理或在构造之后设置对象实例属性，详见Dependencies and configuration in detail。