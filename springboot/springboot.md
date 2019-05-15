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
12. 在打包jar包之外的