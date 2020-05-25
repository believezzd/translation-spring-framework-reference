#### Convinient ApplicationContext instantiation for web application

你可以创建ApplicationContext实例通过，例如，一个ContextLoader。当然你也可以通过使用其中一个ApplicationContext实现来编程创建ApplicationContext实例。

你可以而使用ContextLoaderListener注册ApplicationContext如下：

<context-param>
    <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/daoContext.xml /WEB-INF/applicationContext.xml</param-value>
    </context-param>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>

监听器检查contextConfigLocation参数。如果参数不存在，这个监听器将默认使用/WEB-INF/applicationContext.xml。当参数存在的时候，监听器使用之前定义的分隔符（逗号、分行或空格）并使用其值作为路径来寻找应用上下文。任何类型的路径模式都是支持的。例如/WEB-INF/*Context.xml表示所有以Context.xml结尾的文件在WEB-INF目录中，并且/WEB-INF/**/*Context.xml用于子目录的WEB-INF文件夹。