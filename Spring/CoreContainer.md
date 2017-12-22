# 控制反转依赖注入
  一般的对象创建的时候所依赖的其他对象或变量，大多都是通过构造函数的参数，工厂方法参数或者实例中的变量在对象构造之后进行注入的。
  然而通过控制反转，在建造bean的时候就把这个bean的实例所依赖的东西都注入进来，所以这个过程是相反的。实例的依赖项的控制完全通过
  构造完的classes或者通过位置服务模式直接获得。
## 依赖包
  `org.springframework.beans and org.springframework.context`这两个包是控制反转的基础。

  `BeanFactory`这个接口通过一个高级的配置机制可以管理任何类型的对象。

  `ApplicationContext`是BeanFactory的子接口，他提供简单的aop特性，消息资源的处理，事件发布，应用层特定的上下文
  比如`WebApplicationContext`,用在我们的web应用程序上。
## 依赖关系源于配置
  所有Beans的关系都是通过元数据的配置来反映的。这个元数据的配置的代表就是xml格式的文件。或者java的注解再就是用java的代码去配置。
## Bean
### name与id
  每个bean都会有一个唯一的标识也就是id，如果想要有很多bean的标识可以使用name属性，他相当于bean的别名，通过别名可能访问
  一组的bean。他是用`,`或者`;`来进行分割。

  **如果Bean没有id和name的话，容器会自动生成一个 按照的规则是根据类名开头字母变为小写**

## 别名
   `<alias name="fromName" alias="toName"/>` 只在bean上定义别名可能是不够的，对于大型系统来说，他可能会有很多的子系统，子系统也有自己定义的一些
   bean，为了让别名指向能够统一，可以使用alias，比如，把fromName指向toName
## 内部类
   如果定义的bean的class是内部类的话，需要用`$`符号指定 `com.example.Foo$Bar`
## 工厂类
   如果想通过一个静态的工厂方法去返回bean的类型，那么需要`factory-method`这个属性
   `<bean id="clientService" class="examples.ClientService" factory-method="createInstance"/>`

        public class ClientService {
            private static ClientService clientService = new ClientService();
            private ClientService() {}

            public static ClientService createInstance() {
                return clientService;
            }
        }

   比如你有一个工厂类，他可以建造很多的类型的类，那么就需要配置`factory-bean`，factory-bean指定相应的工厂类的bean，然后可以通过指定
   不同的factory-method去定义返回不同类型的对象的bean

        <bean id="serviceLocator" class="examples.DefaultServiceLocator">
            <!-- inject any dependencies required by this locator bean -->
        </bean>

        <bean id="clientService"
            factory-bean="serviceLocator"
            factory-method="createClientServiceInstance"/>

        <bean id="accountService"
            factory-bean="serviceLocator"
            factory-method="createAccountServiceInstance"/>

        public class DefaultServiceLocator {

            private static ClientService clientService = new ClientServiceImpl();
            private static AccountService accountService = new AccountServiceImpl();

            private DefaultServiceLocator() {}

            public ClientService createClientServiceInstance() {
                return clientService;
            }

            public AccountService createAccountServiceInstance() {
                return accountService;
            }

        }

## 构造器注入与setter注入

   构造器注入主要是用于那么强制性依赖，setter注入主要是可选依赖。

   强制性依赖就是使依赖不可变并且不可为空。

   如果用setter注入应该在用的地方都要做非空的判断。
1. 循环依赖问题

    如果classA在构造器依赖classB，classB依赖classA，在Spring配置文件中进行配置的时候，
    在运行时Spring会检测出存在循环依赖问题并抛出`BeanCurrentlyInCreationException`异常。
    解决办法就是替换为setter方式注入。

## idref
   这个标签可以让spring在发布的时候去验证用这个标签标识的bean是否真的存在，有没有把名字写错

        <bean id="theTargetBean" class="..."/>

        <bean id="theClientBean" class="...">
            <property name="targetName">
                <idref bean="theTargetBean"/>
            </property>
        </bean>

## ref
  1. `<ref bean="someBean"/>` 这个ref可以做为property标签或者构造器标签的原子型子标签
  bean这个属性所指向的bean对象的引用可以是任何容器的，在本容器的也就是本xml文件的，或者是父容器的
  2. ref 还可以只指向父容器中的bean对象

            <!-- in the parent context -->
            <bean id="accountService" class="com.foo.SimpleAccountService">
                <!-- insert dependencies as required as here -->
            </bean>
            <!-- in the child (descendant) context -->
            <bean id="accountService" <!-- bean name is the same as the parent bean -->
                class="org.springframework.aop.framework.ProxyFactoryBean">
                <property name="target">
                    <ref parent="accountService"/> <!-- notice how we refer to the parent bean -->
                </property>
                <!-- insert other configuration and dependencies as required here -->
            </bean>

## collection merge
  我们可以实现定义一个类似父类模板那种的集合，然后可以在子bean中去继承父bean中的集合，并可以对集合中的数据进行重写或者添加

        <beans>
            <bean id="parent" abstract="true" class="example.ComplexObject">
                <property name="adminEmails">
                    <props>
                        <prop key="administrator">administrator@example.com</prop>
                        <prop key="support">support@example.com</prop>
                    </props>
                </property>
            </bean>
            <bean id="child" parent="parent">
                <property name="adminEmails">
                    <!-- the merge is specified on the child collection definition -->
                    <props merge="true">
                        <prop key="sales">sales@example.com</prop>
                        <prop key="support">support@example.co.uk</prop>
                    </props>
                </property>
            </bean>
        <beans>

  需要注意的是list类型因为是有序的，所以父集合的值在子集合的前面。但是对于property或者set等等的是无序的所以不存在有序这么个语义。

设置为null

        <bean class="ExampleBean">
            <property name="email">
                <null/>
            </property>
        </bean>

## lazy-initialized
   因为spring的ApplicationContext创建和配置所有的bean的时候默认bean的生命周期是单例的。
   这种是立即实例化初始化一次，然后等请求使用的时候去使用早就初始化实例化好的。
   如果在bean上加上lazy-initialized="true"，代表使用的时候才会去初始化



