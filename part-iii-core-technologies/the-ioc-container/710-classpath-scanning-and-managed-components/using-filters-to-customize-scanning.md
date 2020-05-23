#### Using filters to customize scanning

默认的，声明@Component、@Repository、@Service、@Controller或自定义注解修饰的类都会被探测为组件。然而，你可以修改和扩展这种行为通过应用自定义的过滤器。可以在@Component注解中说明包括的过滤器和排除的过滤器（或使用component-scan的子元素include-filter和exclude-filter）。每个过滤器元素需要类型或表达式类型。下面的表描述的可选的过滤器选项。

*Table 7.5 Filter Types*

Filter Type | Example Expression | Description
-- | :--: | --
annotation (default) | org.example.SomeAnnotation | An annotation tobe present at the type level in target components.
assignable |org.example.SomeClass | A class (or interface) that the target components are assignable to (extend/implement).
aspectj |org.example..*Service+ | An AspectJ type expression  to be matched by the target components.
regex |org\.example\.Default.* | A regex expression to be matched by the target components class names.
custom | org.example.MyTypeFilter | A custom implementation of the org.springframework.core.type.TypeFilter interface.

下面的例子展示了所有的@Repository注解但是忽略使用Stub的repository。

```
@Configuration
@ComponentScan(basePackages = "org.example",
includeFilters = @Filter(type = FilterType.REGEX, pattern = ".*Stub.*Repository"),
excludeFilters = @Filter(Repository.class))
public class AppConfig {
    ...
}
```

相当于使用 XML 的方式：

```
<beans>
<context:component-scan base-package="org.example">
<context:include-filter type="regex"
expression=".*Stub.*Repository"/>
<context:exclude-filter type="annotation"
expression="org.springframework.stereotype.Repository"/>

    </context:component-scan>
</beans>
```



