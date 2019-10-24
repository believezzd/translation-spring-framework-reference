Servlet2.5虽然依然保留，但是spring框架4.0已经将注意转移到Servlet3.0以上的环境中。如果你正在使用Spring MVC测试框架，你要确保Servlet3.0相关的JAR在你的classpath中。

除了后面提到的支持WebSocket外，也包括以下在spring web模块改进的部分。

- 你可以在spring mvc应用中使用@RestController，去除了需要在你的@RequestMapping方法上添加@ResponseBody的必要。

- 添加了AsyncRestTemplate类，允许开发非阻塞的REST客户端。

- 在开发spring mvc应用中支持功能强大的时区设置。