# 可执行jar包
如果想构建一个可执行jar包，需要借助`maven-shade-plugin`插件并把带有main函数的类配置上。这样这个插件会把main函数的类信息放到MANIFEST中。

# 版本号
1.3.4 表示第一个重大版本的第三个次要版本的第四次增量版本
`<主版本>.<次版本>.<增量版本>-<里程碑版本>`

# maven内置属性
## 内置属性
${basedir}： 表示项目根目录，即pom.xml所在的目录

${version}: 表示项目版本
## POM属性
${project.artifactId}: 对应的是<project>下的artifactId \n
${project.build.sourceDirectory}：项目主源码目录，默认是src/main/java