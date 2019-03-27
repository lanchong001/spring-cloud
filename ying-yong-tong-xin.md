### Http协议——————————spring cloud

### RPC框架——————————Dubbo

---

### Spring Cloud 中服务间 restful 调用方式

* RestTemplate
* Feign

### RestTemplate使用的方式

* 方式1\(直接使用RestTemplate ，url地址写死\)

```
        RestTemplate restTemplate = new RestTemplate();
        String reponse = restTemplate.getForObject("http://localhost:8080/msg",String.class);
        log.info(reponse);
        return reponse;
```

* 方式2\(使用 LoadBalancerClient 获取url地址,使用 RestTemplate 进行调用\)

```
    @Autowired
    private LoadBalancerClient loadBalancerClient;
```

```
        RestTemplate restTemplate = new RestTemplate();
        ServiceInstance serviceInstance = loadBalancerClient.choose("PRODUCT");        //"PRODUCT": eureka Application
        String url = String.format("http://%s:%s/msg",serviceInstance.getHost(),serviceInstance.getPort());
        String reponse = restTemplate.getForObject(url,String.class);
        log.info(reponse);
        return reponse;
```

* 方式3\(利用@LoadBalanced注解，可在RestTemplate 使用应用的名字进行访问\)

```
@Component
public class RestTemplateConfig {

    @Bean
    @LoadBalanced
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }
}
```

```
        String reponse = restTemplate.getForObject("http://PRODUCT/msg",String.class);
        log.info(reponse);
        return reponse;
```

---

### 客户端负载均衡：Ribbon, 使用到Ribbon的客户端

* RestTemplate
* Feign
* Zuul

### Ribbon软负载均衡：

* 服务发现
* 服务选择规则
* 服务监听

### Ribbon主要主件：

* ServerList：获取所有可用的服务列表
* IRule：通过rule规则选择对应的服务
* ServerListFilter：过滤相关服务列表

### 配置Ribbon规则：

```
PRODUCT：
  ribbon:
    NFLoadBalanceRuleClassName: com.netflix.loadbalancer.RandomRule
```



