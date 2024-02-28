---
tags:
  - springboot
  - mysql
  - error
---
项目运行时错误信息：
```error
Error querying database. Cause: org.springframework.jdbc.CannotGetJdbcConnectionException: Failed to obtain JDBC Connection; nested exception is java.sql.SQLException: null, message from server: "Host '10.211.55.2' is blocked because of many connection errors; unblock with 'mysqladmin flush-hosts'"
```
不仅Springboot运行出现以上报错，我在navicat远程连接也显示blocked

## 错误原因
>Host '10.211.55.2' is blocked because of many connection errors; unblock with 'mysqladmin flush-hosts'

这个错误表明在连接到 MySQL 数据库时发生了问题。具体而言，MySQL 数据库服务器拒绝了来自主机 '10.211.55.2' 的连接，原因是由于多次连接错误。在这种情况下，MySQL 服务器会自动阻止主机，以防止进一步的连接尝试，以保护服务器免受潜在的滥用或攻击。
#### 有几种可能的原因导致这个问题
1. **连接错误频繁：** 可能存在一些代码或配置问题，导致在尝试连接到数据库时出现频繁的错误。这可能是由于数据库连接池配置不当、连接没有正确关闭或其他应用程序问题引起的。
2. **网络问题：** 在网络层面可能存在问题，导致连接超时或连接错误。这可能是由于网络中断、防火墙配置不正确或其他网络问题引起的。
3. **数据库配置问题：** 可能存在一些数据库配置问题，导致连接错误。这可能包括数据库连接数限制、权限问题或其他数据库配置问题。


## 解决方式
在mysql服务器上使用 `mysqladmin` 工具，刷新主机以解除连接阻塞
```shell
sudo mysqladmin -h <ip> -u <mysql用户名> -p flush-hosts
# sudo mysqladmin -h 127.0.0.0 -u nihaoya -p flush-hosts
# 然后输入密码即可
```