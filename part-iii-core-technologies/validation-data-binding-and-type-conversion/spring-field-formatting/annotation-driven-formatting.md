#### Annotation-driven Formatting

你看到域格式化可以通过域类型或注解来配置。为了将注解绑定到formatter上需要实现AnnotationFormatterFactory：

```
package org.springframework.format;

public interface AnnotationFormatterFactory<A extends Annotation> {
    Set<Class<?>> getFieldTypes();
    Printer<?> getPrinter(A annotation, Class<?> fieldType);
    Parser<?> getParser(A annotation, Class<?> fieldType);
}
```

参数A用于域注解类型你希望可以和格式化逻辑相关联，例如org.springframework.format.annotation.DateTimeFormat。getFieldTypes方法可以返回使用的注解域的类型。getPrinter可以返回一个Printer用于打印注解域的值。getParser方法可以返回一个Parser用于格式化用于注解域的clientValue。

下面实现AnnotationFormatterFactory的类将NumberFormatter注解和格式化器绑定。注解允许number类型或特定的格式：

```
public final class NumberFormatAnnotationFormatterFactory
implements AnnotationFormatterFactory<NumberFormat> {
    public Set<Class<?>> getFieldTypes() {
        return new HashSet<Class<?>>(asList(new Class<?>[] {
            Short.class, Integer.class, Long.class, Float.class,
            Double.class, BigDecimal.class, BigInteger.class }));
    }
    
    public Printer<Number> getPrinter(NumberFormat annotation, Class<?> fieldType) {
        return configureFormatterFrom(annotation, fieldType);
    }
    
    public Parser<Number> getParser(NumberFormat annotation, Class<?> fieldType) {
        return configureFormatterFrom(annotation, fieldType);
    }
    
    private Formatter<Number> configureFormatterFrom(NumberFormat annotation, Class<?> fieldType) {
        if (!annotation.pattern().isEmpty()) {
            return new NumberStyleFormatter(annotation.pattern());
        } else {
            Style style = annotation.style();
            if (style == Style.PERCENT) {
                return new PercentStyleFormatter();
            } else if (style == Style.CURRENCY) {
                return new CurrencyStyleFormatter();
            } else {
                return new NumberStyleFormatter();
            }
        }
    }
}
```

为了触发格式化，简单声明@NumberFormat注解在域中：

```
public class MyModel {
    @NumberFormat(style=Style.CURRENCY)
    private BigDecimal decimal;
}
```