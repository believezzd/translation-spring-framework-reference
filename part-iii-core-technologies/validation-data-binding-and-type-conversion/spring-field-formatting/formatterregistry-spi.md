#### FormatterRegistry SPI

FormatterRegistry是一个SPI用于注册格式化器和转换器。FormattingConversionService是FormatterRegistry的实现用于大部分环境。该实现可以通过编程配置或定义为一个spring的bean通过使用FormattingConversionServiceFactoryBean。因为这个实现也实现了ConversionService，可以直接配置用于spring的DataBinder和spring的表达式语言。

回忆下面的FormatterRegistry的SPI：

```
package org.springframework.format;
public interface FormatterRegistry extends ConverterRegistry {
    void addFormatterForFieldType(Class<?> fieldType, Printer<?> printer, Parser<?> parser);
    void addFormatterForFieldType(Class<?> fieldType, Formatter<?> formatter);
    void addFormatterForFieldType(Formatter<?> formatter);
    void addFormatterForAnnotation(AnnotationFormatterFactory<?, ?> factory);
}
```

就像上面展示的，可以通过域类型或注解来注册格式化器。

FormatterRegistry的SPI允许你配置格式化规则，避免跨控制器的重复配置。例如，你可以强制所有的日期域格式化为一个特定的形式或有特定注解的域格式化为特定的形式。通过共享FormatterRegistry，你可以一次定义这些规则并将它们应用于你需要的位置。