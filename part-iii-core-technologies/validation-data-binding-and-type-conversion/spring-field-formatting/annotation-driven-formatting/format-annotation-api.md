##### Format Annotation API

方便的格式化注解API在org.springframework.format.annotation包中提供。使用@NumberFormat用于格式化java.lang.Number域。使用@DateTimeFormat用于格式化java.util.Date、java.util.Calendar、java.util.Long或Joda时间域，

下面的例子展示了使用@DateTimeFormate来将java.util.Date格式化为ISO日期（yyyy-MM-dd）：

```
public class MyModel {
    @DateTimeFormat(iso=ISO.DATE)
    private Date date;
}
```


