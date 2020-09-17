## Spring Boot

### Question

* Spring Boot 自动配置原理是什么？
1. @SpringBootApplication
    * @SpringBootConfiguration
    * @EnableAutoConfiguration 载入所有默认配置 
    * @ComponentScan 包扫描

`@EnableAutoConfiguration` 原理
1. @AutoConfigurationPackage 扫描所有的@Entity
2. @Import(AutoConfigurationImportSelector.class) 
    Spring boot会扫描所有META-INFO/spring.factories里的配置，将(????存到容器的哪个类里)
  

* Spring Boot 配置加载顺序？

* Spring Boot starter

* spring-boot-starter-parent

* spring-boot-dependencies

* Spring Boot生成的jar (fat jar)

* Spring Boot异常处理 ControllerAdvice






