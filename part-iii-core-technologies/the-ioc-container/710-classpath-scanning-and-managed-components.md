### 7.10 ClassPath scanning and managed components

大部分的例子使用xml来定义配置元数据使得spring的容器可以处理。前面的章节（章节7.9“基于注解的容器配置”）描述了如何通过使用一系列在源代码级别使用注解来描述。即便是在这些例子中，基本的bean定义也是明确的用xml来定义，注解智能驾驭依赖注入。在这节将介绍通过扫描类路径来实现对备选组件的探测。候选组件就是类，这些类满足一定的条件，并且和容器中定义的bean的依赖有关。这样就避免了使用xml来配置bean的定义，作为代替，你可以使用注解来定义（例如@Component），AspectJ类型表达式或你自定义的匹配规则来选择容器中bean的依赖。

>**Note**

>从spring3.0开始，许多spring JavaConfig项目提供的组件成为了spring框架核心的一部分。这允许你铜鼓java来定义bean而不是使用传统的xml文件。可以看一下@Configuration、@Bean、@Import和@DependsOn注解来了解如何使用这些新特性。