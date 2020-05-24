##### Simple construction

使用spring的xml文件作为输入来初始化ClassPathXmlApplicationContext，和使用@Configuration类作为输入来初始化AnnotationConfigApplicationContext是相似的。这样可以完全不依赖于spring的xml文件：

```
public static void main(String[] args) {
    ApplicationContext ctx = new AnnotationConfigApplicationContext(AppConfig.class);
    MyService myService = ctx.getBean(MyService.class);
    myService.doStuff();
}
```

就像上面提到的，AnnotationConfigApplicationContext 并不局限于只处理@Configuration类。任何@Component修饰的类或JSR330注解类都可以作为输入来进行构造。例如：

```
public static void main(String[] args) {
    ApplicationContext ctx = new AnnotationConfigApplicationContext(MyServiceImpl.class,
    Dependency1.class, Dependency2.class);
    MyService myService = ctx.getBean(MyService.class);
    myService.doStuff();
}
```

上面的例子假设MyServiceImpl、Dependency1和Dependency2使用了类似于@Autowired的注解来进行依赖注入。
