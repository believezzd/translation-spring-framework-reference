#### Configuring a ConversionService 

ConversionService是一个无状态对象，设计用于在应用程序启动时实例化，然后共享在多个线程之间。在Spring应用程序中，通常配置一个ConversionService实例每个Spring容器(或ApplicationContext)。该转换服务将由Spring和然后在框架需要执行类型转换时使用。你也可以注射此服务将转换到任何bean并直接调用它。

>**Note**

>如果spring没有注册ConversionService，则原始的基于属性编辑器的系统将被使用。

为了使用spring注册默认的ConversionService，需要添加id为conversionService的如下bean定义

```
<bean id="conversionService"
class="org.springframework.context.support.ConversionServiceFactoryBean"/>
```

一个默认的ConversionService可以在字符串、数组、枚举、集合、map和其他通用类型间实现转换。为了补充或覆盖默认的转化器使用自定义的转化器，需要设置converters属性。属性值是一个实现Converter或ConverterFactory或GenericConverter接口的某个类。

```
<bean id="conversionService"
class="org.springframework.context.support.ConversionServiceFactoryBean">
    <property name="converters">
        <set>
            <bean class="example.MyCustomConverter"/>
        </set>
    </property>
</bean>
```

在spring的mvc应用中使用GenericConverter也很常见。见22.16.3节，“转换和格式化”在spring的mvc章节。

在特定的情况你或许希望在转换中使用格式化。见9.6.3节，“FormatterRegistry SPI”中的详细内容使用FormattingConversionServiceFactoryBean。

