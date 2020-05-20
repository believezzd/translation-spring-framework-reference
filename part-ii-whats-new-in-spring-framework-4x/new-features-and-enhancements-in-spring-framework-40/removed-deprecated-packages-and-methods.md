### Removed Deprecated Packages and Methods

所有过时的包和许多过时的类和方法在4.0版本中已经去掉了。如果你从以前的版本升级到4.0，你应该保证你修复了每一个对过时方法的调用。

关于详尽的修改，请参考API Differences Report。

通知对第三方依赖的更新最早已经到了2010/2011（也就是spring4通常从2010至今的版本）如Hibernate3.6以上、EhCache2.1以上、Quartz1.8以上、Groovy1.8以上、Joda-Time2.0以上。例外，spring4对Hibernate Validator的要求是4.3以上。并且对Jackson的要求是2.0以上（对Jackson1.8或1.9的支持，spring3.2中包含，现在已经被提升了）。