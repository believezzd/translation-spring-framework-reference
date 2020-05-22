##### Using a custom scope

当你完成并测试一个或多个自定义范围的实现，你需要使得spring容器意识到你定义的范围。下面的方法就是核心方法用于将一个新的范围注册到spring容器中：

```
void registerScope(String scopeName, Scope scope);
```

这个方法定义在ConfigurableBeanFactory接口中，对于多个ApplicationContext实现可见通过BeanFactory属性

registerScope方法的第一个参数是唯一的名字和范围绑定，例如这样的名在spirng容器中是单例和原型。registerScope方法的第二个参数是你需要注册和使用的自定义范围实现的实例

假设你完成你的自定义范围实现，然后就这样进行注册。

>**Note**

>下面的例子使用了spring包含的SimpleThreadScope，但是并没有被注册。这个组件和你自定义的范围实现基本类似。

```
Scope threadScope = new SimpleThreadScope();
beanFactory.registerScope("thread", threadScope);
```

你可以在创建bean定义是使用你自定义的范围：

```
<bean id="..." class="..." scope="thread">
```

自定义的 Scope 实现，不仅仅可以使用编程的方式进行注册。你可以使用声明式的 Scope 注册，使用 CustomScopeconfigurer 类：

```
<ss>sdf

```

我的
