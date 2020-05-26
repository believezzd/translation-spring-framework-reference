##### Constructing ClassPathXmlApplicationContext instances - shortcuts

ClassPathXmlApplicationContext暴露了一部分构造器允许方便的初始化。基本的方法是提供一个字符串数组包含xml文件的文件名（不需要指定路径信息），也支持一个类；ClassPathXmlApplicationContext将会从类中获取路径信息。

一个例子可以很好的说明这件事。考虑一个如下的路径分布：

```
com/
    foo/
        services.xml
        daos.xml
        MessengerService.class
```

ClassPathXmlApplicationContext实例由定义在services.xml和daos.xml中的bean定义组成可以像如下一样初始化？

```
ApplicationContext ctx = new ClassPathXmlApplicationContext(
new String[] {"services.xml", "daos.xml"}, MessengerService.class);
```

详见ClassPathXmlApplicationContext的javadocs来查看不同的构造器信息。