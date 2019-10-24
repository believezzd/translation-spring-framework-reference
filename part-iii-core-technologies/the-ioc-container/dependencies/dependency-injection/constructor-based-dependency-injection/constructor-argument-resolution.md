构造器参数解析产生在使用参数的类型。如果一个构造器参数的bean定义没有潜在的歧义存在，那么构造器中参数的位置和提供给构造的参数依赖必须是相同的顺序。考虑下面的例子:

```
package x.y;

public class Foo {
    public Foo(Bar bar, Baz baz) {
    // ...
    }
}
```

没有歧义存在，假设Bar和Baz没有直接的继承关系。那么下面的配置是没有问题，而且你不需要在<constructor-arg/>元素中定义构造器参数的index或者明确的类型。

```
<beans>
    <bean id="foo" class="x.y.Foo">
        <constructor-arg ref="bar"/>
        <constructor-arg ref="baz"/>
    </bean>
    
    <bean id="bar" class="x.y.Bar"/>
    <bean id="baz" class="x.y.Baz"/>
</beans>
```

如果有另一个bean被引用，类型是已知的，并且可以匹配（就像前面的例子）。当一个简单的类型被使用，例如<value>true</value>，spring不能决定值的类型，因此就无法匹配。考虑下面的这个类：

```
package examples;

public class ExampleBean {
    // Number of years to calculate the Ultimate Answer
    private int years;
    
    // The Answer to Life, the Universe, and Everything
    private String ultimateAnswer;
    
    public ExampleBean(int years, String ultimateAnswer) {
        this.years = years;
        this.ultimateAnswer = ultimateAnswer;
    }
}
```

在上面的例子中，如果你可以在构造器的参数声明中指定type属性，那么容器的类型匹配会很容易。

```
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg type="int" value="7500000"/>
    <constructor-arg type="java.lang.String" value="42"/>
</bean>
```

使用index来声明构造器参数的位置，例如：

```
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg index="0" value="7500000"/>
    <constructor-arg index="1" value="42"/>
</bean>
```

此外解决了多个简单类型的歧义，定义index当构造器的有两个相似类型的参数时。注意index是从0开始的。

你也可以使用构造器参数的name来消除value的歧义。

```
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg name="years" value="7500000"/>
    <constructor-arg name="ultimateAnswer" value="42"/>
</bean>
```

注意，为了让这种方式可以起作用，你必须用debug标志来编译你的代码，以便spring可以根据参数名找到构造器。如果你不能使用debug的模式编译代码（或者你不想）你可以使用JDK提供的@ConstructorProperties注解来声明你的构造器参数的名字。下面有个简单的类可供参考：

```
package examples;

public class ExampleBean {
    // Fields omitted
    @ConstructorProperties({"years", "ultimateAnswer"})
    public ExampleBean(int years, String ultimateAnswer) {
        this.years = years;
        this.ultimateAnswer = ultimateAnswer;
    }
}
```
