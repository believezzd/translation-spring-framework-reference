##### Maven Dependency Management

如果你使用Maven作为依赖管理，那么你不用显式的声明对logging的依赖。例如，如果使用依赖注入来构建一个应用，你的Maven文件看起来应该是这样的。

```
<dependencies>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-context</artifactId>
		<version>4.3.9.RELEASE</version>
		<scope>runtime</scope>
	</dependency>
</dependencies>
```

就这些。声明范围是运行时，如果你不需要再编译时使用spring的api的话。这是典型的使用依赖注入的例子。

上面的例子需要Maven的中心仓库才可以工作。如果使用spring的Maven仓库（例如开发版本或milestones），你需要设定仓库的位置在Maven的设置宏。比如所有的release版本如下：

```
<repositories>
    <repository>
        <id>io.spring.repo.maven.release</id>
        <url>http://repo.spring.io/release/</url>
        <snapshots><enabled>false</enabled></snapshots>
    </repository>
</repositories>
```

如果是milestones的话如下：

```
<repositories>
    <repository>
        <id>io.spring.repo.maven.milestone</id>
        <url>http://repo.spring.io/milestone/</url>
        <snapshots><enabled>false</enabled></snapshots>
    </repository>
</repositories>
```

如果是镜像的话如下：

```
<repositories>
    <repository>
        <id>io.spring.repo.maven.snapshot</id>
        <url>http://repo.spring.io/snapshot/</url>
        <snapshots><enabled>true</enabled></snapshots>
    </repository>
</repositories>
```