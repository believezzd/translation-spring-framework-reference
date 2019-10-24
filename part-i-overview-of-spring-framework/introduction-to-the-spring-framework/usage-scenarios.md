### 2.3 Usage scenarios

building blocks描述了之前在很多场景中选择spring，从资源贫乏的嵌入式应用到使用spring的事务管理和web框架集成的成熟的企业级应用。

![Figure 2.2 Typical full-fledged Spring web application](/graph/Figure 2.2. Typical full-fledged Spring web application.png)
*Figure 2.2 Typical full-fledged Spring web application*

spring的声明式事务管理使得web应用可以fully transactional，就像你使用EJB容易管理事务一样。所有的用户需求都可以通过IOC容器管理的简单的pojo来实现。额外的支持包括发送email、通过你选择的检查规范来检查web层的合法性。Spring ORM使得你可以与JPA、hibernate和JDO集成。例如使用hibernate，你依然可以使用现有的映射文件和标准的hibernate SessionFactory配置。Form控制器无缝的与领域模型相互结合，不需要ActionForms和其他用来和领域模型传输http参数的类。

![Figure 2.3. Spring middle-tier using a third-party web framework](/graph/Figure 2.3. Spring middle-tier using a third-party web framework.png)
*Figure 2.3. Spring middle-tier using a third-party web framework*

在有些情形下不允许你全部切换到另一个框架。spring框架并不强制要求你使用框架的每一个部分，spring框架不是一个all-or-nothing解决方案。已有的前端方案如Struts、Tapestry、JSF或其他UI框架可以和spring的中间层进行集成以便于你使用spring的事务特性。你只需要简单的使用ApplicationContext将你的业务包裹起来，再使用WebApplicationContext来集成你的web层。

![Figure 2.4. Remoting usage scenario](/graph/Figure 2.4. Remoting usage scenario.png)
*Figure 2.4. Remoting usage scenario*

当你需要通过web service来访问你的方法，你可以使用spring的Hessian-、Burlap-、Rmi-或者JaxRpcProxyFactory。使得远程方法调用很简单。

![Figure 2.5. EJBs - Wrapping existing POJOs](/graph/Figure 2.5. EJBs - Wrapping existing POJOs.png)
*Figure 2.5. EJBs*

spring框架也为企业JavaBeans提供了访问和抽象层，使得你可以重用已有的POJO并且将他们通过session beans包裹起来，以方便用于需要声明安全的可扩展、保证安全的web应用中。
