##### ConditionalGenericConverter

有时你希望转换器在特定条件下进行转换。例如，你或许希望执行转换如果一个特定注解在目标域上使用时。或许你只是希望执行转换如果一个特定的方法例如静态的valueOf方法被定义在目标类中。ConditionalGenericConverter就是一个GenericConverter和ConditionalConverter的组合接口允许你定义自定义的匹配逻辑条件：

```
public interface ConditionalConverter {
    boolean matches(TypeDescriptor sourceType, TypeDescriptor targetType);
}
public interface ConditionalGenericConverter extends GenericConverter, ConditionalConverter {
}
``` 

ConditionalGenericConverter的一个好例子是EntityConverter用于转换一个持久化实体定义为一个实体引用。例如EntityConverter可以只匹配目标实体类型定义一个静态的finder方法例如findAccount(Long)。你或许可以使finder方法检测定义在matches(TypeDescriptor, TypeDescriptor)的实现中。

