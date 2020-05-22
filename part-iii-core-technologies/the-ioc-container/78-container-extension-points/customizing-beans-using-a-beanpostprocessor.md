#### Customizing beans using a BeanPostProcessor

BeanPostProcessor接口定义了回调方法，你可以实现这个接口并提供自己的实现集成逻辑（或者覆盖容器中默认的实现）、依赖处理逻辑或其他的。如果你希望在spring容器完成初始化、配置和初始化bean之后做一些逻辑处理，你可以实现一个或多个BeanPostProcessor实现。

