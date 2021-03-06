### 9.4 Bean manipulation and the BeanWrapper

org.springframework.beans包实现了oracle提供的javabean标准绑定。一个javabean就是一个简单的类，有一个默认的无参构造器，按照一定的参数命名规范，如有个属性名字为bingoMadness，则有set方法setBingoMadness和get方法getBingoMadness。更多有关javabean和定义的信息请参考oracle的官网。

在bean包中很重要的类是BeanWrapper接口和他的相关实现（BeanWrapperImpl）。引用在javadocs，BeanWrapper提供了设置和获取属性值的功能（单个或批量）、获得属性描述、查询属性来确定是否是可读或可写的。BeanWrapper也提供支持对内置的属性、允许无限制深度的设置属性的子属性。BeanWrapper支持添加JavaBeans PropertyChangeListeners和VetoableChangeListeners，不需要目标类的代码支持。BeanWrapper支持索引属性的设置。BeanWrapper通常不会再应用代码中直接使用，但是会在DataBinder和BeanFactory中使用。

BeanWrapper工作方式和名字暗示的一部分：他处理bean的行为，类似于设置和获得属性。