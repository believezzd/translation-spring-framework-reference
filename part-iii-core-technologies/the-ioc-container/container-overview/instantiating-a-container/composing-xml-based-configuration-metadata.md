将定义分开到多个xml文件是有用的。每一个独立的xml配置文件代表你应用架构中的一个逻辑或模块。

你可以通过构造器从所有的xml片段中读取bean的定义。构造器支持多个资源的路径就像前面的那个例子。或者你也可以使用<import/>标签来从其他文件中引入作为代替。例如：

```
<beans>
    <import resource="services.xml"/>
    <import resource="resources/messageSource.xml"/>
    <import resource="/resources/themeSource.xml"/>
    <bean id="bean1" class="..."/>
    <bean id="bean2" class="..."/>
</beans>
```

在前面的例子中，外部的bean定义加载自三个文件：services.xml、messageSource.xml和themeSource.xml中。所有的路径依赖于定义的引入的文件。因此因此service.xml文件必须和引入的文件在同一个目录或classpath中。当messageSource.xml和themeSource.xml的资源位置必须是在引入文件的下面。你也看到了，看是的斜线被忽略了，但是这三个路径是相对，最好不要是用那个斜线。被引入的文件的内容，包括顶层的<beans>元素，必须是一个符合spring schema的文件定义。

>Note
>可以但是不建议，使用类似于"../"的路径引用父级文件夹的文件。这样做会导致一个依赖的文件在应用程序的外部。尤其是，这样的引用包含在"classpath:"的URL中也是不推荐的（例如："classpath:../services.xml"），当运行时资源进程会先选择最近的classpath，然后在寻找上一级的路径。classpath配置的改变将会导致选择的不同，从而产生错误的路径。
>你可以使用全限定资源的路径来代替相对路径，例如："file:C:/config/services.xml"或"classpath:/config/services.xml"。然而，你也要意识到你的应用配置和特定的路径产生了耦合。通常的做法与这些路径之间使用间接的联系，例如，通过"${…?}"占位符然后通过jvm的系统属性在运行时确定。

导入指令是bean命名空间本身提供的特性。
除了普通的bean定义之外，Spring提供的XML名称空间中还有其他可用的定义特性进行进一步配置，例如
“context”和“util”命名空间。

