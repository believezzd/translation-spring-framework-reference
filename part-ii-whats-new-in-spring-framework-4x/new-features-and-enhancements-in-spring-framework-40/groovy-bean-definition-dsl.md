### Groovy Bean Definition DSL

如果通过Groovy DSL来开始使用spring4.0，需要定义一个外部的bean配置。这和使用XML的bean定义类似，允许你使用更加简洁的语法。使用Groovy也允许你更加容易的在bootstrap代码中嵌入bean定义，例如：

```
def reader = new GroovyBeanDefinitionReader(myApplicationContext)
reader.beans {
    dataSource(BasicDataSource) {
        driverClassNam = "org.hsqldb.jdbcDriver"
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

详细的信息请查看GroovyBeanDefinitionReader的文档。