# idea
## vm配置文件
  主要配置文件在idea安装目录下的bin文件夹下

1. idea.exe.vmoptions

   这个是针对idea32位可执行程序的VM配置文件，可以配置idea运行时有关虚拟机的相关参数，配置运行的最大内存等等。
2. idea64.exe.vmoptions

   这个是idea64位的
3. idea.properties

   idea相关属性配置

**最好不要直接修改这几个vm配置文件，因为idea重装或者升级会导致修改失效，应该使用Help -> Edit Custom VM Options 和 Help -> Edit Custom Properties去配置**

## 修改vm配置文件
   一般32位机子最大内存也就是2G，所以调整空间很小，一般修改64位配置文件，修改方式通过idea里面的help菜单方式修改。
   1. -Xms128m，16 G 内存的机器可尝试设置为 -Xms512m
   2. -Xmx750m，16 G 内存的机器可尝试设置为 -Xmx1500m
   3. -XX:MaxPermSize=350m，16G 内存的机器可尝试设置为 -XX:MaxPermSize=500m
   4. -XX:ReservedCodeCacheSize=225m，16G 内存的机器可尝试设置为 -XX:ReservedCodeCacheSize=500m
## 修改自定义属性文件

1. idea.config.path=${user.home}/.IntelliJIdea/config，该属性主要用于指向 IntelliJ IDEA 的个性化配置目录，默认是被注释，打开注释之后才算启用该属性，这里需要特别注意的是斜杠方向，这里用的是正斜杠。
2. idea.system.path=${user.home}/.IntelliJIdea/system，该属性主要用于指向 IntelliJ IDEA 的系统文件目录，默认是被注释，打开注释之后才算启用该属性，这里需要特别注意的是斜杠方向，这里用的是正斜杠。如果你的项目很多，则该目录会很大，如果你的 C 盘空间不够的时候，还是建议把该目录转移到其他盘符下。
3. idea.max.intellisense.filesize=2500，该属性主要用于提高在编辑大文件时候的代码帮助。IntelliJ IDEA 在编辑大文件的时候还是很容易卡顿的。
4. idea.cycle.buffer.size=1024，该属性主要用于控制控制台输出缓存。有遇到一些项目开启很多输出，控制台很快就被刷满了没办法再自动输出后面内容，这种项目建议增大该值或是直接禁用掉，禁用语句 idea.cycle.buffer.size=disabled。

