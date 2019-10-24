在使用Maven时很有可能混合了不同版本的spring的jar文件。例如，你或许找到了第三方的库或者其他spring的工程，或者传递依赖了一个过时的版本。如果你忘记显式的进行声明，很多你意想不到的问题会出现。

为了解决这样的问题，Maven支持BOM("bill of materials")的概念依赖。你可以引入spring-framework-bom在你的依赖管理中，以保证所有的spring依赖都在同一个版本上。

```
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-framework-bom</artifactId>
            <version>4.3.4.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

使用BOM的好处在于你依赖spring框架时再也不需要声明版本号了。

```
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
    </dependency>
<dependencies>
```