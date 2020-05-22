##### Combining lifecycle mechanisms

Spring 2.5时代，总共有 3 种可选的方法用来控制 bean 的 生命周期行为：Initialization 和 DisposableBean 回调接口、自定义 init() 和 destroy() 方法、@PostConstruct 和 @PreDestory 注解。可以组合使用这些机制。

>**Note**

> 在多个选择控制bean的生命周期的方式，每个方法都被配置成不同的方法名，并且每一个方法都会按照下面的顺序来执行。然而，如果配置了同名的方法，例如init用于初始化方法，多于一个的生命周期策略，方法只会被执行一次，在之前的章节解释的那样。