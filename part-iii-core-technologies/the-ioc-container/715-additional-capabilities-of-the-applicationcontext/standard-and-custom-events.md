#### Standard and custom events

ApplicationEvent类和ApplicationListener接口用于处理ApplicationContext的事件。如果一个bean实现了ApplicationListener接口并部署到上下文中，每次ApplicationEvent发布到ApplicationContext时，这个bean会被通知到。本质上，这是标准的观察者设计模式。

>**Tip**

>在spring4.2中，事件的基础已经被改进并提供了基于注解的模式也可以用于发布事件，这样可以不需要继承ApplicationEvent。这样的object发布后我们会将其包裹为事件。

spring提供了以下的标准事件

*Table 7.7. Built-in Events*

Event | Explanation
-- | --
ContextRefreshedEvent | Published when the ApplicationContext is initialized or refreshed, for example, using the refresh() method on the ConfigurableApplicationContext interface. "Initialized" here means that all beans are loaded, post-processor beans are detected and activated, singletons are preinstantiated, and the ApplicationContext object is ready for use. As long as the context has not been closed, a refresh can be triggered multiple times, provided that the chosen ApplicationContext actually supports such "hot" refreshes. For example, XmlWebApplicationContext supports hot refreshes, but GenericApplicationContext does not.
ContextStartedEvent | Published when the ApplicationContext is started, using the start() method on the ConfigurableApplicationContext interface. "Started" here means that all Lifecycle beans receive an explicit start signal. Typically this signal is used to restart beans after an explicit stop, but it may also be used to start components that have not been configured for autostart , for example, components that have not already started on initialization.
ContextStoppedEvent | Published when the ApplicationContext is stopped, using the stop() method on the ConfigurableApplicationContext interface. "Stopped" here means that all Lifecycle beans receive an explicit stop signal. A stopped context may be restarted through a start() call.
ContextClosedEvent | Published when the ApplicationContext is closed, using the close() method on the ConfigurableApplicationContext interface. "Closed" here means that all singleton beans are destroyed. A closed context reaches its end of life; it cannot be refreshed or restarted.
RequestHandledEvent | A web-specific event telling all beans that an HTTP request has been serviced. This event is published after the request is complete. This event is only applicable to web applications using Spring’s DispatcherServlet.

你可以创建和发布你自己的事件。这个例子描述了一个简单的类继承了spring的ApplicationEvent基类：

```
public class BlackListEvent extends ApplicationEvent {
    private final String address;
    private final String content;
    
    public BlackListEvent(Object source, String address, String content) {
        super(source);
        this.address = address;
        this.content = content;
    }
    // accessor and other methods...
}
```

发布一个自定义的ApplicationEvent，调用ApplicationEventPublisher中的publishEvent方法。通常这个操作是通过创建一个类实现ApplicationEventPublisherAware并且将其注册为spring的bean。下面的例子展示了这样一个类：

```
public class EmailService implements ApplicationEventPublisherAware {
    private List<String> blackList;
    private ApplicationEventPublisher publisher;
    public void setBlackList(List<String> blackList) {
        this.blackList = blackList;
    }
    public void setApplicationEventPublisher(ApplicationEventPublisher publisher) {
        this.publisher = publisher;
    }
    public void sendEmail(String address, String content) {
        if (blackList.contains(address)) {
            publisher.publishEvent(new BlackListEvent(this, address, content));
            return;
        }
        // send email...
    }
}
```

在配置的时候，spring容器将会探测实现ApplicationEventPublisherAware的EmailService并将自动调用setApplicationEventPublisher方法。实际上，传入的参数是容器本身，你只是通过ApplicationEventPublisher接口和应用上下文交互而已。

处理自定义的ApplicationEvent，需要创建一个类实现的ApplicationListener然后注册为一个spring的bean。下面的例子展示了一个这样的类：

```
public class BlackListNotifier implements ApplicationListener<BlackListEvent> {
    private String notificationAddress;
    public void setNotificationAddress(String notificationAddress) {
        this.notificationAddress = notificationAddress;
    }
    public void onApplicationEvent(BlackListEvent event) {
        // notify appropriate parties via notificationAddress...
    }
}
```

注意ApplicationListener通常参数化由于你自定义的事件，BlackListEvent。这意味着onApplicationEvent方法可以是类型安全，避免任何向下的类型转换。你可以根据需要注册多个事件监听器，但是注意默认的时间监听器同步的接收事件。这意味着publishEvent方法将会被阻塞直到所有监听器完成事件的处理。这种同步的有点和单个线程方法是当一个监听器接收一个事件，他在发布者事务上下文中进行操作如果事务上下文是存在的。如果另一个事件发布的策略不在必要，参考spring的ApplicationEventMulticaster接口的javadocs文档。

下面的例子展示了bean定义用于注册和配置上面的类：

<bean id="emailService" class="example.EmailService">
    <property name="blackList">
        <list>
            <value>known.spammer@example.org</value>
            <value>known.hacker@example.org</value>
            <value>john.doe@example.org</value>
        </list>
    </property>
    </bean>
    <bean id="blackListNotifier" class="example.BlackListNotifier">
        <property name="notificationAddress" value="blacklist@example.org"/>
    </bean>
</bean>

将他们放在一起，当emailService的bean中的sendEmail方法被调用时，如果是黑名单中的email地址，一个自定义事件BlackListEvent将会被发布。blackListNotifier的bean作为一个ApplicationListener被注册并接收BlackListEvent事件当他在适当的时候被通知。

>**Note**

>spring的事件策略的设计是为了在spring的bean和相同的应用上下文之间进行简单的通信。然而对于比较复杂的企业集成需要，分开管理spring的集成项目提供了全面的支持用于建立轻量级、基于模式、事件驱动的架构，并建立在大家熟悉的spring编程模型上。

