#### Internationalization using MessageSource

ApplicationContext接口继承了MessageSource接口，提供了国际化功能。spring也提供了HierarchicalMessageSource接口，用于处理消息层级。这些接口是spring处理消息的基础。定义在接口中的方法有：

* String getMessage(String code, Object[] args, String default, Locale loc):基本方法用于接收MessageSource的消息。在特定locale中没有消息时，将使用默认的消息。任何传入的参数替换值，使用标准库中的MessageFormate功能。
* String getMessage(String code, Object[] args, Locale loc):和以一个方面相似，一个区别是，不需要定义默认的消息，如果没有找到消息会抛出NoSuchMessageException异常。
* String getMessage(MessageSourceResolvable resolvable, Locale locale):所有前面的方法中使用的属性可以定义在一个类中，名字为MessageSourceResolvable，这时你可以使用这个方法。

当ApplicationContext被加载，将自动寻找定义在上下文中的MessageSource。必须有名为messageSource的bean。如果找到这样的bean，所有前面方法的调用都会委托给消息源。如果没有消息源，ApplicationContext将会常是寻找父容器中包含的同名bean。如果找到则作为MessageSource。如果ApplicationContext没有找到任何消息源，一个空的DelegatingMessageSource将会被初始化用于接受上面定义的方法调用。

spring提供了两个MessageSource实现，ResourceBundleMessageSource和StaticMessageSource。每一个都实现了HierarchicalMessageSource用于嵌入消息。StaticMessageSource很少被使用但是提供了编程的方式用于向源中添加消息。ResourceBundleMessageSource在下面的例子中展示：

```
<beans>
    <bean id="messageSource"
    class="org.springframework.context.support.ResourceBundleMessageSource">
        <property name="basenames">
            <list>
                <value>format</value>
                <value>exceptions</value>
                <value>windows</value>
            </list>
        </property>
    </bean>
</beans>
```

这个例子中假设你有三个资源绑定在你的classpath中叫format、exceptions和windows。每个消息的请求都会以JDK标准的方式处理通过ResourceBundles。这个例子的目的，假设上面两个资源绑定的文件是

```
# in format.properties
message=Alligators rock!
```

```
# in exceptions.properties
argument.required=The {0} argument is required.
```

执行MessageSource功能的程序在下一个例子中。记住所有ApplicationContext的实现也是MessageSource的实现，并且可以转化为MessageSource接口。

public static void main(String[] args) {
    MessageSource resources = new ClassPathXmlApplicationContext("beans.xml");
    String message = resources.getMessage("message", null, "Default", null);
    System.out.println(message);
}

上面程序的输出结果是

```
Alligators rock!
```

简而言之，MessageSource定义在名为beans.xml的文件中，该文件在你classpath的根路径中。messageSource的bean通过他的属性引用了一系列的资源。这三个文件在列表中存在于你的classpath的根目录中名字分别为format.properties、exceptions.properties和windows.properties。

下面的例子展示了参数用于消息查找，三个参数将会转化为字符串然后替换寻找的消息。

```
<beans>
    <!-- this MessageSource is being used in a web application -->
    <bean id="messageSource" class="org.springframework.context.support.ResourceBundleMessageSource">
        <property name="basename" value="exceptions"/>
    </bean>
    
    <!-- lets inject the above MessageSource into this POJO -->
    <bean id="example" class="com.foo.Example">
        <property name="messages" ref="messageSource"/>
    </bean>
</beans>
```

```
public class Example {
    private MessageSource messages;
    
    public void setMessages(MessageSource messages) {
        this.messages = messages;
    }
    
    public void execute() {
        String message = this.messages.getMessage("argument.required",
        new Object [] {"userDao"}, "Required", null);
        System.out.println(message);
    }
}
```

调用execute方法的输出将会是

```
The userDao argument is required.
```

由于国际化，spring的不同MessageSource实现遵循相同的本地处理方式并且回调规则如标准JDK的ResourceBundle。简而言之，和前面的messageSource例子通讯，如果你希望消息可以以en-GB处理，你可以创建名字为format_en_GB.properties、exceptions_en_GB.properties和windows_en_GB.properties文件。

通常，本地的处理方法有应用所处的环境决定。在这个例子中，如British的消息需要手动的特殊处理。

```
# in exceptions_en_GB.properties
argument.required=Ebagum lad, the {0} argument is required, I say, required.
```

```
public static void main(final String[] args) {
    MessageSource resources = new ClassPathXmlApplicationContext("beans.xml");
    String message = resources.getMessage("argument.required",
    new Object [] {"userDao"}, "Required", Locale.UK);
    System.out.println(message);
}
```

上面程序的输出将会是

```
Ebagum lad, the 'userDao' argument is required, I say, required.
```

你可以使用MessageSourceAware接口来获得任意一个被定义的MessageSource。任意一个实现了MessageSourceAware接口的定义在ApplicationContext的bean当bean被创建和配置时都注入到MessageSource的应用上下文中。

>**Note**

>作为ResourceBundleMessageSource的替代，spring提供了ReloadableResourceBundleMessageSource类。支持相同的bundle文件格式但是更加的方便比标准JDK的ResourceBundleMessageSource实现。尤其是他支持从任意spring资源位置读取文件（不只是classpath）并且支持热重载资源文件（当有效的缓存之间）。详情见ReloadableResourceBundleMessageSource的javadocs。


