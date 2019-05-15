1. devtools全局配置 在用户home文件夹下`~/.spring-boot-devtools.properties`
2. 测试时使用的`@TestPropertySource`注解
3. 测试时注入的属性 使用`@SpringBootTest`注解时和[test annotations for testing a particular slice of your application.](https://docs.spring.io/spring-boot/docs/2.1.0.RELEASE/reference/htmlsingle/#boot-features-testing-spring-boot-applications-testing-autoconfigured-tests)
4. 通过命令行传递的参数
5. 通过属性`SPRING_APPLICATION_JSON`传递来的json形式的配置
6. `ServletConfig`