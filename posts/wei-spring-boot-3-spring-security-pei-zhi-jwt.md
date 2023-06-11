---
title: '为Spring Boot 3 +  Spring-Security 配置JWT'
date: 2023-06-10 18:47:21
tags: []
published: true
hideInList: true
feature: 
isTop: false
---

## JWT
JWT(JSON Web Token)是当前最流行的跨域身份验证解决方案。
它的工作原理是:
1. 客户端用用户名和密码请求服务器
2. 服务器验证通过后,生成一个JWT,返回给客户端
3. 客户端将JWT存储起来,并在每次请求时附带在Authorization头中
4. 服务器验证JWT的有效性,如果通过就允许请求


JWT的结构很简单,就是三段信息用.连接成一个字符串:
 - 头部(header):包含算法和token类型
 - 负载(payload):包含声明(claim),像用户名、过期时间等
 - 签名(signature):由header和payload加上secret使用指定的算法(如HMAC SHA256)计算得出
  
  例如:



```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ
```



- 头部: `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9`

- 负载: `eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9`

- 签名:`TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ`
  
  

使用JWT的优点:

1. 跨域身份验证:JWT可以在不同的域中传递,使用者无需再在每个域中维护一份用户信息。

2. 易于使用:JWT可以通过URL,POST参数或者在HTTP header发送。

3. 无状态:JWT是自包含的,无需在服务器端保存会话信息。

4. 轻量: JWT通常使用URL safe字符集,这使其可以很轻易地通过URL,POST参数或HTTP header传输,而不会被破坏。

所以,JWT适用于跨域身份验证场景,并能简化OAUTH2.0的使用。
