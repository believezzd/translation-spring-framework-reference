#### Converter SPI

SPI用于类型转换的逻辑时简单的而且是强类型的：

```
package org.springframework.core.convert.converter;
public interface Converter<S, T> {
    T convert(S source);
}
```

创建你自己的转化器需要实现上面的接口。参数S是你需要转换的类型，并且T是转换后的类型。这个转化器也可以应用在集合或数组上用于从S转换为T，提供了一个数组和集合转化器也被注册（默认是DefaultConversionService）。

对于每次调用转化，源参数不可以是null。如果转换失败该方法可以抛出任何非检查异常：特殊的，一个IllegalArgumentException应该被抛出来反映一个错误的原数据值。保证你的转换实现是线程安全的。

一些转换器的实现在core.convert.support包中被提供作为方便。包括将字符串转化为数字和其他常用的类型转换。考虑StringToInteger作为一个列子描述典型的转换器实现：

```
package org.springframework.core.convert.support;

final class StringToInteger implements Converter<String, Integer> {
    public Integer convert(String source) {
        return Integer.valueOf(source);
    }
}
```

