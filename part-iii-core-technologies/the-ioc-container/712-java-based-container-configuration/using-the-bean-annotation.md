#### Using the @Bean annotation

@Bean是方法级别的注解和xml中bean元素类似。这个注解支持bean中提供的一些属性，例如：init-method、destroy-method、autowiring和name。

你可以在@Configuration类或@Component类中使用@Bean注解。