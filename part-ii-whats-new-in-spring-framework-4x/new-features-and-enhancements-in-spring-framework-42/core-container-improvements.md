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
    // ...
  }
  ```

  * ass
  234