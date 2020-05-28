#### Build-in PropertyEditor impl

spring使用PropertyEditors来影响object和字符串的转换。如果你这样考虑，有时会比较简单以不同的方式代表属性而不是object本身。例如，一个日期可以以易于读的方式来展示（例如字符串'2007-14-09'），当我们试图从原始日期类型转换为易于读取的形式（或者将任意日期转化为易于阅读的形式）。这种行为可以通过注册自定义的编辑器，类型是java.beans.PropertyEditor来实现。注册自定义编辑器在BeanWrapper上火替代之前章节中提到的特定的IOC容器，用于实现属性和目标类型的转换。详见PropertyEditors的javadocs。

spring中的一些例子中使用了属性编辑

* 使用PropertyEditors来设置bean的属性。如果你在xml文件中指定某个bean的属性是java.lang.String类型，spring会（如果相应属性的set方法有一个Class-parameter）使用ClassEditor来试图处理一个Class object的参数。

* 解析spring的mvc框架中的http请求参数是通过使用各种的PropertyEditors来实现的，你可以手动绑定所有CommandController的子类。

spring有一些内置的PropertyEditors来简化处理。每个PropertyEditors都列在下面的表中都在org.springframework.beans.propertyeditors包中。大部分不是全部（下面提到的）都是通过BeanWrapperImpl默认被注册的。当属性编辑器被设置后，你依然可以注册自定义的编辑器来覆盖默认值：

*Table 9.2 Build-in PropertyEditors

Class | Explanation
ByteArrayPropertyEditor | Editor for byte arrays. Strings will simply be converted to their corresponding byte representations. Registered by default by BeanWrapperImpl.
ClassEditor | Parses Strings representing classes to actual classes and the other way around. When a class is not found, an IllegalArgumentException is thrown. Registered by default by BeanWrapperImpl.
CustomBooleanEditor | Customizable property editor for Boolean properties. Registered by default by BeanWrapperImpl, but, can be overridden by registering custom instance of it as custom editor.
CustomCollectionEditor | Property editor for Collections, converting any source Collection to a given target Collection type.
CustomDateEditor | Customizable property editor for java.util.Date, supporting a custom DateFormat. NOT registered by default. Must be user registered as needed with appropriate format.
CustomNumberEditor | Customizable property editor for any Number subclass like Integer, Long, Float, Double. Registered by default by BeanWrapperImpl, but can be overridden by registering custom instance of it as a custom editor.
FileEditor | Capable of resolving Strings to java.io.File objects. Registered by default by BeanWrapperImpl.
InputStreamEditor | One-way property editor, capable of taking a text string and producing (via an intermediate ResourceEditor and Resource) an InputStream, so InputStream properties may be directly set as Strings. Note that the default usage will not close the InputStream for you! Registered by default by BeanWrapperImpl
LocaleEditor | Capable of resolving Strings to Locale objects and vice versa (the String format is [country][variant], which is the same thing the toString() method of Locale provides). Registered by default by BeanWrapperImpl.
PatternEditor | Capable of resolving Strings to java.util.regex.Pattern objects and vice versa.
PropertiesEditor | Capable of converting Strings (formatted using the format as defined in the javadocs of the java.util.Properties class) to Properties objects. Registered by default by BeanWrapperImpl.
StringTrimmerEditor | Property editor that trims Strings. Optionally allows transforming an empty string into a null value. NOT registered by default; must be user registered as needed.
URLEditor | Capable of resolving a String representation of a URL to an actual URL object. Registered by default by BeanWrapperImpl.

spring使用java.beans.PropertyEditorManger来设置搜索路径用于属性编辑器根据需要。搜索路径也包括sun.bean.editors，包括PropertyEditor的实现为了其他类型，如Font、Color和大部分原始类型。注意编著的JavaBean架构可以自动发现PropertyEditor类（不需要你来手动注册）如果在相同的包中作为类来处理，或有相同的类名，然后以Editor结尾，例如，可以有如下的类和包结构将足够对于FooEditor类来识别用于Foo类型属性的属性编辑。

```
com
    chank
        pop
            Foo
            FooEditor // the PropertyEditor for the Foo class
```

注意你也可以使用标准的BeanInfo JavaBean的策略（。。。）。见下面这个使用BeanInfo策略的例子用于注册一个或多个属性编辑器实例相对于相关类。

```
com
    chank
        pop
            Foo
            FooBeanInfo // the BeanInfo for the Foo class
```

这是java源代码参考FooBeanInfo类。下面的例子会将CustomNumberEditor和Foo类的age属性联系在一起。

```
public class FooBeanInfo extends SimpleBeanInfo {
    public PropertyDescriptor[] getPropertyDescriptors() {
        try {
            final PropertyEditor numberPE = new CustomNumberEditor(Integer.class, true);
            PropertyDescriptor ageDescriptor = new PropertyDescriptor("age", Foo.class) {
            public PropertyEditor createPropertyEditor(Object bean) {
                return numberPE;
            };
        };
            return new PropertyDescriptor[] { ageDescriptor };
        }
        catch (IntrospectionException ex) {
            throw new Error(ex.toString());
        }
    }
}
```


