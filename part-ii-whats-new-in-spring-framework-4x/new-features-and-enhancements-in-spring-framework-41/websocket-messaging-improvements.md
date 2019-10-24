- 客户端支持SockJS。将SockJsClient和相同包中类。

- 当STOMP的客户端订阅或取消订阅时，新的应用端上下文事件SessionSubscribeEvent和SessionUnsubscribeEvent被触发。

- 新的WebSocket范围。详见26.4.14章节，WebSocket的范围。

- @SendToUser只能标记一个单一的会话，并且不再需要授权用户。

- @MessageMapping的方法可以使用圆点来代替斜线作为路径分割符。见SPR-11660。

- STOMP/WebSocket的监控信息可以被收集和记录。见26.4.16章，运行时监控。

- 即时在DEBUG级别上，具有显著改进作用的日志应当保持易于理解且简洁。

- 化消息的创建，支持临时可变的消息并且避免自动消息id和timestamp的创建。见MessageHeaderAccessor的Javadoc。

- 当WebSocket会话被创建后，在60秒内不允许关闭STOMP/WebSocket连接。见SPR-11884。