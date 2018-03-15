# Profile
  比如我们的产品在开发环境、测试环境还有线上环境，可能我们的一些配置都不一样。
  比如在可开发环境和测试环境用的测试数据库可能就是测试环境的，预发跟线上的环境都是生产环境的数据库。

  那么我们就可以使用Profile去切换我们程序中的配置。

## 使用
1. 在程序中使用注解或者xml方式给特定的beanConfig或者bean进行标记

    1. 使用javaconfig方式
    ```
    @Profile("dev") // 比如用dev或者prod
    ```
    2. 使用xml方式
    ```
    <bean profile="dev">
    ```
2. 激活profile
  我们配置好了针对不同环境的配置项，那么我们需要激活特定的环境。

    跟激活profile有关的是两个属性 一个是spring.profiles.active和spring.profiles.default

    如果没有active就找default 如果都没有的话就只会创建哪些没有定义在profile中的bean

    有很多种方式去设置这两个属性

    1. 作为DispathcerServlet的初始化参数
    2. 作为Web应用的上下文参数
    3. 作为JNDI条目
    4. 作为环境变量
    5. 作为JVM的系统属性
    6. 在集成测试类上，使用@ActiveProfiles