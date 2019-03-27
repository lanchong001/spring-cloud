```
java.sql.SQLException: Access denied for user ''@'10.8.0.194' (using password: NO)
```

### 错误问题解决办法：

* 原错误配置信息

```
  spring:
  application:
    name: product
  datasource:
     driver-class-name: com.mysql.cj.jdbc.Driver
     data-username: root
     data-password: 123456
     url: jdbc:mysql://10.249.0.25:3307/SpringCloud_Sell?useUnicode=true&characterEncoding=utf8
```

* 正确的配置信息

```
spring:
  application:
    name: product
  datasource:
     driver-class-name: com.mysql.cj.jdbc.Driver
     username: root
     password: 123456
     url: jdbc:mysql://10.249.0.25:3307/SpringCloud_Sell?useUnicode=true&characterEncoding=utf8
```



