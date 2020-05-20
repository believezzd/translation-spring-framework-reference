##### Using Log4j 1.2 or 2.x

>**Note**

>Log4j 1.2 is EOL in the meantime. Also, Log4j 2.3 is the last Java 6 compatible release, with newer Log4j 2.x releases requiring Java 7+.

许多人使用Log4j作为日志的配置和管理框架。它是高效且完善，事实上这就是我们在构建Spring时在运行时所使用的。Spring
还提供了一些配置和初始化Log4j的实用程序，因此在一些模块中有一个对Log4j可选的编译时依赖。

要使Log4j 1.2使用默认的JCL依赖项(common -logging)工作，您所需要做的就是将Log4j放到类路径中，并为其提供配置文件(Log4j)。属性或log4j.xml在类路径的根目录中)。对于Maven用户，这是依赖项声明：

```
<dependencies>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-core</artifactId>
		<version>4.3.9.RELEASE</version>
	</dependency>
	<dependency>
		<groupId>log4j</groupId>
		<artifactId>log4j</artifactId>
		<version>1.2.17</version>
	</dependency>
</dependencies>
```

下面是一个log4j.properties配置示例，用于在控制台打印日志：

```
log4j.rootCategory=INFO, stdout

log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{ABSOLUTE} %5p %t %c{2}:%L - %m%n

log4j.category.org.springframework.beans.factory=DEBUG
```

要在JCL中使用Log4j 2，您所需要做的就是将Log4j放到类路径中，并为它提供一个配置文件(log4j2。xml, log4j2。属性或其他受支持的配置格式)。为Maven用户所需的最小依赖项为:

```
<dependencies>
	<dependency>
		<groupId>org.apache.logging.log4j</groupId>
		<artifactId>log4j-core</artifactId>
		<version>2.6.2</version>
	</dependency>
	<dependency>
		<groupId>org.apache.logging.log4j</groupId>
		<artifactId>log4j-jcl</artifactId>
		<version>2.6.2</version>
	</dependency>
</dependencies>
```

如果您还希望使用SLF4J代理Log4j，例如对于默认使用SLF4J的其他库，还需要以下依赖项:

```
<dependencies>
	<dependency>
		<groupId>org.apache.logging.log4j</groupId>
		<artifactId>log4j-slf4j-impl</artifactId>
		<version>2.6.2</version>
	</dependency>
</dependencies>
```

这里是一个log4j2.xml配置示例，用于在控制台打印日志：

```
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="WARN">
	<Appenders>
		<Console name="Console" target="SYSTEM_OUT">
			<PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
		</Console>
	</Appenders>
	<Loggers>
	<Logger name="org.springframework.beans.factory" level="DEBUG"/>
		<Root level="error">
			<AppenderRef ref="Console"/>
		</Root>
	</Loggers>
</Configuration>
```



