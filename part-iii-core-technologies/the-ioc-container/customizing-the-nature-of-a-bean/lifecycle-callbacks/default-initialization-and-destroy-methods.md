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
