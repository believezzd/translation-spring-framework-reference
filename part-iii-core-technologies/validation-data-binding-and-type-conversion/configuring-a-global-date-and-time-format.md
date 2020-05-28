### 9.7 Configuring a global date & time format

默认，日期和时间域没有使用@DateTimeFormat来修饰可以使用DateFormat.SHORT的风格从字符串来转化。如果你需要，你可以通过定义你自己的全局格式来改变。

你将会需要确保spring没有注册默认的格式化器，作为代替你需要手动注册所有的格式化去。使用org.springframework.format.datetime.joda.JodaTimeFormatterRegistrar或org.springframework.format.datetime.DateFormatterRegistrar类根据你是否使用Joda时间库。

例如，下面的Java配置注册了一个全局的“yyyyMMdd”格式。这个例子不需要依赖Joda时间库。

```
@Configuration
public class AppConfig {
    @Bean
    public FormattingConversionService conversionService() {
        // Use the DefaultFormattingConversionService but do not register defaults
        DefaultFormattingConversionService conversionService = new
        DefaultFormattingConversionService(false);
        // Ensure @NumberFormat is still supported
        conversionService.addFormatterForFieldAnnotation(new NumberFormatAnnotationFormatterFactory());
        // Register date conversion with a specific global format
        DateFormatterRegistrar registrar = new DateFormatterRegistrar();
        registrar.setFormatter(new DateFormatter("yyyyMMdd"));
        registrar.registerFormatters(conversionService);
        return conversionService;
    }
}
```

如果你倾向于基于xml的配置你可以使用FormattingConversionServiceFactoryBean。这里给一个例子，其中这里的时间使用了Joda时间库：

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="
http://www.springframework.org/schema/beans
https://www.springframework.org/schema/beans/spring-beans.xsd>
    <bean id="conversionService" class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
        <property name="registerDefaultFormatters" value="false" />
        <property name="formatters">
            <set>
                <bean class="org.springframework.format.number.NumberFormatAnnotationFormatterFactory" />
            </set>
        </property>
        
        <property name="formatterRegistrars">
            <set>
                <bean class="org.springframework.format.datetime.joda.JodaTimeFormatterRegistrar">
                    <property name="dateFormatter">
                        <bean class="org.springframework.format.datetime.joda.DateTimeFormatterFactoryBean">
                            <property name="pattern" value="yyyyMMdd"/>
                        </bean>
                    </property>
                </bean>
            </set>
        </property>
    </bean>
</beans>
```

>**Note**

>Joda时间提供了分割的域来代表日期、时间和日期时间值。JodaTimeFormatterRegistrar类的dateFormatter、timeFormatter和dateTimeFormatter属性应该被使用用于配置不同的类型的格式。DateTimeFormatterFactoryBean提供了一个方便的方式用于创建格式化器。

如果你使用spring的mvc，记住明确配置需要使用的转换服务。对于@Configuration注解来说这意味着扩展WebMvcConfigurationSupport类和覆盖mvcConversionService方法。对于使用xml，你应当使用mvc:annotation-driven元素的conversion-service属性。见22.16.3，“转换和格式化”

