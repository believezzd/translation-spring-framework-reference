- 目前对资源的处理是基于ResourceHttpRequestHandler的，并且被扩展成一个新的抽象集合ResourceResolver、ResourceTransformer和ResourceUrlProvider。内部一系列实现为了支持versioned resource URLs(为了更有效的HTTP缓存)，定位压缩的资源，生成HTML5 AppCache的清单等等。详细见22.16.9章 为了资源服务。

- JDK1.8中的java.util.Optional现在为@RequestParam、@RequestHeader、and @MatrixVariable控制器方法参数提供支持。

- ListenableFuture目前作为一个返回值代替了DeferredResult当使用underlying服务时（或许可以叫AsyncRestTemplate）。

- @ModelAttribute的方法可以在切面间依赖中调用。见SPR-6299。

- Jackson的@JsonView可以直接支持在@ResponseBody和ResponseEntity控制器方法，用于序列化不同的POJO（例如摘要页面和细节页面）。他也支持基于页面的渲染，通过添加序列化的视图类型作为模型的属性使用一种特殊的key。详情见“Jackson Serialization View Support”。

- 在切入@ResponseBody和ResponseEntity方法之间添加了新的生命周期，在控制方法被调用后，返回被写入之前。为了利用@ControllerAdvice的声明实现了ResponseBodyAdvice。内部对@JsonView和JSONP的支持利用这点。详情见章节22.4.1, “Intercepting requests with a HandlerInterceptor”。

- 下面是3个新的HttpMessageConverter选项：

   - Gson，比Jackson更好的footprint已经在spring android中使用了。
   
   - Google协议缓存，有效和有用的相互通信数据协议在企业中使用的，可以通过JSON和XML为浏览器展示。
   
   - 使用jackson-dataformat-xml扩展可以支持Jackson based XML序列化.如果jackson-dataformat-xml在classpath中时，使用@EnableWebMvc或<mvc:annotation-driven/>，将被默认替代JAXB2。

- 像JSP那样的视图可以链接到控制器通过和控制器名称相互绑定。默认的名称已经通过@RequestMapping来分配。例如，拥有handleFoo方法的控制器FooController就叫"FC#handleFoo"。命名规则是可扩展的。也可以在@RequestMapping明确地使用名称的属性命名。一个在spring jsp标签库中新的mvcUrl方法可以使得这件事在jsp页面使用更加方便。详情见22.7.2章节，“Building URIs to Controllers and methods from views”。

- ResponseEntity提供了一个内建形式的API，用于指导控制器方法服务器端的返回准备操作。例如ResponseEntity.ok()。

- RequestEntity是一个新的类型，用于支持内建API来指导客户端REST代码来准备HTTP请求。

- MVC Java配置和XML命名空间

   - 视图解析现在可以被配置，而且支持content negotiation，详情见22.16.8章，“View Resolvers”
   - 视图控制器现在有一个内置功能用来支持重定向和设置返回状态。应用可以使用这个来配置转发路径，返回404错误码通过视图，发送没有内容的返回等等。一些使用的例子列在这里。
   - 自定义路径匹配可以自由使用并且已经内嵌，详见22.16.11章，Path Matching。

- 持Groovy标记模板（基于Groovy2.3）。见GroovyMarkupConfigurer和ViewResolver的实现。







