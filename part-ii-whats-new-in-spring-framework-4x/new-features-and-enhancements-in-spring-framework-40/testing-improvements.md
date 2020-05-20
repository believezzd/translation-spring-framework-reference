### 3.9 Testing Improments

除了去除在spring-test模块中一些过时的代码，spring框架4.0在单元和集成测试中加入了新的特性:

- 所有在spring-test模块中的注解（例如 @ContextConfiguration、@WebAppConfiguration、@ContextHierarchy、@ActiveProfiles等等）现在都可以作为元注解来自定义注解以减少在测试中的配置。

- Active bean definition profiles现在可以通过编程来解决，通过自定义实现ActiveProfilesResolver和使用@ActiveProfiles。

- 一个新的SocketUtils类被增加到spring-core模块，可以使得你在本机上可以扫描TCP和UDP服务器端口。这个功能不仅仅是为了测试，更是为了提供测试时使用socket的帮助。例如测试一个in-memory SMTP服务器、FTP服务器、Servlet容器等。

- 在spring4.0中，一系列在org.springframework.mock.web包的mocks现在是基于Servlet3.0的API。此外，一些Servlet的API mocks（例如MockHttpServletRequest、MockServletContext等）有了一小部分的改进。