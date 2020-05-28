#### Setting and getting basic and nested properties

设置和获得属性由setPropertyValue和getPropertyValue方法来实现，有几个重载的方法。详细描述在javadocs中。重要的知道有一些方法用于声明object的属性。例子：

*Table 9.1 Examples of properties*

Expression | Explanation
-- | --
name | Indicates the property name corresponding to the methods getName() or isName() and setName(..)
account.name | Indicates the nested property name of the property account corresponding e.g. to the methods getAccount().setName() or getAccount().getName()
account[2] | Indicates the third element of the indexed property account. Indexed properties can be of type array, list or other naturally ordered collection
account[COMPANYNAME] | Indicates the value of the map entry indexed by the key COMPANYNAME of the Map property account

下面你会发现一些使用BeanWrapper来获得或设置属性的例子

（下面的内容不是很重要如果在你并不想直接使用BeanWrapper的情况下。如果你只是使用DataBinder和BeanFactory和他们的实现，你可以跳过有关PropertyEditors的内容）

考虑下面两个类：

```
public class Company {
    private String name;
    private Employee managingDirector;
    public String getName() {
        return this.name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public Employee getManagingDirector() {
    return this.managingDirector;
    }
    public void setManagingDirector(Employee managingDirector) {
        this.managingDirector = managingDirector;
    }
}
```

```
public class Employee {
    private String name;
    private float salary;
    public String getName() {
        return this.name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public float getSalary() {
        return salary;
    }
    public void setSalary(float salary) {
        this.salary = salary;
    }
}
```

下面的代码片段展示了一些例子是如果操纵Company和Employee实例的属性值。

```
BeanWrapper company = new BeanWrapperImpl(new Company());

// setting the company name..
company.setPropertyValue("name", "Some Company Inc.");

// ... can also be done like this:
PropertyValue value = new PropertyValue("name", "Some Company Inc.");
company.setPropertyValue(value);

// ok, let's create the director and tie it to the company:
BeanWrapper jim = new BeanWrapperImpl(new Employee());
jim.setPropertyValue("name", "Jim Stravinsky");
company.setPropertyValue("managingDirector", jim.getWrappedInstance());

// retrieving the salary of the managingDirector through the company
Float salary = (Float) company.getPropertyValue("managingDirector.salary");
```

