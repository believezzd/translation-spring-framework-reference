### 7.11 Using JSR 330 Standard Annotation

从spring3.0开始，spring提供了对JSR330标准注解的支持（依赖注入）。这些注解将会以相同的方式被spring扫描。你只需要在你的类中有相关的类就可以。

>**Note**

>如果你使用Maven，那么以javax.inject作为artifact在标准的Mavan仓库中是可用的（http://repo1.maven.org/maven2/javax/inject/javax.inject/1/）。你可以将下面的依赖加入你的pom.xml文件中。

>```
><dependency>
>    <groupId>javax.inject</groupId>
>    <artifactId>javax.inject</artifactId>
>    <version>1</version>
></dependency>
>```