# 可执行jar包
如果想构建一个可执行jar包，需要借助`maven-shade-plugin`插件并把带有main函数的类配置上。这样这个插件会把main函数的类信息放到MANIFEST中。