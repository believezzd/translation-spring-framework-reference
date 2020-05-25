##### Asynchronous Listeners

如果你希望一个特殊的监听器用于异步处理事件，简单的方法就是使用@Async作为支持：

```
@EventListener
@Async
public void processBlackListEvent(BlackListEvent event) {
    // BlackListEvent is processed in a separate thread
}
```

当使用异步事件处理时应该注意的限制

* 如果事件监听器抛出了一个异常将不会传播给调用者，详见AsyncUncaughtExceptionHandler。
* 这样的事件监听器不能发送回复。如你希望发送另一个事件作为处理结果，手动注入ApplicationEventPublisher用于发送事件。