### 7.5 Bean scopes

当你创建bean definition时，你通过bean definition指定了创建类的实例的的过程。 将bean definition作为一个过程来考虑是重要的，因为这就意味着，通过一个过程使得为一个类创建多个对象。

你不只可以控制依赖和配置值用于插件式配置到一个由bean的定义构造出来的对象中，也可以通过bean的定义指定对象的范围。这种方法有力且快捷的设置了object的范围而不再需要你通过Java类的级别来处理。你可以将bean设置为其中一个范围，spring框架支持7种范围，其中的5种你只能在web应用中使用。

下面是范围的列表。你也可以自定义范围。

_Table 7.3. Bean scopes_

| Scope | Description |
| :---: | :---: |
| singleton | 默认选项， 在每个spring的ioc容器中只保留一个实例 |
| prototype | 对于一个bean有多个实例 |
| request | 对于每个bean在一个http请求的生命周期中保持一个实例。 意味着每个http请求都会有他自己的实例。 只在spring的web应用上下文中有效。 |
| session | 对于每个bean在一个http session的生命周期有效。 只在spring的web应用上下文中有效。 |
| globalSession | 对于每个bean在一个全局的http session的生命周期有效。 通常在使用Proletcontext时有效。 只在spring的web应用上下文有效。 |
| application | 对于每个bean在一个ServletContext的生命周期有效。 只在spring的web应用上下文有效。 |
| websocket | 对于每个bean在一个WebSocket的生命周期有效。 只在spring的web应用上下文有效。 |

>**Note**

>在spring3.0中，线程范围是存在的，但是并没有默认被注册。相关信息，见SimpleThreadScope的文档。了解更多有关如何注册或其他自定义范围，见”Using a custom scope“章节。


