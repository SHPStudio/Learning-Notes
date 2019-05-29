# DataSource自动配置
## Registrar
通过`DataSourceAutoConfiguration`上的@Import注解引入，并通过实现`ImportBeanDefinitionRegistrar`接口注入`DataSourceInitializerPostProcessor`
### DataSourceInitializerPostProcessor
 注入这个类的目的是在回调`postProcessAfterInitialization`这个方法时直接强制初始化`DataSourceInitializer`
### DataSourceInitializer
它可以读取`spring.datasource.schema`配置，在项目启动时创建表sql文件创建表和触发数据库初始化事件表示初始化表结构完成，
#### DatabasePopulatorUtils
使用该类通过`ResourceDatabasePopulator`和`DataSource`创建数据库链接并执行脚本。
