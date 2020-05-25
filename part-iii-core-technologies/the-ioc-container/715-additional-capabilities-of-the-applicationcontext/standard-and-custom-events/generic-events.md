##### Generic events

你也会使用泛型来定义你事件的结构。考虑EntityCreatedEvent<T>当T作为一个实际的被创建的实体。你可以创建如下的监听器定义用于接收EntityCreatedEvent用于一个Person：

```
@EventListener
public void onPersonCreated(EntityCreatedEvent<Person> event) {
    ...
}
```

由于类型的消除，这样将导致这个监听器只会当泛型匹配时被执行如事件监听器过滤某个类PersonCreatedEvent继承了EntityCreatedEvent<Person>。

在确切的情况下，如果所有的事件遵循相同的结构将会非常复杂（因为他应该是上面的事件）。在这种情况下，你可以实现ResolvableTypeProvider用于指导框架当运行时环境被提供：

```
public class EntityCreatedEvent<T> extends ApplicationEvent implements ResolvableTypeProvider {
    public EntityCreatedEvent(T entity) {
        super(entity);
    }
    
    @Override
    public ResolvableType getResolvableType() {
        return ResolvableType.forClassWithGenerics(getClass(),
        ResolvableType.forInstance(getSource()));
    }
}
```

>**Tip**
>这不仅对ApplicationEvent有效也包括任何你作为事件发送的object。