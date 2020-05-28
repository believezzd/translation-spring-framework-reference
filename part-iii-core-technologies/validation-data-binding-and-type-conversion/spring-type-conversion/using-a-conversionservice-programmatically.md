#### Using a ConversionService programmatically

为了编程使用ConversionService实例，简单在一个bean中注入一个引用如下：

```
@Service
public class MyService {
    @Autowired
    public MyService(ConversionService conversionService) {
        this.conversionService = conversionService;
    }
    public void doIt() {
        this.conversionService.convert(...)
    }
}
```

在大部分使用方式中，转换方法定义了目标类型可以被使用但是但是在一些参数化元素的集合类型这样复杂的类型可能无法工作。如果你希望编程转换一个Integer的List到一个字符串的List，例如，你需要提供一个格式定义对源类型和目标类型。

幸运的是，TypeDescriptor提供了一些选项用于实现这样的功能：

```
DefaultConversionService cs = new DefaultConversionService();

List<Integer> input = ....
cs.convert(input,
TypeDescriptor.forObject(input), // List<Integer> type descriptor
TypeDescriptor.collection(List.class, TypeDescriptor.valueOf(String.class)));
```

注意DefaultConversionService自动注册转换器可以使用于大部分的情况。这里包括集合转换、标量转换和基本object到字符串转化。通过ConverterRegistry同样的转换可以被注册使用DefaultConversionService类中的静态的addDefaultConverters方法。

用于值类型的转换可以重用在数组和集合中，因此不需要创建特定的转换器用于转换一个集合S到另一个集合T，假设标准集合处理是合适的。
