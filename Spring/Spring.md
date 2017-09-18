# Spring
## 模块介绍
1. `Data Access/Integration` 数据访问模块，它包括`JDBC、ORM`等跟数据库访问等等有关的。
2. `Web` web模块，比如`WebSocket`、`Servlet`等等有关网络访问相关的。
3. `AOP` 切面编程模块，比如拦截器或者需要对某些操作进行切面处理，比如注解日志，注解监控等等。
4. `Aspects` 切面编程模块
5. `Instrumentataion`模块
6. `Messaging`模块
7. `Core Container` 这个模块是Spring基础模块比如Beans和Core、Context之类的。
8. `Test`

## Core Container
  这个是spring的基础模块，包括`spring-core、spring-beans、spring-context、spring-context-support、spring-expression`
  `spring-core和spring-beans`提供框架的基本功能，包括依赖注入等特性。

  `spring-context`是继承了beans功能的上下文，他可以通过一种类似`JNDI`注册表的方式去获取对象,也就是类似配置文件，并且他还有资源绑定、加载，事件传播等等。
  他还支持J2EE的一些特性EJB、JMX和一些基础的远程操作。`ApplicationContext`这个接口对于Conext模块来说是很重要的.

  `spring-context-support`这个可以提供一些第三方的库到spring应用中，比如Guava等缓存，JavaMail，模板引擎Velocity等。

  `spring-expression`提供了一些像jsp符号语言的强大表达式等功能。
