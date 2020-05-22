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

意LifecycleProcessor是Lifecycle接口的扩展。他增加了两个方法用于容器的refresh和close。

>**Tip**

> 注意org.springframework.context.Lifecycle接口只是定义了start和stop方法，但是没有包括在上下文刷新的auto-startup。考虑实现org.springframework.context.SmartLifecycle接口会更加细粒度地控制auto-startup对于特定的bean（包括startup部分）。而且，请注意stop声明并不保证在销毁前执行。在正常的关闭中，所有生命周期方法会首先收到一个stop的提示在通常销毁方法回调之前。然而在上下文的情况下热重启或者放弃热重启的尝试时，只有destroy方法会被调用。

开始和关闭的调用可以很重要。如果在两个object之间存在依赖的关系，依赖一方的启动将会晚于被依赖的一方，并且会比被依赖的一方优先被关闭。然而，有时直接依赖是不清楚的。你或许只能知道某些类型的obj会比另一个类型的object早。在这样的情况下，SmartLifecycle定义了另外一个选项，叫做getPhase方法，这个方法定义在他的父接口Phased中。

```
public interface Phased {
    int getPhase();
}
```

```
public interface SmartLifecycle extends Lifecycle, Phased {
    boolean isAutoStartup();
    void stop(Runnable callback);
}
```


