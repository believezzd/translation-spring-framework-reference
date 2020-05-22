##### Arbitrary method raplacement

一个比lookup method injecion更差形式的方法注入就是在一个被管理bean通过另一个方法实现来替换的任意方法。当需要的时候才看一下内容。

基于xml的配置元数据，你可以使用replaced-method元素来使得另一个方法来替换一个已经部署的bean的已有方法实现。考虑下面的这个类，有一个我们希望覆盖的computeValue方法：

```
public class MyValueCalculator {

    public String computeValue(String input) {
        // some real code...
    }

    // some other methods...
}
```

一个类只要实现org.springframework.beans.factory.support.MethodReplacer接口就可以提供新的方法实现。

```
/**
 * meant to be used to override the existing computeValue(String)
 * implementation in MyValueCalculator
 */
public class ReplacementComputeValue implements MethodReplacer {

    public Object reimplement(Object o, Method m, Object[] args) throws Throwable {
        // get the input value, work with it, and return a computed result
        String input = (String) args[0];
        ...
        return ...;
    }
}
```

原有类的bean定义和替换方法的定义是这样的：

```
<bean id="myValueCalculator" class="x.y.z.MyValueCalculator">
    <!-- arbitrary method replacement -->
    <replaced-method name="computeValue" replacer="replacementComputeValue">
        <arg-type>String</arg-type>
    </replaced-method>
</bean>

<bean id="replacementComputeValue" class="a.b.c.ReplacementComputeValue"/>
```

你可以在<replaced-method/>元素中使用一个或多个<arg-type/>元素来声明需要被覆盖的方法签名。在覆盖的方法有重载或多个参数时是必须的。为了方便，字符串类型可以使用全限定名的简写。例如下面的写法都是字符串类型：

```
java.lang.String
String
Str
```

因为参数的个数通常足够可以区分不同的方法，所以这样的简写可以减少输入，并且允许你输入简写就可以匹配参数。