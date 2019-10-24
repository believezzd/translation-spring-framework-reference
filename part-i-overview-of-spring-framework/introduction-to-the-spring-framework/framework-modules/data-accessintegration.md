#### Data Access/Integration

数据访问好集成包括JDBC、ORM、OXM、JMS和事务模块。

spring-jdbc模块提供了jdbc的抽象处理，屏蔽了冗长的jdbc编码，也屏蔽了不同数据库平台的错误代码。

spring-tx模块支持编程式和声明式的事务管理，支持实现特殊接口的类和你所有的POJO。

spring-orm模块为流行的对象关系映射模型提供了集成框架，包括JPA、JDO和Hibernate。使用spring-orm模块你可以使用所有的orm框架并且与spring的其他特性相结合，例如前面提到的简单的声明式事务管理。

spring-oxm模块提供了一个支持对象XML映射模型的抽象层，包括JAXB、Castor、XMLBeans、JiBX和XStream。

pring-jms模块，Java消息服务包括生产和消费消息的特性。自从spring框架4.1开始，他就提供了与spring-messaging模块的集成。