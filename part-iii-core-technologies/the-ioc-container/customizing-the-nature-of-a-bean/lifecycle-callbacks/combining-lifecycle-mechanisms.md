##### Combining lifecycle mechanisms

Spring 2.5时代，总共有 3 种可选的方法用来控制 bean 的 生命周期行为：Initialization 和 DisposableBean 回调接口、自定义 init() 和 destroy() 方法、@PostConstruct 和 @PreDestory 注解。可以组合使用这些机制。

>**Note**

> 同一个bean 配置了多种生命周期机制，每种配置了不同的方法名，那么每种配置方法都会按照下面的顺序来执行。然而，如果配置了同名的方法，例如init用于初始化方法，多于一个的生命周期策略，方法只会被执行一次，在之前的章节解释的那样。

多个生命周期策略被定义给同一个bean，并使用不同的初始化方法名，会按照下面的顺序来调用：

* @PostConstruct 注解的方法
* 实现 InitializatingBean 接口的 afterPropertiesSet() 方法
* 自定义的 init() 方法

销毁的方法是按照下面的顺序来调用的：

* @PreDestroy 注解的方法
* 实现 DisposableBean 接口的 destroy() 方法
* 自定义的 destroy() 方法






