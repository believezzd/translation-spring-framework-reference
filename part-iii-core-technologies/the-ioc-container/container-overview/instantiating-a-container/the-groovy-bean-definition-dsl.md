作为外部化配置元数据的进一步示例，bean定义也可以用Spring的Groovy Bean定义DSL，如Grails框架所知。一般来说，这样的配置会在".groovy"文件中，结构如下：

```
beans {
    dataSource(BasicDataSource) {
        driverClassName = "org.hsqldb.jdbcDriver"
        url = "jdbc:hsqldb:mem:grailsDB"
        username = "sa"
        password = ""
        settings = [mynew:"setting"]
    }
    sessionFactory(SessionFactory) {
        dataSource = dataSource
    }
    myService(MyService) {
        nestedBean = { AnotherBean bean ->
            dataSource = dataSource
        }
    }
}
```

这种配置风格在很大程度上等价于XML bean定义，甚至支持SpringXML配置名称空间。它还允许导入XML bean配置文件通过“importBeans”指令。
