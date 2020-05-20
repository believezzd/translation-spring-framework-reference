### 3.3 Java 8(as well as 6 and 7)

Spring框架4.0支持Java8的特性。你可以通过Spring的callback接口使用lambda表达式和method references。支持java.time和一些被增强的注释如@Repeatable。你也可以使用Java8的parameter name discovery（基于 -parameters编译标识）作为可以携带调试信息的选择来编译你的代码。

Spring兼容了以前的java和jdk版本。具体是（特别声明，最低的支持是JDK6 update 18，在2010年一月发布）和之后发布的版本。然而，如果开发一个新的spring4应用，建议使用java7或java8。

>**Note**

> 到 2017 年，JDK6 会退出历史舞台，因此 Spring 对 JDK 6 的支持也会在那时候结束。Oracle 和 IBM 会在 2018 年结束对 JDK6 的商业支持。虽然 Spring 会保持 4.3.x 版本对 JDK6 的兼容性，但是对已经升级到 JDK7 以及更高版本的支持会有一个不同点：jdk6 发现的 bug 或者问题之后在 jdk7 的版本上进行修复。