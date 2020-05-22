##### Startup and shutdown callbacks

Lifecycle接口定义了对于一个对象在生命周期中需要的方法（例如，启动和关闭一些后台的进程）:

```
public interface Lifecycle {
    void start();
    void stop();
    boolean isRunning();
}
```

任何spring管理的object可以实现这个接口。然后，当ApplicationContext收到开始和关闭的信号时，例如，在运行时收到了stop或restart指令就会在适当的时候调用这些方法。容器实现这个功能是通过将其代理给LifecycleProcessor来实现的：

```
public interface LifecycleProcessor extends Lifecycle {
    void onRefresh();
    void onClose();
}
```

