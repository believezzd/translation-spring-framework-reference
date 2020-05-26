### 9.2 Validation using Spring's Validator interface

你可以使用spring的验证器接口的来验证object。验证器接口使用一个Errors的object来工作用于完成验证，验证器可以对验证失败的object提供验证报告。

让我们考虑一个小的object:

```
public class Person {
    private String name;
    private int age;
    // the usual getters and setters...
}
```

我们通过实现org.springframework.validation.Validator接口中的两个方法用于对Person类添加验证行为。

* supports(Class) - 验证器验证的实例是否是提供的类的实例？
* validate(Object, org.springframework.validation.Errors) - 验证给定的object是否是由于验证的error，注册在给定的Errors的object上

直接实现Valiator接口，尤其是当你知道spring框架提供了一个ValidationUtils的工具类时。

```
public class PersonValidator implements Validator {
    /**
    * This Validator validates *just* Person instances
    */
    public boolean supports(Class clazz) {
        return Person.class.equals(clazz);
    }
    public void validate(Object obj, Errors e) {
        ValidationUtils.rejectIfEmpty(e, "name", "name.empty");
        Person p = (Person) obj;
        if (p.getAge() < 0) {
            e.rejectValue("age", "negativevalue");
        } else if (p.getAge() > 110) {
            e.rejectValue("age", "too.darn.old");
        }    
    }
}
```