##### Limitations and disadvantages of autowiring

自动注入在跨项目时更加适合。如果不使用自动注入，他可能会在一两个bean中困扰开发者。

考虑自动注入的限制和缺点：

* 对属性和构造参数的显式依赖总是覆盖自动装配。你无法通过自动装配来装配基本类型、字符串和类（简单属性的数组）。这是设计时就有的限制。

* 自动装配没有明确指定那样精确。尽管，在上面的表格中说明，spring已经仔细考虑了不可预料的歧义结果，但是Spring管理的对象关系不再那么明确。

* 自动装配的信息将不会暴露给从spring的容器中生成文档的工具。

* 容器中可能会有多个bean匹配set方法或构造器参数可以被自动装配。对于数组、集合或者map，这不在是一个问题。然而依赖明确的一个value，歧义并没有被简单的解决。如果没有唯一的bean匹配，一个异常会被抛出。

在后面的例子中，你有如下的几个选择：

* 使用明确指定而不再使用自动装配。

* 避免在bean定义中使用自动装配，通过使用autowire-candidate属性并将其属性值设置为false，如下节所描述的那样。

* 设置<bean/>的primary属性，指定将一个bean设置为primary candidate.

* 通过基于注解的配置实现更加细粒度的控制，as described in Section 7.9,“Annotation-based container configuration”

* 





