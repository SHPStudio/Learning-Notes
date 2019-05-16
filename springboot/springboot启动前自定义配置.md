# EnvironmentPostProcessor
通过实现这个接口可以修改特定的配置属性或者新加载一个配置文件。
1. 自定义类实现`EnvironmentPostProcessor`
2. 通过编码方式或者配置文件方式或者使用spring.factories把自定义的实现类配置到Listeners或者Initializers中
3. 例如使用spring.factories
`org.springframework.boot.env.EnvironmentPostProcessor=com.example.YourEnvironmentPostProcessor`

自定义实现:
```java
public class EnvironmentPostProcessorExample implements EnvironmentPostProcessor {

	private final YamlPropertySourceLoader loader = new YamlPropertySourceLoader();

	@Override
	public void postProcessEnvironment(ConfigurableEnvironment environment,
			SpringApplication application) {
		Resource path = new ClassPathResource("com/example/myapp/config.yml");
		PropertySource<?> propertySource = loadYaml(path);
		environment.getPropertySources().addLast(propertySource);
	}

	private PropertySource<?> loadYaml(Resource path) {
		if (!path.exists()) {
			throw new IllegalArgumentException("Resource " + path + " does not exist");
		}
		try {
			return this.loader.load("custom-resource", path).get(0);
		}
		catch (IOException ex) {
			throw new IllegalStateException(
					"Failed to load yaml configuration from " + path, ex);
		}
	}

}
```

