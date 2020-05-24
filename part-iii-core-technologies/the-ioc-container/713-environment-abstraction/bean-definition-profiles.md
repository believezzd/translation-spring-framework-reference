#### Bean definition profiles

bean定义profile是一种在核心容器中的策略允许注册在不同环境中的不同的bean。环境这个单词可以意味着不同的用户使用不同的内容，这个特性在很多方面有用，包括：

* 在开发中使用内置的数据源 vs 在QA或产品中需找相同的数据源
* 当部署一个应用到一个环境是注册监控的基础组件
* 为客户A注册自定义的bean实现 vs 客户B部署

我们考虑一个在特定应用中需要数据源的例子。在测试环境，配置或许是这样的：

```
@Bean
public DataSource dataSource() {
    return new EmbeddedDatabaseBuilder()
        .setType(EmbeddedDatabaseType.HSQL)
        .addScript("my-schema.sql")
        .addScript("my-test-data.sql")
        .build();
}
```

让我们考虑一下这样的应用如何部署到QA或生产环境中，假设应用的数据源将会被应用服务器的JNDI注册。我们的数据现在可以是这样的：

```
@Bean(destroyMethod="")
public DataSource dataSource() throws Exception {
    Context ctx = new InitialContext();
    return (DataSource) ctx.lookup("java:comp/env/jdbc/datasource");
}
```

问题在于如果根据现有的环境切换两种不同的配置。spring花了很长时间提供了很多方法来处理这种情况，通常依赖于系统环境变量和xml中的import定义包括${placeholder}来处理当前的配置文件路径依赖一个系统变量。bean定义profile是一个重要的容器特性提供了这个问题的一种解决方案。

如果我们使用基于环境的bean定义的例子，我们不需要在特定的上下文注册特定bean的定义。你可以在不同情况下使用不同的profile。让我们首先看一下如何根据我们的需要更新配置。




