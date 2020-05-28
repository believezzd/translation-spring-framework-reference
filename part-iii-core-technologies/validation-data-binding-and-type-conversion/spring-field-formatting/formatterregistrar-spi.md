#### FormatterRegistrar SPI

FormatterRegistrar作为一个SPI用于注册格式化器和转换器通过FormatterRegistry：

```
package org.springframework.format;
public interface FormatterRegistrar {
    void registerFormatters(FormatterRegistry registry);
}
```

FormatterRegistrar是有用的当注册多个相关的转换器和格式化器用于给定的格式化种类，例如日期格式化。在直接注册不能实现时可以很有用。例如当一个格式化去需要被索引在特定的域类型下不同于T类型或当注册Printer/Parser对时使用。下一节将展示更多的有关转换器和格式化器注册的内容。

见22.16.3节，“转换和格式化”在spring的mvc章节。