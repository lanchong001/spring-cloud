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
@EnableConfigServer            //标记为注册中心    
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
@EnableDiscoveryClient		//增加EnableDiscoveryClient注解
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
```



