在java5中引入的泛型，你可以使用这种强类型的集合。这意味着你可以定义一个只包含字符串元素的集合类型（举个例子而已）。如果你使用spring来依赖注入一个强类型的集合到一个bean中，你可以使用spring的类型转换支持以至于你定义的强类型集合中的元素可以转化为你之前添加到集合中的内容。

```
public class Foo {
    private Map<String, Float> accounts;

    public void setAccounts(Map<String, Float> accounts) {
        this.accounts = accounts;
    }
}
```

```
<beans>
    <bean id="foo" class="x.y.Foo">
        <property name="accounts">
            <map>
                <entry key="one" value="9.99"/>
                <entry key="two" value="2.75"/>
                <entry key="six" value="3.99"/>
            </map>
        </property>
    </bean>
</beans>
```

当foo的bean中accounts属性准备注入时，元素强类型Map<String, Float>的泛型信息将会通过反射获得。spring的类型转换组件会意识到这是Float类型，然后字符串类型的9.99、2.75和3.99会转换成Float类型。