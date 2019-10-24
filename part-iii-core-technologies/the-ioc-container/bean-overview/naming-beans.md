每个bean都有一个或多个标识符。这些标识符必须是唯一的在容器中所包含的bean中。一个bean通常只有一个标识符，但是如果需要多个标识符的话，其他的可以考虑使用别名。

基于xml配置的元数据，你可以使用id或name属性来定义一个bean。id属性允许你指定恰当的id。通常他们的名字应该是英文（'myBean', 'fooService'等等），或许也会包含特殊的字符。如果你想给你的bean起一个别名，你可以在name属性中定义，用逗号或分号或者空格隔开。在spring3.1之前，id属性是作为一个xsd:ID类型的，并且包含合适的字符。在3.1版本中，他是一个xsd:string的类型。容器强制要求id唯一性，尽管不会被xml解析。

并不一定要求你为bean起一个id或name。如果一个bean没有id或name，容器会为id生成一个唯一的name。然而，如果你想通过名字来引用bean，或者通过ref标签来引用bean或者使用Service Locator style lookup，你必须为bean提供一个name。

>命名规范
>规范和标准java对实例属性的命名规范相同。那就是，bean的名字由小写字母开头，然后采用驼峰式。例如这样的名字'accountManager'、'accountService'、'userDao'、'loginController'等等。
>bean命名的一致性使得你的配置更容易阅读和理解，如果你在使用spring的aop，这会对你有益，当你应用advice对一系列的bean通过name进行切入时候。

>Note
>当在classpath下进行组件扫描时，spring会为每个未命名的组件生成名字，规则是：将类名的首字母小写。然而，有一些特殊情况，比如第一个和第二个字符都是大写，则保留原有的名字。这和java.beans.Introspector.decapitalize的规则是一样的（spring在这里使用）。