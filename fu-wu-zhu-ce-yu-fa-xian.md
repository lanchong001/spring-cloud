# Spring Cloud Eureka

* Eureka Server 注册中心
* Eureka Client 服务注册



### Enreka 服务注册

1.  创建spring boot 项目，选择 Eureka Server 进行创建
2.  spring boot 自动应用以下依赖包

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



