# Spring Cloud Eureka

* Eureka Server 注册中心
* Eureka Client 服务注册

### Enreka 服务注册

* 创建spring boot 项目，选择 Eureka Server 进行创建

* spring boot 自动应用以下依赖包

```
      <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.3.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

        <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
      </dependency>
```

* main 方法增加 @EnableEureKaServer 注解
* 修改项目配置文件  application.yml ,并增加相关配置

```
eureka:
  client:
    service-url:
      defaultZone: "http://localhost:8761/eureka/"
    register-with-eureka: false
spring:
  application:
    name: eureka
server:
  port: 8761
```





