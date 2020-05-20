##### Using SLF4J with Log4j or Logback

SLF4J(The Simple Logging Facade for Java)是其他常用库使用的一种流行API
并通常与Spring一起使用。它通常与Logback一起使用，Logback是SLF4J API的原生实现。

SLF4J提供了对许多常见日志框架(包括Log4j)的绑定，它还可以反向绑定:连接其他日志框架和自身的桥梁。因此，要在Spring中使用SLF4J，您需要使用SLF4J-JCL桥来替换common -logging依赖项。一旦你做到了，来自Spring的日志调用将转换为对SLF4J API的日志调用，因此如果是其他调用
应用程序中的库使用该API，这样就有了一个单独的位置来配置和管理日志记录。

常见的选择可能是将Spring桥接到SLF4J，然后提供从SLF4J到的显式绑定Log4j。您需要提供几个依赖项(并排除现有的公共日志记录)JCL桥、到Log4j的SLF4j绑定以及Log4j提供者本身。在Maven中，您会这样做：
```
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>4.3.9.RELEASE</version>
        <exclusions>
            <exclusion>
                <groupId>commons-logging</groupId>
                <artifactId>commons-logging</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>jcl-over-slf4j</artifactId>
        <version>1.7.21</version>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-log4j12</artifactId>
        <version>1.7.21</version>
    </dependency>
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
    </dependency>
</dependencies>
```

SLF4J用户中更常见的一种选择，它使用更少的步骤并生成更少的内容依赖项，是直接绑定到Logback。这将删除额外的绑定步骤，因为是Logback直接实现SLF4J，因此您只需要依赖于两个库，即jcl-over-slf4j和logback):

```
<dependencies>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>jcl-over-slf4j</artifactId>
        <version>1.7.21</version>
    </dependency>
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-classic</artifactId>
        <version>1.1.7</version>
    </dependency>
</dependencies>
```

