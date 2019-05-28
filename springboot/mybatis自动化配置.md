# DataSource自动配置
## Registrar
通过`DataSourceAutoConfiguration`上的@Import注解引入，并通过实现`ImportBeanDefinitionRegistrar`接口注入`DataSourceInitializerPostProcessor`
### DataSourceInitializerPostProcessor
 注入这个类的目的是在回调`postProcessAfterInitialization`这个方法时直接强制初始化``