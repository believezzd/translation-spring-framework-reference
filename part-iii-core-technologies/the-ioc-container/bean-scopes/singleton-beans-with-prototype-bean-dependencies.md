#### Singletone bean with prototype-bean dependencies

当你的单例bean依赖原型bean时，要意识到是在初始化的时候被依赖注入的。如果你在单例bean中注入原型bean，一个新的原型bean实例会被初始化然后注入单例bean。原型bean只会提供一次实例给单例bean。

然而，假设你希望单例bean在每次运行时获得一个新的原型bean。你不能直接将原型bean注入单例bean，因为只会注入一次，整个注入会在spring容器初始化单例的bean的时候完成。如果你在每次运行时需要一个新的原型bean，见章节7.4.6，“Method injection”。

