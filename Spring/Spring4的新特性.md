## 加入了对java8的支持
## 去掉了不用的包和class
  如果是从老版本的spring迁移到spring4的话，需要注意有些类或者包就可能被删掉了。
# web
## servlet3.0环境支持
## @RestController
 使用@RestController注解来代替在每个方法上加上的@ResponseBody和@RequestMapping
## 异步非阻塞
 `AsyncRestTemplate`这个类提供了非阻塞的异步支持。
## 全时区支持
# websocket、sockjs、stomp messaging
## spring-websocket
 `spring-websocket`模块提供了websocket基础的全面支持，能够实现客户端和服务端的双向通信。
 而且还提供了sockjs websocket仿真功能，用于不支持websocket协议的浏览器能够使用websocket
# web
## @JsonView
  这个注解直接支持了@ResponseBody和ResponseEntity，主要序列化pojo。
## 简化映射
  如果不加上@RequestMapping 那么就根据controller和method去默认指定一个url的映射。
  比如FooController下的handleFoo方法，应该就会默认映射为FC#handleFoo
# core container
## @Bean
  java8默认方法可以检测到@Bean，并可以通过@Bean去构造一个配置类。
## @AliasFor
  类似于重写注解。给注解起一个别名。
