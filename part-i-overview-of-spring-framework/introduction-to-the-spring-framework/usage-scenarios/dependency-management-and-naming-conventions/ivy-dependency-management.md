如果你倾向于使用Ivy来管理依赖，那么下面提供一下简单的配置选项。

配置Ivy指向spring的仓库，你需要修改ivysettings.xml文件。

```
<resolvers>
    <ibiblio name="io.spring.repo.maven.release"
            m2compatible="true"
            root="http://repo.spring.io/release/"/>
</resolvers>
```

可以将root URL从/release改为/milestone或/snapshot，选择一个合适的。

一旦配置好了，你就可以添加依赖了，例如（在ivy.xml文件中）

```
<dependency org="org.springframework"
        name="spring-core" rev="4.3.4.RELEASE" conf="compile->runtime"/>
```