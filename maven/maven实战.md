# 可执行jar包
如果想构建一个可执行jar包，需要借助`maven-shade-plugin`插件并把带有main函数的类配置上。这样这个插件会把main函数的类信息放到MANIFEST中。

# 版本号
1.3.4 表示第一个重大版本的第三个次要版本的第四次增量版本
`<主版本>.<次版本>.<增量版本>-<里程碑版本>`

# maven内置属性
## 内置属性
${basedir}： 表示项目根目录，即pom.xml所在的目录 <br/>
${version}: 表示项目版本

## POM属性
${project.artifactId}: 对应的是<project>下的artifactId<br/>
${project.build.sourceDirectory}：项目主源码目录，默认是src/main/java
...

## settings属性
所有以`settings`开头的属性都会引用settings.xml文件中xml元素的值，例如${settings.localRepository}就是指向用户本地仓库的地址的

## java系统属性
所有Java系统属性都可以使用maven属性引用，例如${user.home}指向用户目录。可以使用`mvn help:system`命令查看所有的Java系统属性

## 环境变量
所有