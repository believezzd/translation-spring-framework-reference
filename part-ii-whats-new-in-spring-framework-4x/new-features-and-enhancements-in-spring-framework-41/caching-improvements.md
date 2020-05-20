### Caching Improvements

spring4.1支持JCache（JSR-107）声明，使用spring已经存在的cache配置和基础抽象。使用标准的注解，不需要修改。

spring4.1也显著提升了其自身的cacheing机制：

- 通过CacheResolver可以在运行时使用缓存。缓存名的定义不再是必需的。

- 更多的自定义操作：缓存解析、缓存管理、key的生成。

- 一个新的可以修饰类的注释@CacheConfig允许通常的设置用于共享而不需要任何缓存操作。

- 使用CacheErrorHandler实现更好的缓存方法的异常处理。

Spring4.1还有一个很大的改进就是添加了一个putIfAbsent的方法在Cache的接口中。