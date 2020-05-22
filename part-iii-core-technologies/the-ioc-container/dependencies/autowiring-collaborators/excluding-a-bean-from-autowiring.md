##### Excluding a bean from autowiring

在每个bean中，你可以将他从自动装配中排除。在spring的xml形式中，在bean元素中指定autowire-candidate属性为false，容器会认为这是一个特殊的bean不能在自动装配被使用（包括注解形式的配置，例如@Autowired）

>**Note**

>autowire-candidate属性被设计成只影响基于类型的自动装配。它不会影响通过名字的显式引用，会正确被处理即使属性被设置成false。因此基于名字的自动装配会注入成功如果匹配的话。

你可以通过模式匹配bean的名字来限制自动装配的候选者。在顶级的beans元素中接受一个或多个模式在default-autowire-candidates属性中。例如，通过限制自动装配候选者的状态对每个bean以Repository结尾的bean，提供一个值*Repository。如果需要指定多个值，则设定逗号分隔的list。对于明确指定autowire-candidate属性的值，模式匹配将不起作用。

如果你不想通过自动装配注入到其他bean中，这些功能将会很有用。这并不意味着被排除的bean不能通过自动装配来配置自身。这些bean在装配时不会作为其他bean的候选者。