# Spring Cloud Eureka

* Eureka Server 注册中心
* Eureka Client 服务注册

### Enreka 注册中心

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
# 设置与Eureka Server 交互的地址
      defaultZone: "http://localhost:8761/eureka/"
# 是否将自己注册到Eureka Server， 默认为true(由于当前应用就是Eureka Server， 因此设为 false)
    register-with-eureka: false
# 是否从Eureka Server获取注册信息，默认为true(一个单点的 Eureka Server，不需要同步其他节点的数据，可以设为false)
    fetch-registry: false
# 禁用自我保护模式,不进行客户端不在线警告提醒
  server:
    enable-self-preservation: false
spring:
  application:
    name: eureka
server:
  port: 8761
```

### Eureka 服务注册

* 创建spring boot 项目，选择 Eureka Discovery进行创建。

* spring boot 自动应用以下依赖包

  * ```
        <parent>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-parent</artifactId>
            <version>2.1.3.RELEASE</version>
            <relativePath/> <!-- lookup parent from repository -->
        </parent>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
    ```

* main 方法增加 @EnableDiscoveryClient 注解

* 修改项目配置文件  application.yml ,并增加相关配置

```
eureka:
  client:
    service-url:
# 配置eureka 注册中心地址
      defaultZone: "http://localhost:8761/eureka/"
  instance:
# 设置eureka客户端域名
    hostname: clientName
server:
# 设置服务端口
  port: 8081
spring:
  application:
# 设置应用名称
    name: client
```



