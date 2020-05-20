### 4.1 JMS Improvements

spring4.1提供了一种更简单的方式注册JMS listener通过使用@JmsListener来修饰一个方法。XML的命名空间支持一种新的方式(jms:annotation-driven)，并且也可以通过java来配置使用(@EnableJms, JmsListenerContainerFactory)。也可以通过编程使用JmsListenerConfigurer来注册listener。

spring4.1也将JMS支持与spring4.0中的spring-messaging抽象结合起来，那就是：

- 消息监听端提供一个更加方便的标识并且可以更加便利通过消息的主机例如@Payload、@Header、@Headers和@SendTo。也支持使用标准的Message代替javax.jsm.Message作为一个方法的参数。

- 个新的JmsMessageOperations接口被添加，允许类似JmsTemplate的操作使用Message抽象。

最后，spring4.1提供的其他改进:

- 在JmsTemplate中支持同步的request-reply操作。

- Listener的优先级可以在每个<jms:listener/>中进行声明。

- 消息listener容器的恢复操作，可以使用BackOff的实现进行配置。

- 支持JMS2.0中的shared consumers。