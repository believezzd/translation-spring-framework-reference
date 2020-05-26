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

你看到了，ValidationUtils中的静态rejectIfEmpty方法用于拒绝当如果name属性是null或者是一个空字符串。可以查看ValidationUtils的javadocs来查看之前的例子是如何工作的。

可以单独实现一个简单的Validator类用于验证每个富object中的内置object，最好将对于内置object的验证逻辑压缩到一个自有的Validator实现中。一个富object的简单例子可以是由两个字符串属性组成的Customer（一个姓，一个名）和一个复杂的Address的object。Address的object可以独立于Customer的object，因此可以实现一个AddressValidator。如果你需要你的CustomerValidator来服用逻辑包括在AddressValidator不需要复制和粘贴，你可以独立注入或在CustomerValidator中使用AddressValidator，你可以使用如下：

```
public class CustomerValidator implements Validator {

    private final Validator addressValidator;
    
    public CustomerValidator(Validator addressValidator) {
        if (addressValidator == null) {
            throw new IllegalArgumentException("The supplied [Validator] is " + "required and must not be null.");
        }
        if (!addressValidator.supports(Address.class)) {
            throw new IllegalArgumentException("The supplied [Validator] must " + "support the validation of [Address] instances.");
        }
        this.addressValidator = addressValidator;
    }

    /**
    * This Validator validates Customer instances, and any subclasses of Customer too
    */
    public boolean supports(Class clazz) {
        return Customer.class.isAssignableFrom(clazz);
    }
    
    public void validate(Object target, Errors errors) {
        ValidationUtils.rejectIfEmptyOrWhitespace(errors, "firstName", "field.required");
        ValidationUtils.rejectIfEmptyOrWhitespace(errors, "surname", "field.required");
        Customer customer = (Customer) target;
        try {
            errors.pushNestedPath("address");
            ValidationUtils.invokeValidator(this.addressValidator, customer.getAddress(), errors);
        } finally {
            errors.popNestedPath();
        }
    }
}
```

Validation错误用于报告封装于验证器中的错误object。在spring的mvc中你可以使用<spring:bing/>标签来检查错误信息，但是你也可以自行注入错误object。更多有关方法的信息见给定的javadocs。



