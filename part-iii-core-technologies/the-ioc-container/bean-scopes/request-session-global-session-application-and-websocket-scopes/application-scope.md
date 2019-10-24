考虑下面用xml配置的bean的定义：

```
<bean id="appPreferences" class="com.foo.AppPreferences" scope="application"/>
```

spring容器通过使用AppPreferences的bean定义创建了一次AppPreferences的实例为整个Web应用。这也就说明appPreferences的bean是ServletContext级别的，保存在常规的ServletContext属性中。这种在某些方面和spring的单例bean相同，但是有2个重要区别是：在每个ServletContext中单例不是每个spring的ApplicationContext（在web应用中可能有多个applicationContext），并且他实际作为ServletContext的属性被暴露与可见。