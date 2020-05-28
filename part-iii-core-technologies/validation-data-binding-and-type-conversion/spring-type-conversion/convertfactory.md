#### ConverterFactory

当你需要集中转换逻辑用于这个类的层级，例如，当将字符串转换为java.lang.Enum的object时，可以实现ConverterFactory:

```
package org.springframework.core.convert.converter;
public interface ConverterFactory<S, R> {
    <T extends R> Converter<S, T> getConverter(Class<T> targetType);
}
```

参数S是你需要转换的类型，R是你需要转换的类型范围。getConverter实现中T是R的一个子类。

考虑StringToEnum的ConverterFactory作为一个例子：

```
package org.springframework.core.convert.support;
final class StringToEnumConverterFactory implements ConverterFactory<String, Enum> {
    public <T extends Enum> Converter<String, T> getConverter(Class<T> targetType) {
        return new StringToEnumConverter(targetType);
    }
    
    private final class StringToEnumConverter<T extends Enum> implements Converter<String, T> {
        private Class<T> enumType;
        public StringToEnumConverter(Class<T> enumType) {
            this.enumType = enumType;
        }
        public T convert(String source) {
            return (T) Enum.valueOf(this.enumType, source.trim());
        }
    }
}
```