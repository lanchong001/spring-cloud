### Http协议——————————spring cloud

### RPC框架——————————Dubbo

---

### Spring Cloud 中服务间 restful 调用方式

* RestTemplate
* Feign

### RestTemplate使用的方式

* 方式1\(固定的url地址\)

```
        RestTemplate restTemplate = new RestTemplate();
        String reponse = restTemplate.getForObject("http://localhost:8080/msg",String.class);
        log.info(reponse);
        return reponse;
```

* 方式2

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



