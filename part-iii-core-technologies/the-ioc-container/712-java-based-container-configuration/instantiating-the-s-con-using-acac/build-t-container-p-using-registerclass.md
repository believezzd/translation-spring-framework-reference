##### Building the container programmatically using register(Class<?>…)

AnnotationConfigApplicationContext可以使用无参的构造器，然后施一公register方法来进行配置。当使用编程来构造AnnotationConfigApplicationContext时这种方法很有用。

```
public static void main(String[] args) {
    AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext();
    ctx.register(AppConfig.class, OtherConfig.class);
    ctx.register(AdditionalConfig.class);
    ctx.refresh();
    MyService myService = ctx.getBean(MyService.class);
    myService.doStuff();
}
```