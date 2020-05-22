#### The prototype scope

非单例，原型模式的bean在每次请求时会创建新的实例。这意味着bean注入到另一个bean或在容器中调用getBean方法。一般情况下，原型范围用于有状态的bean，单例范围用于无状态的bean。

下面的图表展示了spring的原型范围。DAO并不是一个典型的原型模式，因为通常DAO中不需要任何状态变量，这只是为了重用单例的那张图而已。

![Prototype Scope](/graph/Figure 7.5.2 Prototype Scope.png)

下面的例子是原型范围在xml中的定义方式:

```
<bean id="accountService" class="com.foo.DefaultAccountService" scope="prototype"/>
```

和其他的范围进行对比，spring并没有完全管理一个原型bean的整个生命周期：实例化、配置另外包括组合这些object然后提供给客户端，但是不保留这些原型实例的引用。因此，尽管初始化方法在需要这个bean时被回调不论scope，但对于原型bean，destroy方法不会被回调。因此客户端必须手动释放原型范围的object并且释放他们持有的宝贵的资源。为了可以让spring释放原型范围bean的资源，尝试使用自定义的bean的post-procesor，使得需要被清理时可以保留一个bean的引用。

在某种意义上，对于一个原型的bean来说，spring容器的角色就是Java new操作的替代。所有的生命周期管理过去完全是由客户端执行的（详见spring容器对bean的生命周期管理，见章节7.6.1“Lifecycle callbacks”）