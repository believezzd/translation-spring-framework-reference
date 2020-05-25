##### Annotation-based event listeners

从spring4.2开始，事件监听器可以注册在任意公共方法通过EventListener注解在一个被管理的bean中。BlackListNotifier可以重写成如下的样子：

```
public class BlackListNotifier {
    private String notificationAddress;
    public void setNotificationAddress(String notificationAddress) {
        this.notificationAddress = notificationAddress;
    }
    
    @EventListener
    public void processBlackListEvent(BlackListEvent event) {
        // notify appropriate parties via notificationAddress...
    }
}
```

在上面所见，方法的声明中指定了可以监听什么样的事件。这也使用于嵌套泛型只要实际的事件过滤了相应的泛型参数。

如果你的方法希望监听多个事件或你不希望方法有参数，事件也可以通过注解本身来本身。

```
@EventListener({ContextStartedEvent.class, ContextRefreshedEvent.class})
public void handleContextStart() {
    ...
}
```

也可以通过注解的condition属性添加额外的运行时过滤，注解通过定义SpEL表达式来匹配实际的特定事件。

例如，我们的notifier可以重写用于只有当事件的test属性为foo时被调用：

```
@EventListener(condition = "#blEvent.content == 'foo'")
public void processBlackListEvent(BlackListEvent blEvent) {
    // notify appropriate parties via notificationAddress...
}
```

每个SpEL表达式处理一个相应的上下文。下面的表格列出了项目可以适用于上下文因此可以用于条件事件的处理：

*Table 7.8. Event SpEL available metadata*

Name | Location | Description | Explanation
-- | -- | -- | --
Event | root object | The actual
ApplicationEvent | #root.event
Arguments array | root object | The arguments (asarray) use d for invoking the target| #root.args[0]
Argument name | evaluation context | Name of any of the method arguments. If for some reason the names are not available (e.g. no debug information), the argument names are also available under the #a<#arg> where #arg stands for the argument index (starting from 0). | #blEvent or #a0 (one can also use #p0 or #p<#arg> notation as an alias).

注意，#root.event允许你访问潜在的事件，即使你的方法签名实际指向了一个已经被发布的object。

如果你需要发布一个事件作为另一个事件的处理结果，只要改变方法的签名返回应该被发布的事件，例如：

```
@EventListener
public ListUpdateEvent handleBlackListEvent(BlackListEvent event) {
    // notify appropriate parties via notificationAddress and
    // then publish a ListUpdateEvent...
}
```

>**Note**
>这个特性不支持异步的监听器

这个新的方法会发布一个新的ListUpdateEvent用于每个BlackListEvent被上面的方法处理之后。如果你需要发表多个事件，则返回事件的集合作为代替。

