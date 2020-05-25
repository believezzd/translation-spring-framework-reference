##### Ordering listeners

如果你希望这个监听器在另一个监听器之前执行，需要在方法声明中添加@Order注解：

```
@EventListener
@Order(42)
public void processBlackListEvent(BlackListEvent event) {
    // notify appropriate parties via notificationAddress...
}
```

