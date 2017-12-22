# Bean Scope
## bean对象的生命周期
  bean对象默认是单例的，也就是在容器初始化的时候，就已经创建好了，并且在
  以后的使用中，一直使用的同一个对象。 spring一共有7中生命周期，并且有5种是可通过web的applicationcontext获得。
1. singleton 单例模式 默认的 每个spring容器只有一个
2. prototype 原型模式 每一个对象有一个
3. request 跟一次http的生命周期挂钩 并且只能用于web感知的applicationcontext中
4. session 一个session一个
5. globalSession 只能用于Portlet
6. application
7. websocket

3-7 必须要是用web-aware的applicationcontext 比如XmlWebApplicationContext,如果使用常用的容器比如ClassPathXmlApplicationContext，那么就会报错IllegalStateException

  对于spring3.0一种基于线程的scope也是可以获得的，不过他默认不注册。

## 跨生命周期依赖
  比如在单例bean中依赖一个生命周期较短的http的bean，那么其实在每次调用httpbean中的方法时，只是最开始创建的bean的实例
  因为单例只在容器初始化时去处理http的bean。

  解决方案可以对http的bean进行代理处理，每次调用会生成一个代理对象，他持有http生命周期的bean，这样就可以通过代理bean去访问了。

        <bean id="userPreferences" class="com.foo.UserPreferences" scope="session">
            <aop:scoped-proxy/>
        </bean>

        <bean id="userManager" class="com.foo.UserManager">
            <property name="userPreferences" ref="userPreferences"/>
        </bean>

  代理的默认设置是使用cglib代理，他代理只会拦截公共方法也就是public方法的调用，如果调用非public的方法 是无法把调用数据委托给真实的bean对象的。

  不过我们也可以使用java自带的代理 只需要把proxy-target-class这个属性设为false就可以了。

        <!-- DefaultUserPreferences implements the UserPreferences interface -->
        <bean id="userPreferences" class="com.foo.DefaultUserPreferences" scope="session">
            <aop:scoped-proxy proxy-target-class="false"/>
        </bean>

        <bean id="userManager" class="com.foo.UserManager">
            <property name="userPreferences" ref="userPreferences"/>
        </bean>

   不过要代理的生命周期的bean必须至少实现了一个接口，并且所有的依赖必须通过他的一个接口去引用这个bean。

# 生命周期回调函数
## InitializingBean
  这个接口可以在bean初始化时能够回调afterPropertiesSet()方法
## DisposableBean
  这个接口可以在beandestory时回调destroy()方法
## BeanPostProcessor
  spring内部通过这个接口去处理任何的回调函数，并调用适当的方法。如果你想添加一些自定义的行为，
  那么你需要自己去实现一个beanpostprocessor接口

## 初始化
  初始化本来可以使用InitializingBean接口的afterPropertiesSet方法回调去做一些bean初始化时的一些自定义操作，
  不过spring并不建议这么做，因为他会把一些不必要的代码耦合到spring中。
  我们可以使用init-method去指定一个没有返回值没有参数的初始化方法来做些操作。

        <bean id="exampleInitBean" class="examples.ExampleBean" init-method="init"/>

  java config方式可以使用@Bean注解的initMethod属性

## 初始化销毁的执行优先级
1.  @PostConstruct和@PreDestory()标注的方法
2.  InitializingBean的afterPropertiesSet()和DisposableBean的destroy()
3.  自定义的init()

## 对bean的初始化等等做一些自定义处理使用BeanPostProcessor
  这个接口中有两个回调函数，我们可以自己去实现这个接口，然后去做一些自定义操作。
  post-processor会在容器初始化之前调用，并且是在applicationContext的一些初始化的方法之前调用。
  addBeanPostProcessor 可以使用这个方法在使用代码形式的配置中使用。

