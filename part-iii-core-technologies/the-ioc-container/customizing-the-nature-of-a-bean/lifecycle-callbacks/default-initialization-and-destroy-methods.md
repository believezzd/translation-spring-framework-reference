##### Default initialization and destroy methods

当你编写初始化或者销毁回调而不是使用 Spring 定义的 InitializatingBean 和 DisoposableBean 回调接口的时候，你通常将这些方法命名为 init(), initialize(), dispose()等等，理想状态下，这些声明周期回调的方法名在整个项目中是标准化的，所有开发者都使用相同的命名并保持一致性。

你可以配置 Spring 容器通过名字检索来查找初始化和销毁回调方法给所有的 bean。也就是说，作为开发者，可以编码应用类使用 init() 来定义初始化回调，却不用为每一个 bean 的定义配置 init-method = "init"。Spring 容器会自动调用这个方法当 bean 创建的时候（同样适用于之前章节描述的标准生命周期回调）。这个特性要求为初始化和销毁回调定义一致的命名规范。

假设你的初始化回调方法名为 init()，销毁回调方法名为 destroy()。你会定义如下例子的类:

```
public class DefaultBlogService implements BlogService {

    private BlogDao blogDao;
    
    public void setBlogDao(BlogDao blogDao) {
        this.blogDao = blogDao;
    }
    
    // this is (unsurprisingly) the initialization callback method
    public void init() {
        if (this.blogDao == null) {
            throw new IllegalStateException("The [blogDao] property must be set.");
        }
    }
}
```

```
<beans default-init-method="init">
    <bean id="blogService" class="com.foo.DefaultBlogService">
        <property name="blogDao" ref="blogDao" />
    </bean>
</beans>
```

当前在 <bean/> 中的 default-init-method 属性配置导致 Spring 容器识别 init 方法，把其当成初始化回调。当 bean 创建且组装好后，如果 bean 有这个方法，这个方法会在适当的时机被回调。

配置销毁回调也是差不多的，通过使用在 <beans/> 上的 default-destory-method 属性。

如果已经存在的类已经有了不同于规范命名的回调方法，可以使用 init-method 和 desotry-method 来重写默认配置

spring容器可以保证一个配置的初始化回调方法会在所有依赖注入完成后被调用。初始化方法作为原始bean的引用被调用，也就意味着AOP拦截器还没有应用于这个bean。目标首先被创建，然后AOP代理的拦截链才会被应用。如果目标bean和代理是分开定义的，你的代码可以通过代理与原始目标bean产生联系。因此，将一个拦截器用于初始化方法是相悖的，因为这样做会导致目标bean的生命周期和他的代理或拦截器相互耦合并且会留下莫名的语义当你的代码和原始目标对象相关联时。













