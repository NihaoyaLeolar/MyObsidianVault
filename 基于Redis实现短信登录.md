---
tags:
  - Redis实践项目-黑马点评
---
之前使用的Session登录，存在两个问题
- 在我们的负载均衡配置下，存在**session共享问题**：多台Tomcat并不共享session存储空间，当请求切换到不同tomcat服务时导致数据丢失的问题。
- **登录攻击漏洞**：当让发送验证码和登录时的手机不一致 ———— 发送到自己的手机上，登录别人的手机。因为对比的都是本浏览器带过去的cookie，登录别人的手机号在校验时得到的session是一样的。
![[CleanShot 2024-02-26 at 16.11.41@2x.png]]
# 业务的逻辑
---
### 1. 发送验证码
验证码在Redis中的存储方式：以手机号为key，值为验证码，String类型
![[CleanShot 2024-02-26 at 16.44.41@2x.png]]

### 2. 校验登录
用户在Redis中的存储方式：以token（不用手机号，随机生成uuid来用）为key，值为user对象，Hash类型
**登录凭证不再是Cookie中的sessionID，这里使用token，让前端每次请求都携带token**
注意：tomcat不会把token自动写到浏览器中，所以要自己手动的把token返回给前端
![[CleanShot 2024-02-26 at 16.46.43@2x.png]]
# 具体操作
---
这里面的具体业务实现看似复杂，其实只要弄懂上面的设计，以及token的使用逻辑，还有与session的区别，就很好理解！具体的技术点就那么几个，主要是理解使用方式。
## 1. 将以上业务逻辑实现
- 修改UserServiceImpl实现类中
	- sendCode方法：将生成的验证码保存在Redis中，并设置有效期为2min
	- login方法：生成随机token，将登录的用户存储在Redis中，并设置有效期为30分钟

这里主要注意一个技术点，第一运行报错的：StringRedisTemplate对类型的规定，所以我们不能一股脑的传，记得做一些转化，成他规定的样子。（具体的之后再深入，现在先过一遍这么个事）
后面也用到了这个StringRedisTemplate存入List，也有相关大大小小问题，先[[浅浅理解StringDataRedis API的使用]]。
## 2. 修改拦截器
1. 以token实现登录校验
用户登录成功，会返回生成的token，这个token前端已经写好了处理：保存下来，之后进行其他需要登录校验的操作时，都会在请求头的authorization字段中。拦截器从请求头拿到token去进行校验！
2. 新增拦截器
- **RefreshTokenInterceptor**：只要用户进行了请求，就刷新token/user有效期，对一切路径生效。
	- 要像session一样，用户登录一次，有效期就重新计算！redis如果不手动更改ttl，则不会实现以上。所以想到在拦截器这里对user的ttl进行更新
- **LoginInterceptor**：拦截未登录的用户，对于需要登录的路径生效
![[CleanShot 2024-02-27 at 11.56.19@2x.png]]
**这里要注意拦截器是我们手动创建的，所以不能直接依赖注入RedisTemplate！到拦截器配置类中去注入！**
3. 配置拦截器的优先级
以上两个拦截器是有先后顺序的！根据[[拦截器优先级控制]]，修改order值来配置优先级!



# 总结
---
Redis代替session需要考虑的问题：
◆ 选择合适的数据结构
◆ 选择合适的key
◆ 选择合适的存储粒度

在开发时还遇到了[[Mysql connection blocked]]错误！