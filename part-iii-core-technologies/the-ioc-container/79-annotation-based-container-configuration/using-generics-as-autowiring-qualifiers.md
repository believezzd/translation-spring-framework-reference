#### Using generics as autowiring qualifiers

除了@Qualifier注解外，也可以使用java泛型作为qualification的一种实现形式。例如，假设你有这样的配置：

```
@Configuration
public class MyConfiguration {
    @Bean
    public StringStore stringStore() {
        return new StringStore();
    }
    
    @Bean
    public IntegerStore integerStore() {
        return new IntegerStore();
    }
}
```

假设上面的beans实现了泛型接口，例如Store<String>和Store<Integer>，你可以用@Autowired来修饰Store接口，泛型将会被使用当做一个qualifier：

```
@Autowired
private Store<String> s1; // <String> qualifier, injects the stringStore bean

@Autowired
private Store<Integer> s2; // <Integer> qualifier, injects the integerStore bean
```

泛型的qualifier也支持自动注入List、Maps和数组

```
// Inject all Store beans as long as they have an <Integer> generic
// Store<String> beans will not appear in this list
@Autowired
private List<Store<Integer>> s;
```
