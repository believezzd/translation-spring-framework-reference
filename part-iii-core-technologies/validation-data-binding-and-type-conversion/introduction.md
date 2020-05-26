### 9.1 Introduction

>JSR-303/JSR-349 Bean Validation

>spring框架4支持bean验证1.0（JSR303）和bean验证1.1（JSR349），同时适用于spring的验证接口。

>应用可以选择是否开启全局的bean验证，在9.8节中描述的“spring验证”，将其用于所有验证的需要。

>一个应用可以注册额外的spring验证实例对于每个数据绑定实例，在9.8.3节中描述的“配置一个数据绑定”。这对不使用注解而实现验证逻辑的插件化处理很有用。

将验证作为业务逻辑，spring提供一个设计用于验证（和数据绑定）包括全部。明确的验证不应该和web层绑定，应该易于局部化并且可以在需要验证时以插件的方式使用。考虑到上面的几点，spring提供了一个验证器接口可以用于一个应用的每一层。

数据绑定是有用的允许读者动态输入绑定应用的对象模型（或你需要处理用户输入的object）。spring提供了这样的DataBinder用于处理这些。验证器和数据绑定器组成了验证的包，主要用于但是不限制于MVC的框架中。

BeanWrapper是spring框架的基本内容用在很多的位置。然而，你不需要直接使用BeanWrapper。因为这是一个参考文档，然而我们会提供一些解释。我们将会这一节解释BeanWrapper，如果你决定使用它，当你试图将数据绑定到object时会用到。

spring的DataBingder和低级的BeanWrapper都使用了PropertyEditors来解析和格式化属性值。PropertyEditor是JavaBean定义的一部分，并且在这节中得到解释。spring3介绍了一个core.conver包用于提供通用的类型转换方法，在formate包中由于格式化UI属性值。这些新的包将可以简单的使用PropertyEditors来替代，并且将会这节中讲解。




