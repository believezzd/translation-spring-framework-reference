#### Request, session, global session, application, and WebSocket scopes

request、session、global session、application和WebSocket范围只有在你在web中使用spring的ApplicationContext（例如XmlWebApplicationContext）中才可以。如果你在普通的spring的IOC容器中使用这些范围例如ClassPathXmlApplicationContext，那么IllegalStateException会被抛出由于识别不了bean的范围。

