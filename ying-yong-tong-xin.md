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



