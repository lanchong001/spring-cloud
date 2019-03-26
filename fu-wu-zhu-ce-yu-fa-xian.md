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

### Eureka Server高可用\(集群\)

* eureka server 相互注册
* 两台eureka server 1, eureka server 2 相互注册

```
# Server 1 注册到 Server 2  (Server1 Server.Port=8761)

eureka:
  client:
    service-url:
      defaultZone: "http://localhost:8762/eureka/"
```

```
# Server 2 注册到 Server 1 (Server2 Server.Port=8762)
eureka:
  client:
    service-url:
      defaultZone: "http://localhost:8761/eureka/"
```

```
# eureka client 配置(配置多个注册中心)
eureka:
  client:
    service-url:
# 配置eureka 注册中心地址
      defaultZone: http://localhost:8761/eureka/,http://localhost:8762/eureka/
```

* 三台以上的eureka server服务器时，每一台服务器分别注册到除自己以外的其他机器中注册。 eureka client 则注册到所有的eureka server 服务器中

### Eureka Client 分组

* 通过将spring.application.name设置不同的应用名称来打包应用分组的目的，便于管理不同的区域，不同的角色，访问不同的 Eureka Client 客户端程序

### Eureka总结

* @EnableEureKaServer ，@EnableDiscoveryClient ，@EnableEurekaClient
* 心跳检测，健康检查，负载均衡
* Eureka的高可用，生产环境建议至少部署两台以上
* 分布式系统中，服务注册中心是最重要的基础部分

### @EnableDiscoveryClient与@EnableEurekaClient区别

* @EnableDiscoveryClient基于spring-cloud-commons。 如果使用其他注册中心（eureka、consul、zookeeper），推荐使用 @EnableDiscoveryClient注解

* 如果选用的注册中心是eureka，那么就推荐@EnableEurekaClient

### 分布式服务发现的两种方式

* 客户端发现（Eureka,Zookeeper）
* 服务端发现（Nginx,Kubernetes）

### ZooKeeper 与 EureKa 的区别

* Zookeeper 会选举一个leader，由leader同步到其他机器，并且会保存相关的日志
* Eureka没有中心的概念,各个Server之间相互进行注册，不会保存日志信息

[阿里巴巴为什么不用 ZooKeeper 做服务发现？](http://jm.taobao.org/2018/06/13/%E5%81%9A%E6%9C%8D%E5%8A%A1%E5%8F%91%E7%8E%B0%EF%BC%9F/ "阿里巴巴为什么不用 ZooKeeper 做服务发现？")

### 客户端发现（Eureka）缺点

* 缺点：如果采用非java语言进行开发的话,需要其他语言自己实现Eureka的客户端程序（比如： nodejs的eureka-js-client）



