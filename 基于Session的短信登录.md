---
tags:
  - Redis实践项目-黑马点评
---
[[Session]]是基于[[Cookie]]的，每一个session(tomcat的内存空间)都有一个唯一的sessionID，保存在浏览器的cookie中。浏览器向tomcat服务器发送请求时会带着cookie，从而能通过其中的session ID 找到 session 
![[CleanShot 2024-02-16 at 12.40.15@2x.png]]
## 1. 在UserServiceImpl中实现基本的业务逻辑
这里注意使用[[基于接口编程的编程方式]]，先在接口中定义，再在实现类中实现！
  - sendcode()
  - login()
## 2. 使用拦截器实现对用户登录的校验
每次当业务需要校验登录时，都要进行一遍校验登录。
可以直接**在进入controller前使用拦截器来校验登录**，防止每次进入后再校验。
![[CleanShot 2024-02-16 at 14.40.50@2x.png]]
1. 写拦截器：在Util下新建LoginInterceptor类，拦截器需要实现 HandlerInterceptor 接口。有三个方法可以实现： preHandle, postHandle, afterCompletion
- 在preHandle写登录校验逻辑，注意要将user从session中取出，看是否有，作为是否登录了的凭证。然后为了线程安全（这块还没懂逻辑），将user存入ThreadHandler中，之后从这里取
- 在afterCompletion释放内存，防止泄露
2. 配置拦截器：在config下新建MvcConfig类，实现WebMvcConfigurer

## 3. 保护隐私，创建UserDTO类
前面实现是将user的全部字段都会原封不动地保存入Session，拦截器校验时又会将其从session取出并存入ThreadLocal。其实不应该将User完整对象传递，因为会造成隐私泄露问题，**创建一个UserDTO类，只包含User中部分不敏感字段**。
- 采用hutool工具，将user对象转为新建UserDTO对象！
- [[DTO 设计模式通用概念]]