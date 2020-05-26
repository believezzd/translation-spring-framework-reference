##### Other notes relating to wildcards

请注意classpath*:当和Ant风格的匹配一起使用时将可以工作至少一个根路径在模式开始之前，除非实际的目标文件在文件系统中。这意味着类似于classpath*:*.xml模式将不会从根的jar文件中读取而是从扩展根路径中读取。这起源于JDK的ClassLoader.getResources()方法的限制只能返回文件系统位置用于一个空的字符串（隐含需找的根路径）。

Spring检索类路径条目的能力源于JDKgetresources()方法，该方法只返回passedin的文件系统位置空字符串(指示要搜索的潜在根)。Spring计算URLClassLoader运行时配置和“java.class”。路径“在jar文件中也显示，但这并不能保证会导致便携式的行为。

>**Note**
>扫描类路径包需要存在相应的目录条目在类路径中。当您使用Ant构建jar时，请确保不只是激活文件切换JAR任务。此外，基于安全性，类路径目录可能不会公开在某些环境中的策略，例如JDK 1.7.0_45或更高版本上的独立应用程序(这需要在您的清单中设置“托管库”;参https://stackoverflow.com/questions/19394570/java-jre-7u45-breaks-classloader-getresources)。

Ant风格的模式和classpath:资源融合不能简单匹配资源如果用于查找的根包在多个类路径中可用。这是因为资源是如下：

```
com/mycompany/package1/service-context.xml
```

可能只在一个位置，但当路径如

```
classpath:com/mycompany/**/service-context.xml
```

是需要处理的，处理器将会工作返回URL通过getResource("com/mycompany")。如果这个基本的包节点在多个类加载路径中存在，实际的资源不会在底层。因此，最好使用" `classpath*:`"和Ant风格模式在这样的例子中，将会查找所有的类路径包含根包。
