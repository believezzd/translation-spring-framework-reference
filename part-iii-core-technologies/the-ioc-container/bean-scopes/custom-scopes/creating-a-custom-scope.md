如果需要将你的自定义范围和spring容器结合，你需要实现org.springframework.beans.factory.config.Scope接口，将会在这个章节中进行介绍。关于如何实现你自己的范围，请参考spring框架自身或scope的javadocs文件，会更加详细的介绍你需要实现的方法。

scope接口有4个方法，来获得范围中的对象、删除范围中的对象和允许他们被销毁。

下面的方法从一个潜在的范围返回一个对象。session范围的实现，例如，返回session范围的bean（如果不存在，该方法返回bean的一个新实例，然后与session绑定用于后面的引用）。

```
Object get(String name, ObjectFactory objectFactory)
```

下面的方法从一个潜在的范围删除一个对象。session范围的实现，例如，删除session范围的bean。然后被删除的bean会被返回，如果没有找到指定名的对象会返回null。

```
Object remove(String name)
```

下面的方法注册回调方法当范围被销毁或者定义在范围的bean被销毁时执行。销毁回调详见spring框架的javadoc或spring范围的实现。

```
void registerDestructionCallback(String name, Runnable destructionCallback)
```

下面的方法为潜在的范围获得会话标识。这个标识与其他范围是不一样的。例如session范围的实现，这个定义可以session的标识符。

```
String getConversationId()
```
