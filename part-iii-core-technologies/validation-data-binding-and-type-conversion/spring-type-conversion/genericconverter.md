#### GenericConverter

当你需要一个复杂的转换器实现，考虑一下GenericConverter接口。更加方便但是没有强类型标识，支持多个源码到目标类型的转换。此外，GenericConverter使得当你实现你的转换逻辑时使用源码和目标域。这样的上下文允许一个类型转换通过域注解或定义在域中通用的信息。

```
package org.springframework.core.convert.converter;
public interface GenericConverter {
    public Set<ConvertiblePair> getConvertibleTypes();
    
    Object convert(Object source, TypeDescriptor sourceType, TypeDescriptor targetType);
}
```

实现GenericConverter，getConvertibleTypes需要返回支持源类型到目标类型的对。然后实现convert方法来实现你的转换逻辑。源TypeDescriptor提供访问被转换的元域的值。目标TypeDescriptor提供访问转换后的目标域的值，该值可以被设置。

一个比较好的GenericConverter例子就是Java数组到集合的转换。这样的ArrayToCollectionConverter自省定义在目标集合类型的域用于处理集合元素类型。允许每个在源数组的元素被转换为集合元素类型在结合被设置到目标域之前。

>**Note**

>因为GenericConverter是一个比较复杂的SPI接口，当你需要的时候才会使用。倾向Converter或ConverterFactory用于基本类型转换根据需要。

