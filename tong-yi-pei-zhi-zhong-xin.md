### 没有统一配置中心存在的问题？

* 不方便维护
* 配置内容的安全与权限
* 更新配置项目需要重启

### 统一配置中心架构

### ![](/assets/config.png)Config Server 配置中心

```
package com.lbx.springcloud.configserver;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
import org.springframework.cloud.config.server.EnableConfigServer;

@SpringBootApplication
@EnableDiscoveryClient            //注册到eureka
@EnableConfigServer               //标记为注册中心    
public class ConfigServerApplication {

    public static void main(String[] args) {
        SpringApplication.run(ConfigServerApplication.class, args);
    }
}
```

```
spring:
  application:
    name: config
  cloud:
    config:
      server:
        git:
          username: liuyuanming
          password: 12345678
          uri: http://10.252.0.29:19090/root/config-repo.git
#          配置本地配置目录
          basedir: E:\WorkSpaces\lbxframework\basedir
eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka/
```

application.yml

/{name}-{porfiles}.yml

/{lable}/{name}-{profiles}.yml

name:服务名称

profiles: 环境名称

lable：git分支\(branch\)

[http://localhost:8080/order-a.yml](http://localhost:8080/order-a.yml)

[http://localhost:8080/order-dev.yml](http://localhost:8080/order-dev.yml)

[http://localhost:8080/release/order-dev.yml](http://localhost:8080/release/order-dev.yml)

### Config Client 配置

```
@SpringBootApplication
@EnableDiscoveryClient        //增加EnableDiscoveryClient注解
public class ConfigClientApplication {

    public static void main(String[] args) {
        SpringApplication.run(ConfigClientApplication.class, args);
    }
}
```

重命名并创建bootstrap.yml

```
spring:
  application:
    name: order
  cloud:
    config:
      discovery:
        enabled: true
#        配置在eureka注册的服务ID
        service-id: CONFIG
#      配置运行环境(test:测试环境)
      profile: test
eureka:
  client:
    service-url:
      defaultZone: http://localhost:8762/eureka/
```

### Config Server 集群

直接启动多台Config Server,客户端通过 service-id,随机的在各台Config Server中获取对应的配置信息

> 访问 [http://localhost:9003/order-test.yml](http://localhost:9003/order-test.yml) 时，会同时访问 一下两个文件。并且，将两个配置文件的数据合并，再返回给Config Clieng 进行使用。因此，可以将通用的配置信息写到order.yml文件中，便于各个环境进行应用。

```
2019-04-02 16:28:07.797  INFO 11676 --- [nio-9003-exec-5] o.s.c.c.s.e.NativeEnvironmentRepository  : Adding property source: file:/E:/WorkSpaces/lbxframework/basedir/order-test.yml
2019-04-02 16:28:07.797  INFO 11676 --- [nio-9003-exec-5] o.s.c.c.s.e.NativeEnvironmentRepository  : Adding property source: file:/E:/WorkSpaces/lbxframework/basedir/order.yml
```

### 配置的动态刷新：

> 在gitlab中修改配置文件后,刷新Config Server 对应的配置信息的页面，相关的配置信息会发生变化。这时，如果刷新Config Client, Config Client对应的配置信息不会发生变化

### Spring Cloud Bus 自动刷新配置信息

![](/assets/spring cloud config bus.png)

---

### Spring Cloud Config Server 配置中心\(Spring Cloud Bus 自动刷新\)

* 创建spring boot 项目，选择 Cloud Discovery -&gt; Eureka Discovery, Could Config -&gt; Config Server 进行创建

* 在项目中引入以下依赖

```
        <!--spring cloud config server 依赖-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-config-server</artifactId>
        </dependency>
        <!--spring cloud eureka client 依赖-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <!--spring cloud bus rabbitmq 依赖-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-bus-amqp</artifactId>
        </dependency>
```

* Spring Boot 启动方法 main 方法增加  @EnableDiscoveryClient, @EnableConfigServer 注解
* 修改项目配置文件 application.yml ,并增加相关配置

```
# 配置应用名称
spring:
  application:
    name: config1
  cloud:
    config:
      server:
        git:
# gitlab 相关配置信息
          username: liuyuanming
          password: 12345678
          uri: http://10.252.0.29:19090/root/config-repo.git
# rabbitmq配置信息
  rabbitmq:
    host: 192.168.1.104
    port: 5672
    username: admin
    password: admin
    virtual-host: rabbit_vhost
# 允许/actuator/bus-refresh接口被外部调用
management:
  endpoints:
    web:
      exposure:
        include: "*"
# 注册中心配置信息
eureka:
  client:
    service-url:
      defaultZone: http://192.168.1.104:9999/eureka/
  instance:
    prefer-ip-address: true
    ip-address: 127.0.0.1
```

### Spring Cloud Config Client 客户端 \(Spring Cloud Bus 自动刷新\)

* 创建spring boot 项目，选择 Cloud Discovery -&gt; Eureka Discovery, Could Config -&gt; Config Client 进行创建

* 在项目中引入以下依赖

```
        <!--spring cloud config client 依赖-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-config</artifactId>
        </dependency>
        <!--spring cloud eureka client 依赖-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <!--spring cloud web 依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--spring cloud bus rabbitmq 依赖-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-bus-amqp</artifactId>
        </dependency>
```

* Spring Boot 启动方法 main 方法增加  @EnableDiscoveryClient 注解

* 修改项目配置文件 bootstrap.yml ,并增加相关配置

```
# 配置应用名称
spring:
  application:
    name: order
# config server 配置信息
  cloud:
    config:
      discovery:
        enabled: true
#       配置在eureka注册的confer server 服务ID
        service-id: config1
#       配置运行环境(test:测试环境)
      profile: test
# rabbitmq配置信息
  rabbitmq:
    host: 192.168.1.104
    port: 5672
    username: admin
    password: admin
    virtual-host: rabbit_vhost
# 注册中心配置信息
eureka:
  client:
    service-url:
      defaultZone: http://192.168.1.104:9999/eureka/
```

* 在需要获取配置信息的类中配置  @RefreshScope 注解

```
@RestController
@RefreshScope    //配置 Spring cloud bus refresh 刷新范围
public class EnvController {
    @Value("${env}")
    private String env;

    @GetMapping("print")
    public String print() {
        return env;
    }
}
```

---

### Spring Cloud Bus 自动刷新

1. 启动 spring cloud config server.\(前提是已经启动了spring cloud eureka 注册中心\)
2. 启动 spring cloud config client,动态获取配置信息
3. 修改 gitlab 上的相关配置
4. 配置修改后，刷新 config server 对应的相关页面，配置信息自动会获取最新的。但是，客户端的配置信息不对自动更新
5. 利用postMan 请求 config server 的相关地址（http://192.168.1.221:9001/actuator/bus-refresh），刷新配置信息
6. 再次刷新spring cloud config client 对应的页面时，配置信息就会自动更新了



