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
## AOP
   和AOP相关的有`spring-aop`还有`spring-aspects`
   `spring-aop`是spring对aop的基本支持,`spring-aspects`是spring整合aspectJ框架的模块。
## web模块
1. `spring-web`，这个模块是springweb的基础模块，他提供比如多块文件上传和使用servletlistener去初始化依赖注入容器，还有是一个面向web程序的上下文。
他还包括一个Http的客户端和对网络相关远程操作的支持。
2. `spring-webmvc`，这个模块提供了springmvc有关的功能和restfulwebservice的功能。他还把我们的数据对象domain
等与视图层表单之类的进行解耦合并且还集成了很多spring的其他功能。
## Test
  `spring-test` 模块提供了使用JUnit等测试框架去进行单元和集成测试，他提供了加载applicationContext上下文并缓存这些数据的功能，并且他还能提供mock模拟数据来对测试你的代码进行隔离。

## Logging
   Spring的log是使用了JCL通用Log Api的手段，这个功能是集成在spring-core模块中，他可以在运行时动态的去选择一些log框架比如log4j等等，甚至是java自带的。

### 可以用`slf4j`代理log4j或者其他日志框架 那么就需要log4j和slf4j的架包

        <dependencies>
          <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-slf4j-impl</artifactId>
            <version>2.6.2</version>
          </dependency>
        </dependencies>

### 屏蔽`common-logging`
   这个通用logapi可能会导致某些问题，所以有可能会屏蔽spring-core中的common-logging模块

        <dependencies>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-core</artifactId>
                <version>4.3.11.RELEASE</version>
                <exclusions>
                    <exclusion>
                        <groupId>commons-logging</groupId>
                        <artifactId>commons-logging</artifactId>
                    </exclusion>
                </exclusions>
            </dependency>
        </dependencies>

  但是把自带的通用logapi屏蔽之后那么应用将被认定为损坏，所以我们应该做一种替代的方案。就是用slf4j与log4j或者logback结合来代替通用logapi。
  所以我们使用slf4j来代替commons-logging的时候需要引入几个依赖
  1. jcl-slf4j 使用slf4j来代替commons-logging
  2. slf4j-log4j/logback slf4j代理其他日志框架
  3. log4j 日志架包

  不过我们通常使用slf4j与logback的结合，因为logback直接实现了slf4j。

        <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-core</artifactId>
                <version>4.3.11.RELEASE</version>
                <exclusions>
                    <exclusion>
                        <groupId>commons-logging</groupId>
                        <artifactId>commons-logging</artifactId>
                    </exclusion>
                </exclusions>
            </dependency>
            <dependency>
                    <groupId>org.slf4j</groupId>
                    <artifactId>jcl-over-slf4j</artifactId>
                    <version>1.7.21</version>
                </dependency>
                <dependency>
                    <groupId>ch.qos.logback</groupId>
                    <artifactId>logback-classic</artifactId>
                    <version>1.1.7</version>
                </dependency>

