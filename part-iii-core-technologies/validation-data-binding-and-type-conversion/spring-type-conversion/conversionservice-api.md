#### ConversionService API

ConversionService定义了一个统一的API用于在运行时执行类型转换逻辑。Converters经常执行在此接口的后面：

```
package org.springframework.core.convert;
public interface ConversionService {
    boolean canConvert(Class<?> sourceType, Class<?> targetType);
    <T> T convert(Object source, Class<T> targetType);
    boolean canConvert(TypeDescriptor sourceType, TypeDescriptor targetType);
    Object convert(Object source, TypeDescriptor sourceType, TypeDescriptor targetType);
}
```

大部分ConversionService实现也实现了ConverterRegistry，用于提供SPI来注册转换器。在其内部，ConversionService实现委托了注册机制给他的注册转换器用于处理类型转换逻辑。

一个强健的ConversionService实现在core.convert.support包中被提供。GenericConversionService是一个通用的实现可以用于大部分的情况。ConversionServiceFactory提供了一个方便的工厂用于创建普通的ConversionService配置。