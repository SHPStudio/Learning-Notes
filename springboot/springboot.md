# propertie加载顺序
1. devtools全局配置 在用户home文件夹下`~/.spring-boot-devtools.properties`
2. 测试时使用的`@TestPropertySource`注解
3. 测试时注入的属性 使用`@SpringBootTest`注解时和[test annotations for testing a particular slice of your application.](https://docs.spring.io/spring-boot/docs/2.1.0.RELEASE/reference/htmlsingle/#boot-features-testing-spring-boot-applications-testing-autoconfigured-tests)
4. 通过命令行传递的参数
5. 通过属性`SPRING_APPLICATION_JSON`传递来的json形式的配置
6. `ServletConfig`初始化参数
7. `ServletContext`初始化参数
8. 来自`java:comp/env`的JNDI属性
9. java系统属性`System.getProperties()`
10. 系统环境变量
11. `rondom.*`属性，来自`RandomValuePropertySource`
12. 在打包jar包之外的`profile-specific`特定profile的应用配置文件 `application-{profile}.properties`
13. jar包内的profile应用配置文件
14. 打包文件外部的application.properties
15. jar包内的application.properties
16. 在配置类上的`@PropertySource`注解
17. 通过`SpringApplication.setDefaultProperties`设置的默认属性

# 修改配置文件路径
可以通过`spring.config.location`属性去配置要加载的配置文件路径，注意他是反序查询的，多个路径都逗号分隔
因为使用上述的属性会覆盖掉默认的配置文件查找路径，所以如果想要在之前的基础上增加路径可以使用`spring.config.additional-location`属性

# 加密属性处理
如果想对加密的属性值解密再放入spring环境中可以用`EnvironmentPostProcessor`接口允许在Spring正式启动前修改属性。参考[Section 76.3, “Customize the Environment or ApplicationContext Before It Starts” ](https://docs.spring.io/spring-boot/docs/2.1.0.RELEASE/reference/htmlsingle/#howto-customize-the-environment-or-application-context)
如果想以更安全的方式存储证书和密码的话，可以参考` Spring Cloud Vault `项目，它提供外部的一个安全外部配置的存储。



