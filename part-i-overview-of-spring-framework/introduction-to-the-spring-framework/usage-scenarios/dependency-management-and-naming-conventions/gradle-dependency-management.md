使用Gradle管理spring来构建系统，包括适合的URL地址。

```
repositories {
    mavenCentral()
    // and optionally...
    maven { url "http://repo.spring.io/release" }
}
```

你可以将URL从/release改为/milestone或/snapshot，选择一个合适的。一旦仓库配置好了，你就可以使用Gradle的方式配置依赖了。

```
dependencies {
    compile("org.springframework:spring-context:4.3.4.RELEASE")
    testCompile("org.springframework:spring-test:4.3.4.RELEASE")
}
```

