# DataSource自动配置
## Registrar
通过`DataSourceAutoConfiguration`上的@Import注解引入，并通过实现`ImportBeanDefinitionRegistrar`接口注入`DataSourceInitializerPostProcessor`
### DataSourceInitializerPostProcessor
 注入这个类的目的是在回调`postProcessAfterInitialization`这个方法时直接强制初始化`DataSourceInitializer`
### DataSourceInitializer
它可以读取`spring.datasource.schema`配置，在项目启动时创建表sql文件创建表和扫描data.sql等数据sql创建数据。并触发数据库初始化事件。
#### DatabasePopulatorUtils
使用该类通过`ResourceDatabasePopulator`和`DataSource`