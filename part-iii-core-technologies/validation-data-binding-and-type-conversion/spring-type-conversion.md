### 9.5 Spring Type Conversion

spirng3引入了core.convert包用于提供通用类型的转换系统。系统定义了SPI来实现类型转换逻辑，和API在运行时执行类型转换一样。在spring的容器中，这个系统可以用于PropertyEditors来转化属性字符串到需要的属性类型。公共API也可以在你应用中使用当需要类型转换时。