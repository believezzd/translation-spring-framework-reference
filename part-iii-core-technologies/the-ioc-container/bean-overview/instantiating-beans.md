#### Instantiating beans

一个bean的定义用于实例化一个或多个object。容器根据bean的要求，使用配置元数据封装bean的定义创建（或获得）一个实际的object。

如果你使用基于xml的配置元数据，你在bean标签定义的class属性将会是object实例化的类型（或class）。这个class属性中必须强制定义一个class用于一个bean定义的实体。（例如，见“Instantiation using an instance factory method”and Section 7.7, “Bean definition inheritance”）。你会在以下二选一的方式中使用 Class 属性：

* 通常，定义bean的类用于构造以方便容器自身通过反射调用它的构造器创建实例，有一点与通过java代码使用new操作符类似。

* 声明包含静态工厂方法的实际类用于创建object，在很少的情况下容器会调用类的静态工厂方法用于创建bean。调用静态工厂方法的返回值可能是相同的类或完全是另外一个类。

>内部类的命名

>如果你想为静态内部类配置bean的定义，你必须使用内部类的binary name。

>例如，如果在com.example包下有个类叫Foo，并且Foo有个内部类叫Bar，那么class属性的值应该是？

>```com.example.Foo$Bar```

>注意，在name中使用$符号用于区分内部类和外部类。