##### Using JUL(java.util.logging)

如果没有检测到Log4j, 默认情况下Commons Logging将委托给java.util.logging。因此，不需要设置特殊的依赖项:只需要在没有外部的情况下使用Spring日志输出到java.util的依赖性。日志记录，可以在独立应用程序中(使用JDK级别上的自定义或默认的JUL设置)，也可以在应用服务器的日志系统(及其系统范围设置)。