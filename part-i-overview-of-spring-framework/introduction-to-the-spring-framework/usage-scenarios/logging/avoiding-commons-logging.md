##### Avoiding Commons Logging

不幸的是，标准的公共日志API中的运行时发现算法对于终端用户来说很方便，但可能会有问题。如果您想避免JCL的标准查找，基本上有两种方法来关闭它:

1. 将依赖项从spring-core模块中排除(因为它是惟一显式地依赖commons-logging的模块)
2. 依赖于用空jar替换库的特殊公共日志依赖项(更多细节可以在SLF4J FAQ中找到)

要排除公共日志记录，请将以下内容添加到dependencyManagement部分

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
</dependencies>
```

由于没有JCL API的实现在类路径上，所以该应用程序当前已损坏，因此要修复它，必须提供一个新的。在下一节中，我们将向您展示如何使用SLF4J作为JCL的另一种实现。