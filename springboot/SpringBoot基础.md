# 占位符
  如果使用`spring-boot-starter-parent`统一管理spring相关依赖，它不仅管理了依赖，还有很多事先配置好的一些配置，
，其中需要注意的是配置文件的占位符,默认为Spring样式的占位符`${...}`, 因此Maven的一些占位符需要使用`@...@`(不过
可以使用Maven属性`resource.delimiter`覆盖掉它)

# 覆盖继承Spring依赖的版本
  我们一般只需要配置一个`spring-boot-starter-parent`依赖就可以了，它会帮助我们统一spring相关依赖版本。例如

  ```
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.17.RELEASE</version>
    </parent>
  ```

  但是如果我们要升高一些独立的依赖版本的话，可以在自己的项目中的`pom.xml`中定义相关依赖版本的属性。

  ```
    <properties>
        <spring-data-releasetrain.version>Fowler-SR2</spring-data-releasetrain.version>
    </properties>
  ```

# 覆盖非继承的Spring依赖
  我们还可能不想通过继承`spring-boot-starter-parent`,可能一些公司都有自己的parentPom,那么为了能达到一致的效果
我们可以通过自己写`<dependencyManagement>`来管理依赖项
  ```
     <dependencyManagement>
          <dependencies>
             <dependency>
                 <!-- Import dependency management from Spring Boot -->
                 <groupId>org.springframework.boot</groupId>
                 <artifactId>spring-boot-dependencies</artifactId>
                 <version>1.5.17.RELEASE</version>
                 <type>pom</type>
                 <scope>import</scope>
             </dependency>
         </dependencies>
     </dependencyManagement>
  ```

  但以这种方式要想覆盖其中某个依赖版本的话就不能使用定义属性覆盖的方法了，需要在`spring-boot-dependencies`前加入要替换的依赖的pom定位

  ```
    <dependencyManagement>
        <dependencies>
            <!-- Override Spring Data release train provided by Spring Boot -->
            <dependency>
                <groupId>org.springframework.data</groupId>
                <artifactId>spring-data-releasetrain</artifactId>
                <version>Fowler-SR2</version>
                <scope>import</scope>
                <type>pom</type>
            </dependency>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>1.5.17.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
  ```