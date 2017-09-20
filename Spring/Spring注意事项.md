# 依赖冲突
  如果使用了第三方的库，如果第三方的库依赖了老版的spring版本，那么如果自己不明确指定自己的spring版本，很可能会把
  老版本的spring引入进来，这样就可能造成问题。

  所以可以把`spring-framework-bom`放到dependencyManagement中，他可以让不管是直接还间接使用spring模块的让spring的版本一致。

        <dependencyManagement>
            <dependencies>
                <dependency>
                    <groupId>org.springframework</groupId>
                    <artifactId>spring-framework-bom</artifactId>
                    <version>4.3.11.RELEASE</version>
                    <type>pom</type>
                    <scope>import</scope>
                </dependency>
            </dependencies>
        </dependencyManagement>