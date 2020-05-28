#### Formatter SPI

Formatter的SPI用于实现域格式化逻辑是简单而且强类型的：

```
package org.springframework.format;
public interface Formatter<T> extends Printer<T>, Parser<T> {
}
```

Formatter继承了Printer和Parser内置的接口：

```
public interface Printer<T> {
String print(T fieldValue, Locale locale);
}
```

```
import java.text.ParseException;
public interface Parser<T> {
    T parse(String clientValue, Locale locale) throws ParseException;
}
```

为了创建你自己的Formatter，需要实现上面的Formatter接口。参数T类型是你需要格式化的类型，例如，java.util.Data。实现print方法用于打印T的实例在客户端本地。实现parse方法用于将T实例从客户端本地格式化。你的Formatter应该抛出ParseException或IllegalArgumentException如果格式化尝试失败的时候。注意保证你的Formatter是线程安全的。

一些Formatter实现提供在format子包中作为方便使用。该Number子包中提供了NumberFormatter、CurrencyFormatter和PercentFormatter用于格式化java.lang.Number通过使用java.text.NumberFormat。datetime子包中提供了DateFormatter用于格式化java.util.Data通过使用java.text.DataFormat。datetime.joda子包中提供了复杂的日期转化用于支持Joda的时间包。

```
package org.springframework.format.datetime;

public final class DateFormatter implements Formatter<Date> {
    private String pattern;
    public DateFormatter(String pattern) {
        this.pattern = pattern;
    }
    
    public String print(Date date, Locale locale) {
        if (date == null) {
            return "";
        }
        return getDateFormat(locale).format(date);
    }
    
    public Date parse(String formatted, Locale locale) throws ParseException {
        if (formatted.length() == 0) {
            return null;
        }
        return getDateFormat(locale).parse(formatted);
    }
    
    protected DateFormat getDateFormat(Locale locale) {
        DateFormat dateFormat = new SimpleDateFormat(this.pattern, locale);
        dateFormat.setLenient(false);
        return dateFormat;
    }
}
```

spring小组欢迎社区的格式化贡献：见jira.spring.io用于贡献。


