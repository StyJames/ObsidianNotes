# `@SpringBootApplication`
通过该注解引入了自动装配，该注解包含三个如下注解。
1. `@EnableAutoConfiguration` 在springBoot的主类上使用，负责启用自动装配功能
	1. `@AutoConfigurationPackage` 指定了默认的包的规则，将主程序所在的包及所有子包下的组件扫描到Spring容器中
		1. `@Import(AutoConfigurationPackages.Registrar.class)` 利用register给Spring容器中导入一系列组件
	2. `@Import(AutoConfigurationImportSelector.class)` 通过import导入AutoConfigurationImportSelector类， 然后通过该类的selectImports方法读取MATE-INF/spring.factories文件中配置的组件的全类名，并按照一定的规则过滤（即条件装配，`@Conditional`）掉不符合要求的组件的全类名，将剩余读取到的各个组件的全类名集合返回给IOC容器并将这些组件注册为bean
2. `@ComponentScan` 配置扫描路径，用于加载配置的bean
3. `@SpringBootConfiguration` 标注在某个类上，表示这是一个Spring Boot的配置类
