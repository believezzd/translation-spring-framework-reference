Java EE 6作为基准的spring4的考虑范围，和JPA2.0、Servlet3.0规范是相关的。为了保留Google App Engine和以前应用服务器的版本，可以将spring4的应用部署在Servlet2.5环境中。然而，强烈推荐Servlet3.0在开发环境对spring的测试和模拟。

>**Note**

>如果你是WebSphere7的用户，确认安装了JPA2.0的特性包。如果是WebLogic10.3.4或以上的版本，内部是安装了JPA2.0的。这使得这些服务器是适合spring4的开发环境。

在更早的一份注意声明中有提到过，spring框架4.0现在支持Java EE 7的规范，特别的，包括JMS2.0、JTA1.2、JPA2.1、Bean Validation 1.1和JSP-236并发包。和通常一样，这些特性支持单独使用，例如，在Tomcat或一些独立的环境。然而，但是部署在Java 7的服务器上和spring在一起会更好。

注意，Hibernate4.3支持JPA2.1并且只支持spring4.0以上的版本。和Hibernate Validator5.0、Bean Validation1.1一样。他们在官方说明中都不支持spring3.2。