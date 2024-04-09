---
tags:
  - mysql
---
> 如果没有配置，使用navicat远程连接会有36错误！

如果你希望从本机的Navicat连接到MySQL服务器，需要确保MySQL服务器允许远程连接。打开MySQL的配置文件，一般位于`/etc/mysql/mysql.conf.d/mysqld.cnf`，并确保`bind-address`设置为`0.0.0.0`。