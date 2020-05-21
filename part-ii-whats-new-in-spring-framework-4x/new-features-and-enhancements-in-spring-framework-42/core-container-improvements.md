### 5.1 Core Container Improvements

* 像 @bean 的注解在 Java8 的默认方法上也会被检测并解析，允许接口的默认方法上使用 @bean 注解使接口组成一个配置类

* 配置类可以声明 @import 来引入一个普通的组件类，允许混合引入配置类和组件类。

* 配置类可以使用 @order 使处理具有顺序性（例如通过名字重写的 beans）即使是通过路径扫描检测到的。

* @Resource 注解支持 @Lazy 的声明，与 @Autowired 类似，会接受一个延迟加载的代理来获取目标 bean

* 应用的事件框架现在提供注解模型，而且可以用来发布任意事件。
    * 任何在 IoC 容器中被管理的bean 的 public 方法在都可以被 @EventListener 修饰来进行事件消费。
    * @TransactionEventListener 支持绑定事务的事件消费。
  
* Spring 4.2 提供了对注解别名的声明和查找的一流支持。新的@AliasFor注解可以用于通过一个注解声明一对别名的属性，或者一个别名从自定义组合的注解到一个元注解。
    * 以下的注解已经更新了对 @AliasFor 的支持，使他们为自身的属性值提供有意义的别名: @Cacheable, @CacheEvict, @CachePut, @ComponentScan, @ComponentScan.Filter, @ImportResource, @Scope, @ManagedResource, @Header, @Payload, @SendToUser, @ActiveProfiles, @ContextConfiguration, @Sql, @TestExecutionListeners, @TestPropertySource, @Transactional, @ControllerAdvice, @CookieValue, @CrossOrigin, @MatrixVariable, @RequestHeader, @RequestMapping, @RequestParam, @RequestPart, @ResponseStatus, @SessionAttributes, @ActionMapping, @RenderMapping, @EventListener, @TransactionalEventListener
  
    * 例如，在 Spring-test 模块中的 @ContextConfiguration 被声明成下面这样：
    ```
    public @interface ContextConfiguration {
    
        @AliasFor("locations")
        String[] value() default {};
        
        @AliasFor("value")
        String[] locations() default {};
        //...
    }
    ```
    
    * 同样的，在元注解上覆盖属性的组合注解可以使用 @AliasFor 进行细粒度控制，在有层次的属性重写情况下。事实上，现在可以给元注解的属性定义一个别名
    
    * 例如，可以自定义属性定义组合注解：
    ```
    @ContextConfiguration
    public @interface MyTestConfig {
        @AliasFor(annotation = ContextConfiguration.class, attribute = "value")
        String[] xmlFiles();
        // ...
    }
    ```
    * 更多内容请查看 Spring Annotation Programming Model

* 搜索元注解的检索算法有大量的改进。例如，本地声明的组合注解优于继承性的注解。

* 注解属性的集合（与 AnnotationAttributes 实例）可以被合成（也就是转化）为一个注解。

* 基于field的数据绑定（DirectFieldAccessor）的特性已经与当前基于property的数据绑定一样（BeanWrapper）。特别的是，基于field的绑定现在支持集合、数组和Maps。

